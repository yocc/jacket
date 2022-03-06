# fdisk

---



## 目录

[TOC]



## 总揽

```shell
# Disk 段落是一个物理磁盘, 名称为 /dev/sda, 在Linux下对 SCSI 和 SATA 设备是以 sd 命名的, a为第几个磁盘, a为第一个磁盘
# /dev/sda1, 1为分区. 
# 对于主启动记录 (MBR) 磁盘, 可以最多创建四个主分区(每块物理硬盘), 或最多三个主分区加上一个扩展分区. 在扩展分区内, 可以创建多个逻辑驱动器.
# 每块物理硬盘都分为若干个分区, 每个分区都有自己的文件系统. Windows为这些文件系统各自指定了一个字母. 不过 GNU/Linux 使用唯一的树形结构来管理文件, 而每个文件系统都挂载于树形结构的某个位置. 

[chenchen@grpc01 ~]$ sudo fdisk -lu

Disk /dev/sda: 314.6 GB, 314572800000 bytes, 614400000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a8afc

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    20971519     9436160   8e  Linux LVM
/dev/sda3        20971520   614399999   296714240   8e  Linux LVM

Disk /dev/mapper/centos-root: 312.4 GB, 312416927744 bytes, 610189312 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos-swap: 1073 MB, 1073741824 bytes, 2097152 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

[chenchen@grpc01 ~]$ 
[chenchen@grpc01 ~]$ sudo lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0  293G  0 disk 
├─sda1            8:1    0    1G  0 part /boot
├─sda2            8:2    0    9G  0 part 
│ ├─centos-root 253:0    0  291G  0 lvm  /
│ └─centos-swap 253:1    0    1G  0 lvm  [SWAP]
└─sda3            8:3    0  283G  0 part 
  └─centos-root 253:0    0  291G  0 lvm  /
sr0              11:0    1 1024M  0 rom  
[chenchen@grpc01 ~]$ 


```



```shell
# fdisk 为交互式对磁盘分区

[root@localhost /]# fdisk /dev/sda
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)

Command (m for help): n
Partition type:
   p   primary (2 primary, 0 extended, 2 free)
   e   extended
Select (default p): p
Partition number (3,4, default 3): 3
First sector (20971520-614399999, default 20971520):
Using default value 20971520
Last sector, +sectors or +size{K,M,G} (20971520-614399999, default 614399999):
Using default value 614399999
Partition 3 of type Linux and of size 283 GiB is set

Command (m for help): t
Partition number (1-3, default 3): 3
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
[root@localhost /]#




[root@localhost /]# fdisk -lu

Disk /dev/sda: 314.6 GB, 314572800000 bytes, 614400000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a8afc

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    20971519     9436160   8e  Linux LVM
/dev/sda3        20971520   614399999   296714240   8e  Linux LVM

Disk /dev/mapper/centos-root: 8585 MB, 8585740288 bytes, 16769024 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos-swap: 1073 MB, 1073741824 bytes, 2097152 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

[root@localhost /]# reboot

 SSH  192.168.56.5: session closed
Press any key to reconnect

 SSH  Connecting to 192.168.56.5
 SSH  Host key fingerprint:
 SSH   SHA256  118f4456929293134390b22050c76166847d834bff45465824d866cb621d6dc5
 SSH  Trying saved password
Last login: Fri Feb  4 04:25:47 2022 from 192.168.56.1
[chenchen@localhost ~]$ sudo -s
[root@localhost ~]# fdisk -lu

Disk /dev/sda: 314.6 GB, 314572800000 bytes, 614400000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a8afc

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    20971519     9436160   8e  Linux LVM
/dev/sda3        20971520   614399999   296714240   8e  Linux LVM

Disk /dev/mapper/centos-root: 8585 MB, 8585740288 bytes, 16769024 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos-swap: 1073 MB, 1073741824 bytes, 2097152 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

[root@localhost ~]# vgdisplay
  --- Volume group ---
  VG Name               centos
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <9.00 GiB
  PE Size               4.00 MiB
  Total PE              2303
  Alloc PE / Size       2303 / <9.00 GiB
  Free  PE / Size       0 / 0
  VG UUID               d4vkw1-KS9f-9Kxt-GJqV-ej9t-wqU7-0hv9ss

[root@localhost ~]# pvcreate /dev/sda3
  Physical volume "/dev/sda3" successfully created.
[root@localhost ~]# vgextend centos /dev/sda3
  Volume group "centos" successfully extended
[root@localhost ~]# lvextend /dev/mapper/centos-root /dev/sda3
  Size of logical volume centos/root changed from <8.00 GiB (2047 extents) to 290.96 GiB (74486 extents).
  Logical volume centos/root successfully resized.
[root@localhost ~]# xfs_growfs /dev/centos/root
meta-data=/dev/mapper/centos-root isize=512    agcount=4, agsize=524032 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=2096128, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 2096128 to 76273664
[root@localhost ~]# lvscan
  ACTIVE            '/dev/centos/swap' [1.00 GiB] inherit
  ACTIVE            '/dev/centos/root' [290.96 GiB] inherit
[root@localhost ~]# df -hT
Filesystem              Type      Size  Used Avail Use% Mounted on
devtmpfs                devtmpfs  908M     0  908M   0% /dev
tmpfs                   tmpfs     920M     0  920M   0% /dev/shm
tmpfs                   tmpfs     920M   17M  903M   2% /run
tmpfs                   tmpfs     920M     0  920M   0% /sys/fs/cgroup
/dev/mapper/centos-root xfs       291G  8.0G  284G   3% /
/dev/sda1               xfs      1014M  149M  866M  15% /boot
tmpfs                   tmpfs     184M     0  184M   0% /run/user/1000
[root@localhost ~]#
[root@localhost ~]# ll /dev/sd*
brw-rw----. 1 root disk 8, 0 Feb  5 01:01 /dev/sda
brw-rw----. 1 root disk 8, 1 Feb  5 01:01 /dev/sda1
brw-rw----. 1 root disk 8, 2 Feb  5 01:07 /dev/sda2
brw-rw----. 1 root disk 8, 3 Feb  5 01:07 /dev/sda3
[root@localhost ~]# sudo yum install -y redhat-lsb
[root@localhost ~]# lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.7.1908 (Core)
Release:        7.7.1908
Codename:       Core
[root@localhost ~]# cat /etc/issue
\S
Kernel \r on an \m

[root@localhost ~]# uname -a
Linux localhost.localdomain 3.10.0-1062.el7.x86_64 #1 SMP Wed Aug 7 18:08:02 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
[root@localhost ~]# cat /etc/redhat-release
CentOS Linux release 7.7.1908 (Core)
[root@localhost ~]#
```

```shell
低级格式化, 一般有厂家完成, 特别慢, 伤盘
fdisk, 仅将硬盘硬件分区, 4个主分区, 如果扩展, 需要占用一个主分区, 扩展并不存在, 而是要继续分逻辑分区
mkfs, 将分好的区, 进行创建文件系统, ext4, 这一步主要是在对应的磁盘分区上创建文件系统所需的super block和inode表（参考这篇文章）, mkfs.ext4 /dev/sda1
mount, 将文件系统挂载到系统上, 即, 加入到linux 管理中
1) 创建挂在点: mkdir /mnt/disk1
2) mount -t <type> <device> <mount point>
mount /dev/sdb1 /mnt/disk1
```





```shell
附2:
指令：fdisk
用途：观察硬盘之实体使用情形与分割硬盘用。

使用方法：
　　　　　　一、在 console 上输入 fdisk -l /dev/sda ，观察硬盘之实体使用情形。
　　　　　　二、在 console 上输入 fdisk /dev/sda，可进入分割硬盘模式。
　　　　　　　　1. 输入 m 显示所有命令列示。
　　　　　　　　2. 输入 p 显示硬盘分割情形。
　　　　　　　　3. 输入 a 设定硬盘启动区。
　　　　　　　　4. 输入 n 设定新的硬盘分割区。
　　　　　　　　　4.1. 输入 e 硬盘为[延伸]分割区(extend)。
　　　　　　　　　4.2. 输入 p 硬盘为[主要]分割区(primary)。
　　　　　　　　5. 输入 t 改变硬盘分割区属性。
　　　　　　　　6. 输入 d 删除硬盘分割区属性。
　　　　　　　　7. 输入 q 结束不存入硬盘分割区属性。
　　　　　　　　8. 输入 w 结束并写入硬盘分割区属性

eg:
格式化与分区
　　hd--IDE设备 sd--SCSI设备
　　fdisk -l /dev/sda 查看第一块硬盘分区情况
　　fdisk /dev/sdb 给第二块硬盘分区
　　command acton (m for help)：m #显示命令列表
　　a-设置可引导标志;b-设置卷标; d-删除一个分区; n-新建分区
　　p-显示分区信息; v-校验分区表;q-不存盘退出;w-存盘退出;t-改变分区类型
　　command acton (m for help)：n 新建分区
　　command action
　　e extended #扩展分区
　　p primary partition (1-4) #主分区
　　p #创建主分区
　　partition number (1-4):1 #创建第一个主分区
　　first cylinder (1-522,default 1):1 #起始柱面(第一个分区始终为1)
　　last cylinder or +size or +sizeM or +siezK(1-522,default 522): 10 #截止柱面(若522则整个硬盘分给了一个区)此分区大小是系统按照柱面大小自动计算出来的
　　command acton (m for help)：n
　　command action
　　e extended
　　p primary partition (1-4)
　　p
　　partition number (1-4):2 #创建第二个主分区
　　first cylinder (11-522,default 11):11
　　last cylinder ...(11-522,default 522): +100M #自定义分区大小
　　command acton (m for help)：n
　　command action
　　e extended
　　p primary partition (1-4)
　　e #创建扩展分区，注意一个磁盘只能创建一个扩展区
　　partition number (1-4):3
　　first cylinder (28-522,default 28):28
　　last cylinder ...(28-522,default 522):522 #将剩余空间全部分给扩展分区
　　扩展分区是不能直接使用的，必须在其上创建逻辑分区!
　　command acton (m for help)：n
　　command action
　　l logical (5 or over) #逻辑分区
　　p primary partition (1-4)
　　l
　　first sylinder (28-255,default 28):28 #在扩展分区里建逻辑分区
　　last cylinder ...(28-522,default 522):522 #柱面用尽，等于说只建一个逻辑分区
　　command acton (m for help)：w #保存退出
　　转换分区类型：
　　command acton (m for help)：t #转换分区类型
　　partition number (1-4):2 #选择第二个主分区
 　hex code (type L to list codes):82 #按L可列出分区类型所对应的编码
    可以使用 “partprobe” 命令，重新探测磁盘中分区清空,     #partprobe  /dev/sdb
　　格式化与挂载: (挂载目录可以自行创建也可指定存在的空目录)
　　mksf.ext3 /dev/sdb1 把第二块硬盘的第一个主分区格式化为ext3
　　mkswap /dev/sdb2 初始化swap区，此区不可格式化。
　　mount /dev/sdb1 /mnt/d #将第一个分区挂载到d这个目录
　　重启后自动挂载：vi /etc/fstab
　　添加：/dev/sdb1 /mnt/d ext3 default 0 0
　　
总结
以上所述是小编给大家介绍的Linux下的fdisk命令用法详解，希望对大家有所帮助，如果大家有任何疑问请给我留言，小编会及时回复大家的。在此也非常感谢大家对脚本之家网站的支持！
```

```shell
# 更推荐下面这种方式, 查看UUID通过命令: blkid 查看, 例如:
[chenchen@grpc01 ~]$ blkid
[chenchen@grpc01 ~]$ sudo blkid
/dev/sda1: UUID="5844f801-0e31-4932-afa2-cf3df7bfbacf" TYPE="xfs" 
/dev/sda2: UUID="EMC1gw-siEa-iTuC-vsJm-JWhd-adqs-5mP18U" TYPE="LVM2_member" 
/dev/sda3: UUID="9CqZjR-ga2b-Oasj-KyCZ-SSoR-ZNUF-O00oh9" TYPE="LVM2_member" 
/dev/mapper/centos-root: UUID="fb238c9b-eb33-4e3a-95a1-3d8dc178dcc8" TYPE="xfs" 
/dev/mapper/centos-swap: UUID="e9f0a081-087a-4e93-b5c1-f25c905f6d3d" TYPE="swap" 
[chenchen@grpc01 ~]$ 
```



## MBR区

```shell
参考: http://blog.chinaunix.net/uid-22915173-id-597702.html
MBR区
      MBR（Main Boot Record 主引导记录区）位于整个硬盘的0磁道0柱面1扇区。不过，在总共512字节的主引导扇区中，MBR只占用了其中的446个字节，另外的64个字节交给了 DPT（Disk Partition Table硬盘分区表），最后两个字节“55，AA”是分区的结束标志。这个整体构成了硬盘的主引导扇区。
    主引导记录中包含了硬盘的一系列参数和一段引导程序。其中的硬盘引导程序的主要作用是检查分区表是否正确并且在系统硬件完成自检以后引导具有激活标志的分 区上的操作系统，并将控制权交给启动程序。MBR是由分区程序（如Fdisk．exe）所产生的，它不依赖任何操作系统，而且硬盘引导程序也是可以改变 的，从而实现多系统共存。
    下面，我们以一个实例让大家更直观地来了解主引导记录:
    例：80 01 01 00 0B FE BF FC 3F 00 00 00 7E 86 BB 00 在这里我们可以看到，最前面的“80”是一个分区的激活标志，表示系统可引导；“01 01 00”表示分区开始的磁头号为01，开始的扇区号为01，开始的柱面号为00；“0B”表示分区的系统类型是FAT32，其他比较常用的有 04（FAT16）、07（NTFS）；“FE BF FC”表示分区结束的磁头号为254，分区结束的扇区号为63、分区结束的柱面号为764；“3F 00 00 00”表示首扇区的相对扇区号为63；“7E 86 BB 00”表示总扇区数为12289622。
```



## See Also

