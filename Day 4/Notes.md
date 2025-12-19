[[RHEL/Day 4/Commands]]


* scp and rsync uses ftpd
* `vim /etc/chrony.conf` --> config file for 
#### To sync time with a server

1. `ping classroom` --> host should ping
2. `vim /etc/chrony.conf` --> config file
	1. add **server hostname iburst**
	2. `esc : x!`
3. systemctl restart chronyd
* `timedatectl list-timezone` --> list all time zones
#### Cron -- Scheduling

Minute   Hour       Day        Month    Day     Username   Command_to_execute
(0-59)    (0-23)     (1-31)      (1-12)       (0-6)    l4feer
\*/1 * * * * echo hi

above one executed every 1 second,  star slash num considered as all time, without month, it isc considered as second not minute

* when edit with username specific , then username column not needed, directly we use command, it only needed when we done in etc/crontab
Also this also possible,
\*/1 * * * Tue-Thu echo hi
then 
`sudo systemctl restart crond.service`  --> to restart services and it starts

<hr>

### Key based auth (SSH)


* gen key in host using `ssh-keygen`
* id_rsa --> private key
* id_rsa.pub --> public key
* ssh-copy-id l4feer@servera --> the keys are copied to the server, therefore, when login with tha same  user, it wont ask password as the pub keys are exchanged.


#### Temp dirs

/run/tmpfiles.d/
/tmp
/usr/tmp

#### Log files


/var/log
/var/log/secure* --> login log important
/var/log/boot* --> boot log
/var/log/cron* --> cron job log

/etc/rsyslog.conf  --> customize log settings, shows where log saves

/etc/rsyslog.d/debug.cinf
\*.debug      /var/log/message-debug.conf
systemctl restart rsyslog.service
logger -p user.debug "ownlog"

### journalctl --> used to see sys logs

`journalctl --since "2025-12-18 11:56:32" --until "2025-12-18 11:58:31"`
`journalctl --since "-1hour"
`journalctl _SYSTEM_UNIT=sshd.service` or `journalctl -u`
`journalctl _PID=3321`
`journalctl `
`journalctl --list-boots


#### LDAP -Lightweight Directory Access Protocol

ldap-server
many-user

#### Client USe: ->
autofs --> software/service

sudo dnf install autofs

sudo systemctl start autofs.service
sudo systemctl reload-or-restart autofs.service
reload  --> same pid,try to rectify
restart --> diff pid, restart

vim /etc/auto.master

/-         /etc/auto.misc
	/ -  --> represent for all user


vim /etc/auto.misc

/localhome/production5 -fstype=nsf4,rw,sync servera.lab.example.com:/user-homes/production5


/localhome/production5  --> client path
servera.lab.example.com:/user-homes/production5  --> server path



### Manual Mount 
temporary mount :

mount -t nfs -o rw,sync servera:/user-homes/production5 /localhome/production5
umount /localhome/production5

permanent mount:

/etc/fstab

servera:/user-homes/production5 /localhome/production5 nfs rw 0 0
mount -a
systemctl daemon-reload
mount -a
df -hT

lab start netstorage-nfs


NEW TRY:

/etc/fstab

serverb:shares/public /public nfs rw,sync 0 0 
mount /public


lab finish netstorage-nfs