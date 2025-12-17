* Enabling a service means, it start to run on boot without manually running it by us.
* start means starting the service manually




<hr> 
### running server with httpd (include semange nd firewall setup)
steps
1. edit config of httpd at 
	1. `sudo vi /etc/httpd/conf/httpd.conf`
	
	Things to change in  httpd.conf:
	Documentroot "$dir"
	<Directory "$dir">
	<directory "$dir">
		replace $dir with actual dir with path
2. give selinux dir perm to that $dir  (eg: /server)
		`semanage fcontext -a -t httpd_sys_content_t "/server(/.)?"`
		`chcon -RFvv /server`
		Now check them by running `ll -Z /` 
		make sure the context is set, 
3. Now create a html file under the dir (eg: /server)
	1. index.html file is loaded defaultly when site is opened, hence creating the html file with that name
	2. `echo "test subject " > /server/index.html` 
	3. check the seperm : `ll -Z /mepco` having the context 
4. now retsart the server, 
	`sudo systemctl restart httpd `
5. when curl servera/b it prints the content inside index.html, thats it
6. to access outside the vm, setting up firewall
	1. `firewall-cmd --permission --add-server=http`
	2. `firewall-cmd --reload`
	now the server works outside the vm, in foundation when curl, it will work!
	
<hr>


### Tuning Sys Perf

package name : tuned-adm
service name : tuned

available params: #active #recommend #off #verify #profile_mode #profile_info


<hr>

### Package Management

package : <b>rpm</b> --*updated*--> <b>yum </b> --*updated*--> <b>dnf</b>

dnf repo --? Repo ? a server where packages are there, so we can install packages from it.


<hr> 

### Compressions

Gunzip --> z
Bunzip --> j
XZ --> J

tar -cvf fname.tar dir/

c --> create
t --> view/list (instead of c)
x --> extract


<hr>

### Create own repo

* login as root user in  servera : `sudo -i`
* `dnf repo list all`
* `vi /etc/yum.repos.d/redhat.repo`
* `vi /etc/yum.repos.d/rhel_dvd.repo`
* Repo of base os --> [repo_link](http://content.example.com/rhel9.3/x86_64/dvd/BaseOS) (based on RHEL9.3)
* creating own repo : `vim /etc/yum.repos.d/laf.repo`
	* [noir]    #id
	* name=Core Package
	* baseurl=http://content.example.com/rhel9.3/x86_64/dvd/BaseOS
	* enabled =1
	* gpgcheck=0
	* 
	* [d4rk]    #id
	* name=Additional Package
	* baseurl=http://content.example.com/rhel9.3/x86_64/dvd/AppStream
	* enabled =1
	* gpgcheck=0  #GNU Privace Guard Check
* then update the repo
	* `dnf update`
	* `dnf repolist all`

<hr>

## Copy files btwn two system

#SCP
`scp src dst` :
				src is from the source with path, if its in current path, then name enough, 
				dst is for destination path, mostly ssh server, eg: 23bcs108@172.16.16.200:
				
#RSYNC
				rsync * serverb.lab.examole.com:/home/student/backup/
				same as scp
###### Difference between them? rsync synchronises between local and remote, while scp just does copy job, 


### Hostname --> /etc/hostname
#### vim /etc/chrony.conf
server 172.25.254.254 iburst