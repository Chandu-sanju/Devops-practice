# Here you are learning how to mount user_data on sdb relatime

┌──(vcse@chnws456)-[~]
└─>>> ssh sydws604     # Take the ssh into the machine                                                                                                                                                      
vcse@sydws604 /u/vcse sudo su      # become root

[root@sydws604 vcse]# lsblk            # Check sdb is available or not


NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  100G  0 disk 
├─sda1   8:1    0  600M  0 part /boot/efi
├─sda2   8:2    0    1G  0 part /boot
├─sda3   8:3    0   16G  0 part [SWAP]
└─sda4   8:4    0 82.4G  0 part /
sdb      8:16   0  500G  0 disk ----- Not partitioned so need to create a partition

[root@sydws604 /]# mkdir -m 777 /user_data  # Create /user_data with 777 permissions
[root@sydws604 /]# fdisk /dev/sdb # Create the partition

 


Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
 
Command (m for help): n
Partition number (1-128, default 1): 
First sector (34-1048575966, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-1048575966, default 1048575966): 
Created a new partition 1 of type 'Linux filesystem' and of size 500 GiB.
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

 

[root@sydws604 /]# lsblk        # For cross check


NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  100G  0 disk 
├─sda1   8:1    0  600M  0 part /boot/efi
├─sda2   8:2    0    1G  0 part /boot
├─sda3   8:3    0   16G  0 part [SWAP]
└─sda4   8:4    0 82.4G  0 part /
sdb      8:16   0  500G  0 disk 
└─sdb1   8:17   0  500G  0 part 

[root@sydws604 /]# mkfs.xfs /dev/sdb1      # Format the file system with xfs

 


meta-data=/dev/sdb1              isize=512    agcount=4, agsize=32767935 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=0 inobtcount=0
data     =                       bsize=4096   blocks=131071739, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=63999, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
Discarding blocks...Done.

 

 

[root@sydws604 /]# blkid /dev/sdb1 # Cross check


/dev/sdb1: LABEL="/user_data" UUID="f5919a88-cd2c-4f99-b6bd-007abfc9f897" BLOCK_SIZE="512" TYPE="xfs" PARTUUID="4681375d-065a-534e-8b22-2fe82eda7dc8"

 

[root@sydws604 /]# sudo xfs_admin -L /user_data /dev/sdb1 # Create a label


writing all SBs
new label = "/user_data"

[root@sydws604 /]# vi /etc/fstab # add in fstab  (LABEL=/user_data /user_data xfs rw 0 0)

 

[root@sydws604 /]# mount -a  # mount it


mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.

[root@sydws604 /]# mount -a

 

[root@sydws604 /]# df -h   #User_data mounted


Filesystem         Size  Used Avail Use% Mounted on
devtmpfs            59G     0   59G   0% /dev
/dev/sdb1          500G  3.6G  497G   1% /user_data [root@sydws604 /]# 

mkdir -m 1777 /user_data/.tmp


 # Create temp dir inside /user_data with sticky bit permission

 

vcse@sydws604 /u/vcse ls -ld /user_data/.tmp # check the permissions


drwxrwxrwt 2 root root 6 Mar 24 09:11 /user_data/.tmp

[root@sydws604 /]# exit 