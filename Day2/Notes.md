
* workstation, server a, server b for practice
* 
Permissions: (chmod, chown, chgrp)
	
* r  (read) w (write) x (execute)
* u (user) g (group) o (other) = a (all)
* r -> 4
* w -> 2
* x -> 1
* null -> 0

* 0 -> null
* 1 -> x
* 2 -> w
* 3 -> wx
* 4 -> r
* 5 -> rx
* 6 -> rw
* 7 -> rwx

* default permission for dir -> rwx
* default permission for file -> rw
*  viewing dir needs x (execute) perm
* file execute perm -> programming files
* file perm when cretaed -> -rw-r--r--
* dir perm when cretaed -> drwxrwxr-x
* c -> /dev -> character files
* b -> block files
*  chmod  200 user
* ll -i  -> inode view in number
*  ll -z 
* chown for user change
* chgrp for grp change
* chown can be used to change both also

for giving defualt permission:
* umask -> always set the opposite
* for dir 7 - (numebr used in umask)
* for file 6 - (number in umask)

Special perm: uid, gid, stickybit
	Prevent deletion and renaming of a file
	chmod ug+s file
	s -> executable permission there
	S -> executable permission not there
	
	
* chmod o+s file not possible , because sticky bit not available for other always
* for others its t, then chmod o+t will work
	*t -> executable permission there
	T-> executable permission not there
	


*  /etc/login.defs -> default permission config
umask 022 is default,  but 027 and 077 can be considered as default

chmod 1700 file -> sets -rwx-------T
it sets sticky bit for others, hence reverse it adds
avialable are, 124,  they represetn 1-other 2-grp 4-user
then for user, chmod 4700 file, it sets rwS------ 


Links
changes made all across link files if they linked
hardlink - inode copy, if src delted then all deleted, 
shortlink - jsut shortcut, if src delted then link brokern

stat --> used to view statistical analysis of an file

getfacl --> get file access control list, used to dispay the owner file perm all, 

##  Process Managment

used to view process 

fg -> foreground the process
bg -> background the process


Security
Enforcing -> secure
Permissive -> non secure

### Systemctl
poweroff
reboot

Semanage