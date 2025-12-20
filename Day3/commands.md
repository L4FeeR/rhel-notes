1. `systemctl start all
2. `ssh servera`
3. `sudo dnf install httpd`
4. `sudo sytemctl is-enabled httpd`
5. `sudo systemctl enable httpd`
6. `sudo systemctl start httpd`
7. `sudo systemctl status httpd``
8. `sudo systemtctl is-active httpd`
9. `vi /var/www/index/index.html`  --> <i> Welcome nigga </i>
10. `curl servera `--> <i> Welcome nigga </i>
11. on browser (after firewall rel) --> http://servera.lab.example.com
12. `semange fcontext -l `
13. grep pattern file
	1. `grep edu /usr/share/dict/words`
	2. `grep ^edu /usr/passwd`  --> start with edu
	3. `grep education$  /usr/passwd` --> ending with education
	4. `grep ^edu$ /usr/share/dict` --> exact match edu
14. `getsebool -a ` --> list all bool vars
15. `getsebool mount_anyfile`  --> check automount on or off
16. `getsebool virt_use_usb `--> in vm, usb is mount or not
17. `setsebool virt_use_usb off `--> value change
18. `sestatus` --> SElinux status
19. `setsebool -P virt_use_usb off` --> P = permanent
20. `dnf install tuned`
21. `tuned-adm list`
22. `tuned-adm profile desktop` --> tuned for desktop (obtianed list via prev command)
23. `tuned-adm active` --> activated prof
24. `systemctl enable --now tuned` --> setting the tuned service on boot,(if reboot, automaticaaly service starts)hence, profiles are loaded at start
25. `dnf repolist all` 
26. `dnf list`
27. `dnf install firewall* mariadb postfix`
28. `dnf info postfix`
29. `dnf group list`
30. `dnf module list`
31. `dnf module install ruby`
32. `dnf group install "server with GUI"`
33. `dnf history`
34. `dnf history undo 12` 
35. `dnf remove pckgname`
36. `dnf clean`
37. `dnf localinstall pkgname`
38. `dnf makecache`
39. `dnf reinstall pckname`
40. `dnf groupinfo`
41. `dnf groupinstall`
42. `dnf upgrade`
43. `du -h /home`
44. `tar -cvzf b.tar.gz dir`
45. `tar -cvjf b1.tar.bz2 dir`
46. `tar -cvf b3.tar dir`
47. `tar -cvJf b4.tar.xz dir`
48. `tar -tvJf b4.tar.xz dir` --> listing
49. `systemctl list-units`
50. `systemctl list-units --type=service`
51. `systemctl list-units --type=target`
52. `systemctl list-units --all`
53. `systemctl list-unit-files`
54. `systemctl list-unit-files`
55. `systemctl status sshd.service`
56. `systemctl status chrond.service`
57. `systemctl status chronyd.service`
58. `systemctl disable sshd`
59. `scp src dst`
60. `scp fn1 fn2 serverb:/home/student/
61. `rsync * serverb.lab.examole.com:/home/student/backup/
62. `rsync -av *  dst`
63. `hostnamectl hostname noirNull.domain5.example.com`
64. `timedatectl`
65. `timedatectl set-ntp-false`
66. `chronyc sources -v`
67. 