# System information

---



### 目录

[TOC]



### LSB (Linux Standard Base) and Distribution information 基于 Linux 标准和发行信息

- $ cat /proc/version, 查看内核版本
- $ cat /etc/redhat-release, 查看 CentOS 版本
- \$ uname -a, $uname -r, 显示系统名, 节点名称, 操作系统的发行版号, 操作系统版本, 运行系统的机器 ID 号, 显示操作系统的发行版号
- \$ lsb_release -a, LSB是Linux Standard Base的缩写, *lsb_release* 命令用来显示LSB和特定版本的相关信息
- \$ cat /etc/issue, /etc/issue文件是Linux系统开机启动时在命令行界面弹出的欢迎语句文件.

```shell
[chenchen@grpc01 ~]$ cat /proc/version
Linux version 3.10.0-1062.el7.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC) ) #1 SMP Wed Aug 7 18:08:02 UTC 2019
[chenchen@grpc01 ~]$ 
```

```shell
[chenchen@dev3_10.211.21.18 ~]$ cat /etc/redhat-release
CentOS Linux release 7.3.1611 (Core)
```

```shell
[chenchen@dev3_10.211.21.18 ~]$ uname -a
Linux dev5 3.10.0-514.el7.x86_64 #1 SMP Tue Nov 22 16:42:41 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
[chenchen@localhost ~]$ uname -r		// 查看内核版本
3.10.0-1062.el7.x86_64
[chenchen@localhost ~]$ uname -a
Linux localhost.localdomain 3.10.0-1062.el7.x86_64 #1 SMP Wed Aug 7 18:08:02 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux

Linux         内核名称
hathor69      网络中主机名称
2.6.32-431.11.2.el6.toa.2.x86_64       内核发行版本号
#1 SMP Wed Apr 9 15:55:33 CST 2014     内核的名称和编译的日期
x86_64        操作系统版本     x86_64 表示64位
x86_64        处理器类型
x86_64        硬件平台
GNU/Linux     操作系统
参考: man uname
```

```shell
[chenchen@localhost ~]$ lsb_release -a
sudo: lsb_release: command not found
[chenchen@localhost ~]$ sudo yum install -y redhat-lsb

[chenchen@localhost ~]$ lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 8.1.1911 (Core)
Release:        8.1.1911
Codename:       Core
```

```shell
[chenchen@dev3_10.211.21.18 ~]$ cat /etc/issue
\S
Kernel \r on an \m
```



### See Also

/etc/issue: https://www.jianshu.com/p/0260ef80d779

/etc/motd: https://www.cnblogs.com/pzk7788/p/10544340.html

/etc/issue.net: https://www.cnblogs.com/pluse/p/5531523.html
