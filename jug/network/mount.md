# mount

---



## 目录

[TOC]



## 总揽

```shell
# 我们将 /dev/sda1, /dev/sda2 等等, 称为‘文件系统’. 因为他们最后是通过 mkfs.ext4 格式化而来的. ‘文件系统’通过格式化来创建
# 1. 在 Linux 系统的文件管理树的根(/)上, 必须挂载一个‘根文件系统’, 类似Win下的C:盘
# 2. 在 ‘根挂载点’ 挂载完成之后, 就可以继续挂载其他‘文件系统’了
# 3. 任何目录(必须目录)都可以做为‘挂载点’, 并且同一个‘文件系统’可以同时挂载到不同的‘挂载点’上
# 4. 一个分区挂载在一个已存在的目录上，这个目录可以不为空, 但挂载后这个目录下以前的内容将不可用. 对于其他操作系统建立的文件系统的挂载也是这样, 卸载后, 目录以前的文件都还在, 不会有任何丢失.



# mount, 将文件系统挂载到系统上, 即, 加入到linux 管理中
1) 创建挂在点: mkdir /mnt/disk1
2) mount -t <type> <device> <mount point>
mount /dev/sdb1 /mnt/disk1
```

## 例子

```shell
查看挂载的方法:
df -h
mount
mount -l        
/etc/mtab    开机挂载

mkdir -p /mnt/usbhd1				// -p, 递归
mount -t ntfs /dev/sdc1 /mnt/usbhd1

mkdir -p /mnt/usbhd2
mount -t vfat /dev/sdc5 /mnt/usbhd2

# mount -t ntfs -o iocharset=cp936 /dev/sdc1 /mnt/usbhd1        # 文件名如果是中文, 防止乱码
# mount -t vfat -o iocharset=cp936 /dev/sdc5 /mnt/usbhd2
# mount –o iocharset=gb2312 codepage=936 /dev/hda5 /mnt/hda5

mount -t 文件系统类型 -o 选项 要挂载的设备 dir设备在系统上的挂载点(mount point)(通常是一个目录)
-t      文件系统类型, nfs, 指定文件系统的类型, 通常不必指定. mount 会自动选择正确的类型
-o     挂载方式, loop 把一个文件当做硬盘分区挂载到系统上, 参数之间用半角逗号隔开
要挂载的设备
dir    挂载点, 必须要目录表示, 这个目录可以不为空，但挂载后这个目录下以前的内容将不可用，umount以后会恢复正常

```

```shell
# 开机启动自动挂载
vim /etc/fstab
#
# /etc/fstab
# Created by anaconda on Mon Jan  6 18:43:11 2020
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=5844f801-0e31-4932-afa2-cf3df7bfbacf /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0

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

```



## iso

```shell
# iso文件挂载
挂载命令:
mount [-t vfstype] [-o options] device dir
mount 是挂载命令
-t + 类型
-o + 属性
device iso的文件
dir 挂载的目录
详细的参数如下:
-t vfstype 指定文件系统的类型，通常不必指定。mount 会自动选择正确的类型。常用类型有：
　　光盘或光盘镜像：iso9660
　　DOS fat16文件系统：msdos
　　Windows 9x fat32文件系统：vfat
　　Windows NT ntfs文件系统：ntfs
　　Mount Windows文件网络共享：smbfs
　　UNIX(LINUX) 文件网络共享：nfs
-o options 主要用来描述设备或档案的挂接方式。常用的参数有：
　　loop：用来把一个文件当成硬盘分区挂接上系统
　　ro：采用只读方式挂接设备
　　rw：采用读写方式挂接设备
　　iocharset：指定访问文件系统所用字符集
device 要挂接(mount)的设备。
dir设备在系统上的挂接点(mount point)。
例如:
mount -t iso9660 -o loop hi-spider-isp6.10.iso /mnt/234/
将iso以硬盘模式挂载到 /mnt/234


$ mount -o loop ubuntu-18.04.2-live-server-amd64.iso /mnt/
```



## /etc/fstab 和 /etc/mtab 的区别

- /etc/fstab

> /etc/fstab 是开机自动挂载的配置文件, 在开机时起作用. 相当于启动linux的时候, 自动使用检查分区的 fsck 命令和挂载分区的 mount 命令, 检查分区和挂载分区都是根据 /etc/fstab 中记录的相关信息进行的.

- /etc/mtab

> /etc/mtab 是当前的分区挂载情况, 记录的是当前系统已挂载的分区. 每次挂载/卸载分区时会更新 /etc/mtab 文件中的信息(执行 mount 命令会改变 /etc/mtab 的信息).

- 区别

> /etc/fstab 是在开机时起作用, 相当于在开机时执行了 mount 和 fsck 命令, 系统根据 /etc/fstab 配置的信息自动挂载相关分区, 自动挂载之后，/etc/mtab 会被更新.
>  /etc/mtab 是当前分区的挂载信息, 如果挂载信息改变就会更新 /etc/mtab 文件. 开机后, 系统根据 /etc/fstab 的配置信息自动挂载分区, 再更新 /etc/mtab 中的信息.
>  mount 命令的使用不会改变 /etc/fstab, 而会改变 /etc/mtab.

综上所述:
. /etc/fstab 是记录开机自动挂载信息的配置文件, 开机时自动挂载是根据这个文件.
. 而 /etc/mtab 是记录当前系统的挂载信息, 每次系统挂载情况的改变都会更新/etc/mtab文件.



## umount

```shell
$ umount 挂载点dir

# umount -l /mnt/hda5
来卸载设备. 选项 –l 并不是马上umount, 而是在该目录空闲后再umount
还可以先用命令 ps aux 来查看占用设备的程序PID, 然后用命令kill来杀死占用设备的进程, 这样就umount的非常放心了

使用「fuser -m 挂载点」[3]列出正在使用装置挂载点目录以下档案的程序:
$ fuser -vm /media/cdrom0
fuser -k 挂载点  // 杀掉所有使用这个挂载点的进程

在 fuser 命令加上选项 -v 可以显示较详细的资讯：
$ fuser -vm /media/cdrom0
                     USER        PID ACCESS COMMAND
/media/cdrom0:       johndoe    6015 ..c.. bash
                     johndoe    6132 f.... rhythmbox

亦使用命令「ps auxw | grep PID」获知个别程序的详细资讯：
$ ps auxw | grep 6132
johndoe 6132 0.4 3.0   220017 57104 ?  S+ 18:27 0:00 rhythmbox
以上画面显示音乐播放程序 Rhythmbox 使用了光盘，您只需要关掉 Rhythmbox 或播放清单，就可以卸载或退出光盘。
除 fuser 外，亦使用命令 「lsof 挂载点」[4]列出正在使用装置挂载点目录以下档案的程序：
$ lsof /media/cdrom0
COMMAND  PID    USER   FD   TYPE DEVICE SIZE NODE NAME
bash    7531 johndoe  cwd    DIR   11,0 4096 1856 /media/cdrom0
lsof    7698 johndoe  cwd    DIR   11,0 4096 1856 /media/cdrom0
lsof    7699 johndoe  cwd    DIR   11,0 4096 1858 /media/cdrom0
以上画面表示有一个 bash 和两个 lsof 程序正在使用 /media/cdrom0，您需要令它们全部关闭 /media/cdrom0 才可以退出光盘。当然，直接结束相关程序是最简单或一般情况下唯一的方法。
此时您可能会发觉有些很讽刺的事情发生。超过一半的情况，您所使用的命令模式或终端机就是正在使用装置和阻止您退出卸载的程序，如上面画面就是这个程序，bash 程序就是正在使用的命令模式，而两个 lsof 程序就是刚为查询而输入的命令，它们合部都使用 /media/cdrom0 作为当前工作目录 (current working directory)，令系统拒绝卸载。解决方法就是改变工作目录至 /media/cdrom0 及以下以外的目录就可以了：
$ cd
$ pwd
/home/johndoe
$ eject cdrom0
$
很多情况下，我们需要终止有关程序才可以令它不再使用有关装置而让我们卸载或退出装置。这当然不是一个好方法，有关程序如未储存盘案，可能会遗失一些资料。但这却是最简单快捷的方法。建议初学者在无计可施时才好出此懒人的下策：
$ umount /media/sdb1
umount: /media/sdb1: device is busy
umount: /media/sdb1: device is busy
$ lsof /media/sdb1
COMMAND   PID    USER   FD   TYPE DEVICE SIZE NODE NAME
bash    67429 johndoe  cwd    DIR   11,0 4096 4257 /media/sdb1/documents
$ kill 67429
$ lsof /media/sdb1
COMMAND   PID    USER   FD   TYPE DEVICE SIZE NODE NAME
bash    67429 johndoe  cwd    DIR   11,0 4096 4257 /media/sdb1/documents
$ kill -9 67429
$ lsof /media/sdb1
$ umount /media/sdb1
$ umount /media/sdb1

```



## See also

https://www.cnblogs.com/sammyliu/p/4521315.html

https://www.jianshu.com/p/d39287b704d4
参考: http://wiki.linux.org.hk/w/Unmount/eject_filesystem
参考: http://man.lupaworld.com/content/other/Linux/linuxmanage/node46.html

http://blog.sina.com.cn/s/blog_659b48590100n2qf.html
