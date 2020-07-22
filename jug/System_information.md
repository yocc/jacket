# System information

---

### LSB (Linux Standard Base) and Distribution information 基于 Linux 标准和发行信息

- \$ cat /etc/redhat-release
- \$ uname -a
- \$ lsb_release -a
- \$ cat /etc/issue

```
[chenchen@dev3_10.211.21.18 ~]$ cat /etc/redhat-release
CentOS Linux release 7.3.1611 (Core)
```

```
[chenchen@dev3_10.211.21.18 ~]$ uname -a
Linux dev5 3.10.0-514.el7.x86_64 #1 SMP Tue Nov 22 16:42:41 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

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

```
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

```
[chenchen@dev3_10.211.21.18 ~]$ cat /etc/issue
\S
Kernel \r on an \m
```
