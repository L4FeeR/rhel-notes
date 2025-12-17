1. alt . -> previous commands last string

2. rht-vmctl start all 
3. rht-vmctl status all
4. ping workstation
5. ssh workstation
6. lab
7. lab start    (, logout form workstation)
8. ssh servera  (student@servera)
9. touch f1
10. touch f{1..10}
11. mkdir f{1..10}
12. chmod ugo-rw f1  #rem perm
13. chmod ugo+rw f1 #add perm
14. chmod u+rw, o+r f1 #mutliple perm in one comm
15. chmod 776 f1 #octal way
16. ll -i 
17. ll -z
18. chgrp grpname filename
19. sudo chown user:grp filename
20. umask 777
	* mkdir ls --> d--------
21. umask 666
	* touch 
22. chmod u+s file
23. cmod g+s file
24. chmod 4700 file ->  rwS------ 
25. chmod 1700 file
26. ln -s fileame shortcut
27. ll -d
28. ln fname hfname
29. stat fname
30. getfacl fname
31. ps aux
32. ps -U username 
33. top
34. fg
35. bg
36. jobs
37. ps -jt
38. ps -j
39. kill
40. pgrep 
41. pkill
42. sudo setenforce 1  #1-enf #0-per
43. sudo getenforce
44. vim  /etc/selinux/config
45. systemctl reboot, init , reboot
46. systemctl get-default   #single-user, multi, network, emergency, rescue
47. sudo set-default multi-user.target
48. startx
49. systemctl poweroff
50. ll -Z
51. semanage boolean -l
52. semanage port -l
53. chcon -t http_sys_content filename
54. restorecon -v file 
55. semanage fcontext -a -t http_sys_content_t '/mepco(/.*)?'
56. restorecon -RFvv file/folder
57. nvim /etc/httpd/conf/httpd.conf
58. systemctl enable --now httpd.service 
59. firewall-cmd --permanent --add-service=http
60. firewall-cmd --reload
61. curl url 
62. 