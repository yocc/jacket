# NetworkManager

---



## 目录

[TOC]



## 总揽

```shell
1. 先关掉原来系统的网络服务, 名称待查
2. 配置启动 NetworkManager 网络服务, 并能开机启动

```



```shell
Linux里面有两套管理网络连接的方案：
1、/etc/network/interfaces（/etc/init.d/networking）
2、Network-Manager

两套方案是冲突的, 不能同时共存.
第一个方案适用于没有X的环境, 如: 服务器；或者那些完全不需要改动连接的场合.
第二套方案使用于有桌面的环境, 特别是笔记本, 搬来搬去, 网络连接情况随时会变的.

```

```shell
NetworManager
网络管理器(NetworManager)是检测网络、自动连接网络的程序。

NetworkManager 是 2004 年 RedHat 启动的项目, 皆在能够让Linux用户更轻松的处理现代网络需求, 尤其是无线网络, 能够自动发现网卡并配置IP地址.
RHEL7 以上同时支持 network.service 和 NetworkManager.service(简称NM).
默认情况下这2个服务都有开启, 但是因为 NetworkManager.service 当时的兼容性不好, 大部分人都会将其关闭.
但是在 RHEL 8/Centos 8 以上已废弃 network.service(默认不安装), 只能通过NetworkManager进行网络配置.
```

```shell
NetworkManager 主要管理2个对象: 
Connection（网卡连接配置） 和 Device（网卡设备）, 他们之间是多对一的关系(多connection对1device), 但是同一时刻只能有一个 Connection 对于 Device 才生效.

```

```shell
在 RHEL 8/Centos 8 有三种方法配置网络:
* 通过 nmcli connection add 命令配置, 会自动生成ifcfg文件
* 手动配置 ifcfg 文件, 通过nmcli connection reload 来加载生效
* 手动配置 ifcfg 文件, 通过传统 network.service 来加载生效
```

```shell
NetworManager 服务管理:
1. 在systemd里面，可以直接使用systemctl进行管理
启动: systemctl start NetworkManger
关闭: systemctl stop NetworkManager
重启: systemctl restart NetworkManager
开机启动: systemctl enable NetworkManger
查看是否开机启动: systemctl is-enabled NetworkManager
禁用开机启动: systemctl disable NetworkManager
[root@localhost 桌面]# chkconfig NetworkManager off
[root@localhost 桌面]# chkconfig --list NetworkManager

# e.g.:
[chenchen@grpc01 ~]$ chkconfig --list

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.
注: 此输出仅显示 SysV 服务, 不包括本机 systemd 服务. SysV 配置数据可能会被本机 systemd 配置覆盖.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.
如果要列出 systemd 服务, 请使用 "systemctl list-unit-files".
      要查看在特定目标上启用的服务, 请使用 "systemctl 列表依赖项 [target]".

netconsole      0:off   1:off   2:off   3:off   4:off   5:off   6:off
network         0:off   1:off   2:off   3:off   4:off   5:off   6:off
[chenchen@grpc01 ~]$

[chenchen@grpc01 ~]$ systemctl list-unit-files
UNIT FILE                                     STATE
NetworkManager-dispatcher.service             enabled 
NetworkManager-wait-online.service            enabled 
NetworkManager.service                        enabled
[chenchen@grpc01 ~]$

[chenchen@grpc01 ~]$ systemctl list-dependencies NetworkManager.service
NetworkManager.service
● ├─system.slice
● ├─basic.target
● │ ├─microcode.service
● │ ├─rhel-dmesg.service
......
● │ │ │ └─systemd-remount-fs.service
● │ │ └─swap.target
● │ │   └─dev-mapper-centos\x2dswap.swap
● │ └─timers.target
● │   └─systemd-tmpfiles-clean.timer
● └─network.target
lines 1-65/65 (END)
[chenchen@grpc01 ~]$

# 命令行工具: nmcli, 参看 nmcli.md 文件
```

```shell
# 已经废弃不用的方式
2. 在CentOS6里面就有这个服务, 可以用 service 和 chkconfig 里面进行管理 NetworkManager:
启动: service NetworkManager start
关闭: service NetworkManager stop
重启: service NetworkManager restart
开机启动: chkconfig NetworkManager on
禁用开机启动: chkconfig NetworkManager off

# e.g.:
[chenchen@dev3_10.211.21.18 ~]$ chkconfig --list

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

netconsole      0:off   1:off   2:off   3:off   4:off   5:off   6:off
network         0:off   1:off   2:on    3:on    4:on    5:on    6:off

xinetd based services:
[chenchen@dev3_10.211.21.18 ~]$ 
```



```shell
NetworkManager 有两种管理工具:
1. systemctl(全管)
2. service(启动关闭) 和 chkconfig(开机启动)

1.1. systemctl
$ man systemctl
systemctl list-units
systemctl list-unit-files
systemctl list-dependencies
systemctl list-dependencies NetworkManager.service
systemctl status|restart|start|stop|enable|is-enabled|disable NetworkManager   // enable 开机自动启动

2.1. service, 服务的启动和关闭
service NetworkManager start|stop|restart // 服务启动

2.2. chkconfig, 服务是否开机自动启动
chkconfig --list
chkconfig --list NetworkManager
chkconfig NetworkManager off // 开机不启动


```

