1. `sudo su `
2. `vim /etc/chrony.conf`
3. `timedatectl list-timezones`
4. `timedatectl set-timezone Asia/Kolkata`
5. `timedatectl set-time "9:40"`
6. `vi /etc/crontab`
7. `crontab -lu l4feer
	 `49 9 * * * * touch ls`
8. `tail -f fname` -> show changes
9. `sudo systemctl restart crond.service`
10. `ssh-keygen`
11. `ssh-keygen -f l4feer`
12. `ssh-copy-id l4feer@servera`
13. `systemctl cat systemd-tmpfiles-clean.timer`
14. `journalctl`
15. `logger -p local7.notice 'Its a new logggggg'`
16. `systemctl restart rsyslog.service`
17. `journalctl -u sshd`
18. `journalctl -p err`
19. `journalctl -p debug`
20.  `journalctl sync today`
21. `locate fname`
22. `updatedb`
23.  `file LOCATION -type f/d/s/l -name PATTERN`
24. `file -user student`
25. `file -perm g+s`
26. `file / -size +100M`
27.  `file / -size -10M`
28.  `file / -size -10M -size +5M`
29. `find / -user student -exec cp -rvfp {} /root \;`
30. `lab start rhcsa-compreview3`  --> in workspace
31. `df -hT`
32.  `mount -t nfs -o rw,sync servera:/user-homes/production5 /localhome/production5`
33.  `umount /localhome/production5`
34.  `vim /etc/fstab`
35. `lab start netstorage-nfs`
36. `mount serverb:/shares/public /public`
37. `umount /public`
38. `mount | grep public`
39. `lab finish netstorage-nfs`