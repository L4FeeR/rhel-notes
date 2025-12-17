

# SELinux: Complete Guide

## 1. **What is SELinux?**

### Overview

**SELinux (Security-Enhanced Linux)** is a mandatory access control (MAC) security mechanism implemented in the Linux kernel. It was developed by the NSA and provides fine-grained access control beyond traditional Linux permissions.

### Core Concepts

#### **Security Context (Label)**

Every process, file, port, and system object has a security context with the format:

Code

```
user:role:type:level
```

**Example:**

bash

```
system_u:object_r:httpd_sys_content_t:s0
```

- **user** (`system_u`): SELinux user
- **role** (`object_r`): SELinux role
- **type** (`httpd_sys_content_t`): The most important field - defines what can access what
- **level** (`s0`): Multi-Level Security (MLS) level

#### **Type Enforcement (TE)**

The primary SELinux model where **types** define access rules:

- Processes run with a **domain type** (e.g., `httpd_t`)
- Files have a **file type** (e.g., `httpd_sys_content_t`)
- Policies define which domains can access which types

---

## 2. **SELinux Modes**

SELinux operates in three modes:

|Mode|Description|Use Case|
|---|---|---|
|**Enforcing**|SELinux policy is enforced; violations are blocked and logged|Production environments|
|**Permissive**|Policy violations are logged but NOT blocked|Troubleshooting and policy development|
|**Disabled**|SELinux is completely turned off|Not recommended|

### Commands

bash

```
# Check current mode
getenforce

# Set mode temporarily
setenforce 0  # Permissive
setenforce 1  # Enforcing

# Permanent change (edit /etc/selinux/config)
SELINUX=enforcing  # or permissive or disabled
```

---

## 3. **semanage - Policy Management Tool**

### Purpose

`semanage` is the **policy management** tool that makes **persistent changes** to SELinux policy configuration.

### Key Features

- Manages boolean settings
- Manages file context mappings (policy database)
- Manages port contexts
- Manages user mappings
- Changes persist across reboots

### Common Operations

#### **A. File Context Management**

bash

```
# View file context policy for a path
semanage fcontext -l | grep '/var/www'

# Add new file context rule
semanage fcontext -a -t httpd_sys_content_t "/web/html(/.*)?"
# -a: add
# -t: type
# (/.*)?: regex for directory and all contents

# Modify existing context
semanage fcontext -m -t httpd_sys_rw_content_t "/web/upload(/.*)?"

# Delete context rule
semanage fcontext -d "/web/html(/.*)?"

# Apply the policy changes (use restorecon after)
restorecon -Rv /web/html
```

#### **B. Port Management**

bash

```
# List all port contexts
semanage port -l

# Add port to a service type
semanage port -a -t http_port_t -p tcp 8080

# Modify port
semanage port -m -t http_port_t -p tcp 8080

# Delete port
semanage port -d -t http_port_t -p tcp 8080
```

#### **C. Boolean Management**

bash

```
# List all booleans
semanage boolean -l

# Set boolean persistently
semanage boolean -m --on httpd_can_network_connect
semanage boolean -m --off httpd_can_network_connect

# Alternative command
setsebool -P httpd_can_network_connect on
# -P: persistent (writes to policy)
```

#### **D. Login/User Management**

bash

```
# List user mappings
semanage login -l

# Add user mapping
semanage login -a -s user_u username
```

---

## 4. **restorecon - Restore File Contexts**

### Purpose

`restorecon` applies the **default SELinux context** from the policy database to files and directories.

### How It Works

1. Reads file context rules from the policy (stored in `/etc/selinux/targeted/contexts/files/`)
2. Compares current file contexts with policy
3. Applies correct contexts based on policy

### Common Usage

bash

```
# Restore context for a single file
restorecon /var/www/html/index.html

# Restore recursively
restorecon -R /var/www/html

# Verbose output (show changes)
restorecon -v /var/www/html/index. html

# Recursive + Verbose (most common)
restorecon -Rv /var/www/html

# Show what would change without applying
restorecon -Rvn /var/www/html
# -n: dry run

# Force reset (even if contexts match)
restorecon -F /path/to/file
```

### When to Use

- After copying/moving files to new locations
- After creating new files in specific service directories
- After using `semanage fcontext` to add/modify rules
- When troubleshooting permission issues

### Example Workflow

bash

```
# 1. Add policy rule
semanage fcontext -a -t httpd_sys_content_t "/web/html(/.*)?"

# 2. Apply the rule to actual files
restorecon -Rv /web/html
```

---

## 5. **chcon - Change Context Temporarily**

### Purpose

`chcon` **temporarily changes** file contexts. Changes are NOT persistent and will be reset by:

- `restorecon`
- System relabeling
- File system operations (copy, backup/restore)

### Syntax

bash

```
chcon [options] context file
```

### Common Usage

bash

```
# Change type only (most common)
chcon -t httpd_sys_content_t /var/www/html/file.html

# Copy context from another file
chcon --reference=/var/www/html/index.html /var/www/html/newfile.html

# Change entire context
chcon -u system_u -r object_r -t httpd_sys_content_t file.html

# Recursive change
chcon -R -t httpd_sys_content_t /var/www/html

# Verbose
chcon -v -t httpd_sys_content_t file.html
```

### When to Use (Rarely!)

- **Quick testing** to see if SELinux context is the issue
- **Temporary fixes** before implementing proper policy
- **Debugging** - verify if changing context solves the problem

### ⚠️ Warning

**DON'T use `chcon` for permanent changes!** Always use:

bash

```
semanage fcontext + restorecon
```

---

## 6. **Comparison Table**

|Tool|Purpose|Persistence|Use Case|
|---|---|---|---|
|**semanage**|Modify SELinux policy database|✅ Persistent|Set rules for file contexts, ports, booleans|
|**restorecon**|Apply policy contexts to files|✅ Persistent|Restore correct contexts based on policy|
|**chcon**|Change file context directly|❌ Temporary|Quick testing only|

---

## 7. **Practical Workflow Examples**

### **Example 1: Setting up Custom Web Directory**

bash

```
# Scenario: Move web root from /var/www/html to /web/html

# 1. Check current context
ls -Z /var/www/html
# drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t: s0 /var/www/html

# 2. Create new directory
mkdir -p /web/html

# 3. Check its context (wrong!)
ls -Z /web
# drwxr-xr-x. root root unconfined_u:object_r:default_t:s0 html

# 4. Add policy rule
semanage fcontext -a -t httpd_sys_content_t "/web/html(/.*)?"

# 5. Verify rule was added
semanage fcontext -l | grep '/web/html'

# 6. Apply context
restorecon -Rv /web/html

# 7. Verify
ls -Z /web
# drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 html
```

### **Example 2: Custom SSH Port**

bash

```
# Change SSH to port 2222

# 1. Check current SSH ports
semanage port -l | grep ssh
# ssh_port_t  tcp  22

# 2. Add new port
semanage port -a -t ssh_port_t -p tcp 2222

# 3. Verify
semanage port -l | grep ssh
# ssh_port_t  tcp  2222, 22

# 4. Update SSH config
vim /etc/ssh/sshd_config
# Port 2222

# 5. Restart SSH
systemctl restart sshd
```

### **Example 3: Troubleshooting Access Denied**

bash

```
# 1. Check for SELinux denials
ausearch -m avc -ts recent

# Or use audit2why
grep denied /var/log/audit/audit.log | audit2why

# 2. Test with permissive mode
setenforce 0
# Try operation again
setenforce 1

# 3. Check file context
ls -Z /path/to/file

# 4. Check what it should be
matchpathcon /path/to/file

# 5. Fix if needed
restorecon -v /path/to/file

# 6. If custom location, add policy
semanage fcontext -a -t correct_type_t "/custom/path(/.*)?"
restorecon -Rv /custom/path
```

---

## 8. **Best Practices**

### ✅ Do's

1. **Always use `semanage + restorecon`** for permanent changes
2. **Check audit logs** when troubleshooting: `/var/log/audit/audit. log`
3. **Use permissive mode** temporarily for debugging
4. **Document custom policies** for team knowledge
5. **Test booleans** before making permanent: `setsebool httpd_can_network_connect on` (test), then `setsebool -P` (permanent)

### ❌ Don'ts

1. **Don't disable SELinux** unless absolutely necessary
2. **Don't use `chcon`** for permanent changes
3. **Don't ignore AVC denials** - they indicate security policy violations
4. **Don't modify policy files directly** - use `semanage`

---

## 9. **Helpful Commands**

bash

```
# View file context
ls -Z /path/to/file

# View process context
ps -eZ | grep httpd

# Check what context a path should have
matchpathcon /var/www/html

# Generate policy module from audit log
audit2allow -a -M mypolicy
semodule -i mypolicy.pp

# List loaded policy modules
semodule -l

# Check boolean status
getsebool -a | grep httpd

# Search audit log for denials
ausearch -m avc -ts today
```

---

## 10. **Decision Tree**

Code

```
Need to change SELinux context?
│
├─ Temporary/Testing? 
│  └─ Use:  chcon -t type_t /path
│
└─ Permanent?
   │
   ├─ Standard location?
   │  └─ Use: restorecon -Rv /path
   │
   └─ Custom location?
      └─ 1. semanage fcontext -a -t type_t "/path(/.*)?"
         2. restorecon -Rv /path
```