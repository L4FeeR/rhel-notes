

USERS AND GROUP


1. usermod -L <user>
2.  usermod -U <user>
3. useradd -r -s /sbin/nologin <user>
4. usermod -u 8766 <user>
5. usermod -g 8977 <user>
6. id <user>
7. usermod -aG wheel <user>
8. groupadd 
9. getent group <group name>
10. getent passwd <user name>
11. chsh <user> 
12. chsh <usr> /sbin/nologin (nologin, sysuser)
13. ssh servera
14. su -
15. groupadd -g 8787 wipro
16. passwd <usr> --stdin
/etc/sudoers
	user    ALL=(ALL)    ALL
	 %group    ALL=(ALL) ALL
17.rht-vmctl start all