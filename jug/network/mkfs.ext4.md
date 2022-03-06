# mkfs.ext4

---



## 目录

[TOC]



## 总揽

```shell
# 关于'文件系统'的三个易混淆的概念:
# 创建(mkfs): 以某种方式格式化磁盘的过程就是在其之上建立一个文件系统的过程. 创建文件系统时, 会在磁盘的特定位置写入关于该文件系统的控制信息.
# 注册: 向内核报到, 声明自己能被内核支持. 一般在编译内核的时侯注册; 也可以加载模块的方式手动注册. 注册过程实际上是将表示各实际文件系统的数据结构 struct file_system_type 实例化.
# 安装(mount): 也就是我们熟悉的 mount 操作, 将'文件系统'加入到 Linux 的根文件系统的目录树结构上; 这样文件系统才能被访问.
```



```shell
# Linux 使用 mkfs 命令来创建文件系统, 使用 mkswap 命令创建交换空间.
# mkfs 命令实际上是几个特定文件系统的命令的前缀, 比如面向 ext3 的 mkfs.ext3, 面向 ext4 的 mkfs.ext4 以及面向 ReiserFS 的 mkfs.reiserfs. 你的文件系统上安装的是什么文件系统支持？使用 ls /sbin/mk* 命令即可得到答案.

# mkfs, 将分好的区, 进行创建文件系统, ext4, 这一步主要是在对应的磁盘分区上创建文件系统所需的super block和inode表（参考这篇文章）, 
$ mkfs.ext4 /dev/sda1			# 格式化分区成 ext4
$ mkfs.ext4 -T largefile /dev/sda1
$ mkfs.ext3 /dev/sdb1			# 格式化分区成 ext3
$ mkfs.ext2 /dev/sdb1			# 格式化分区成 ext2
```

