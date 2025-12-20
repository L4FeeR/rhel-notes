
*  classroom --> 172.25.254.254   --> DNS server
* student --> 172.25.250.X 
* dns server, dns search domain addr, nearby server
* 172.25.250.X 
* 9 -> workstation
* 10 -> servera
* 11-> serverb
* 220 -> utility
* 254 -> classroom --> where the dnf packg repo there

`nmcli connection modify System\ eth0 ipv4.addresses 172.25.254.200/24 ipv4.gateway 172.25.250.254 ipv4.dns 172.25.254.254 ipv4.method manual autoconnect yes`

gateway --> bastion (172.25.250.254)
dns -->  classroom(172.25.254.254)

bastion is bridge between host and vm server connections , clasroom act as repo for pkg


#### Network manager

sudo vim /etc/NetworkManager/system-connections/System\ eth0.nmconnection




### Containers

CONTAINER IMAGE
CONTAINER REGISTRIES


	Container URL --> LOGIN --> 
	Container 

podman login registry.lab.example.com --tls-verify=false 
username: admin
passwd: redhat321


podman info
podman pull registry.lab.example.com/ubi9/ubi --tls-verify=false

podman run -rm registry.lab.example.com/ubi9/ubi echo "welcome baka"


podman ps -a

podman run -e name=lafeer reg... printenv name

podman pull registry.lab.example.com/rhel9/httpd-24 --tls-verify=false


podman run -p 8000:8080 -rm registry.lab.example.com/rhel9/httpd-24 echo "welcome baka" 

-d used to detach , run all times in bg
