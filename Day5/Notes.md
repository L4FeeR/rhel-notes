### Disk Managements
* Parted , GDisk , Fdisk , cfdisk --> available programs
* lsblk --> to view disk
* lsblk -fp --> same with dev path and info about fs
* blkid  --> id of block device

Statring the fs lab:
`lab start fs-mount`
`mount UUID="UUID_OF_PARTITION" /mnt`

* MBR --> Master Boot Record 
* GPT --> GUID Partition Table  --> Gpt Partition Scheme Method 

* GPT uses UEFI :
	UEFI -->  Unified Extensible Firmware Interface

* MBR support max upto 2TB
* MBR can upto 4 primary partition, 15 logical partition
* MBR partition table size is 32bit 
* GPT upto 128 paritions
* GPT partition table size is 64bit
* LVM in gpt , can create unlimited partitions
* GPT support upto Zebibytes(ZiB) or Eight Billio Teby Bytes(TiB)
* GUID --> Globally Uniquw IDentifier 
* parted --> used to view partitions
* msdos format --> MBR 
* gpt format --> GPT
inside parted:

parted 
NAME


### SWAP

* free
* parted /dev/vdb mklabel gpt
* parted /dev/vdb mkpart Swaps linux-swap 2049s 1G 
* mkswap /dev/vdb1
* 
* in fstab:
	* UUID=UID swap swap defaults 0 0 
* systemctl daemon-reload 
* swapon
* LVM --> Logical Volume Manager
	* Physical Volume(PVs)
	* Physical Devices
	* Volume Groups(VGs)
	* Logical Volumes(LVs)
	*
-->  *Commands*:
* pvcreate lvcreate vgcreate 
*  pvdisplay lvdisplay vgdisplay
* pvremove lvremove vgremove
*  pvextend lvextend vgextend
* `pvcreate`
* `pvs` or `pvdisplay`
* `pvremove`
* `vgcreate /dev/vdb /vdc`
* `vgcreate CSE /dev/vdb /dev/vdc`
* `vgs`
* `lvcreate -L 2G -n FirstYear CSE`
* `lvs`

IN ORDER:  available partition: /dev/vdb /dev/vdc /dev/vdd
* pvcreate /dev/vdb /dev/vdc /dev/vdd
* pvs --> to list them added to pvs
* vgcreate PERSONAL /dev/vdb /vdc  --> PERSONAL is label, vdc and vdb combined used
* vgcreate PUBLIC /dev/vdd
* vgs --> To display
* lvcreate -L 500M -n boot PERSONAL  --> 500MB partition with name boot on PERSONAL (from pv)
* lvremove /dev/PERSONAL/boot
* mkfs.ext4 /dev/PERSONAL/boot
* rht-vmctl reset servera
* mount /dev/PERSON/boot /boot

### test

parted /dev/vdc/ mklabel gpt
udevadm settle
parted /dev/vdc mkpart first  1M 257M 
parted  /dev/vdc mkpart sec  258M 514M
parted /dev/vdc set 1 lvm-on    --> setting lvm on on first partition created above
parted /dev/vdc set 2 lvm-on  
pvcreate /dev/vdc1 /dev/vdc2
vgcreate servera_group /dev/vdc1 /dev/vdc2
lvcreate -L 400M -n servera_vol1 server_group
xfs_grow  if pvscreate , vgextend and lvextend used



### Fix Linux Corrupt


repair filesystem at boot

ctrl + alt + del
then e
systemd.unit=emergency.target
redhat  --> passwd
mount 
mount -o remount,rw /
mount -a   --> gives any error on mounts 
in etc/fstab its there, just remove/comment it
