1. `lsblk -fp`
2.  `blkid`
3. `lab start fs-mount`
4. `mount UUID="UUID_OF_PARTITION" /mnt`
5. `parted`
6. `parted /dev/vda unit MB print`
7. `parted /dev/vdc mklabel msdos`
8. `udevadm settle ` --> finalise changes in partition
9. `parted /dev/vdc mklabel gpt`
10. `udevadm settle `
11. `parted /dev/vdc primary xfs 400M 600M`
12.  `parted /dev/vdb mklabel gpt`
13. `free`

--
14. `ip link show`
15. `ip addr`
16. `ip a s eth0`
17. `ip route`
18. `tracepath google.com`
19. `ss -t`
20. `nmcli connection show`
21. `nmcli dev status`
22.  `nmcli connection modify "System eth0 ipv4.addresses 172.25.255.110/24 ipv4.gateway 172.25.250.254 ipv4.dns 172.25.254.254 ipv4.method manual autoconnect yes`
23. 