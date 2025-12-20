
1. `nmcli connection modify System\ eth0 ipv4.addresses 172.25.254.200/24 ipv4.gateway 172.25.250.254 ipv4.dns 172.25.254.254 ipv4.method manual autoconnect yes`
2. `nmcli connection modify "System eth0" `
3. nmcli con mod
4. nmcli connection up "system eth0"
5. nmcli reload
6. nmcli connection show Wired\ Connection1