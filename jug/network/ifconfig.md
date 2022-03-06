# ifconfig

---



## 目录

[TOC]



## 概述

```shell
# 查看所有逻辑网卡
[chenchen@localhost ~]$ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:f7:89:a1:b1  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.56.5  netmask 255.255.255.0  broadcast 192.168.56.255
        inet6 fe80::4754:ee75:32a2:31e1  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:e4:48:5a  txqueuelen 1000  (Ethernet)
        RX packets 25757  bytes 2378356 (2.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 17309  bytes 1518310 (1.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.3.15  netmask 255.255.255.0  broadcast 10.0.3.255
        inet6 fe80::b7d1:73fd:a261:a6f  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:b4:2b:80  txqueuelen 1000  (Ethernet)
        RX packets 24  bytes 4216 (4.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 43  bytes 5912 (5.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[chenchen@localhost ~]$ 
```

```shell
# 查看指定的逻辑网卡
[chenchen@localhost ~]$ ifconfig enp0s3
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.56.5  netmask 255.255.255.0  broadcast 192.168.56.255
        inet6 fe80::4754:ee75:32a2:31e1  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:e4:48:5a  txqueuelen 1000  (Ethernet)
        RX packets 25875  bytes 2388428 (2.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 17388  bytes 1528560 (1.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[chenchen@localhost ~]$ 

# enp0s3, 逻辑网卡的名字
# ether 08:00:27:e4:48:5a, MAC地址
```

```shell
# 临时修改逻辑网卡配置, 重启后会失效
$ ifconfig enp0s3 192.168.56.56

# 永久配置, 修改文件
# vim /etc/sysconfig/network-scripts/ifcfg-enp0s3

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=da54e839-61b7-48d8-b424-cb4fac3ac92e
DEVICE=enp0s3
ONBOOT=yes
#USERCTL=no
#HWADDR=08:00:27:BD:69:D1
#NM_CONTROLLED=no
# 当 BOOTPROTO=static时, 需要配置以下三行, IP, 子网掩码, 网关
#IPADDR=192.168.10.10
#NETMAK=255.255.255.0
#GETWAY=192.168.10.0

$ systemctl restart network
```

```shell
# 设置网络接口的IP地址、子网掩码
格式1：ifconfig 接口名 ip地址 [netmask 子网掩码]
格式2：ifconfig 网络接口 ip地址 [/掩码长度]
# 禁用或者重新激活网卡
格式1：ifconfig 网络接口 up
格式2：ifconfig 网络接口 down
```

