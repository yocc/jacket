# route

---



## 目录

[TOC]



## 总揽

```shell
[chenchen@grpc01 ~]$ route -nee
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface    MSS   Window irtt
0.0.0.0         10.0.3.2        0.0.0.0         UG    101    0        0 enp0s8   0     0      0
10.0.3.0        0.0.0.0         255.255.255.0   U     101    0        0 enp0s8   0     0      0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker   0     0      0
192.168.56.0    0.0.0.0         255.255.255.0   U     100    0        0 enp0s3   0     0      0
[chenchen@grpc01 ~]$ 
# Destination, 目的地址, 可以是主机地址、网络地址，常用的是网络地址
# Gateway： 网关地址，所有未知地址都会找网关，有网关统一转发，只有边缘网络才会配置网关，并且直连网络不需要配置网关
# Genmask：目的地址的子网掩码

# 0.0.0.0 已经不是一个真正意义上的IP地址了. 它表示的是这样一个集合, 所有不清楚的主机和目的网络。这里的“不清楚”是指在本机的路由表里没有特定条目指明如何到达。如果在网络设置中设置了缺省网关，那么Windows系统会自动产生一个目的地址为0.0.0.0的缺省路由。

参考RFC文档：
0.0.0.0/8 - Addresses in this block refer to source hosts on "this"
network. Address 0.0.0.0/32 may be used as a source address for this
host on this network; other addresses within 0.0.0.0/8 may be used to
refer to specified hosts on this network ([RFC1122], Section 3.2.1.3).
根据RFC文档描述, 它不只是代表本机, 0.0.0.0/8可以表示本网络中的所有主机, 0.0.0.0/32可以用作本机的源地址, 0.0.0.0/8也可表示本网络上的某个特定主机,
综合起来可以说0.0.0.0表示整个网络. 它的作用是帮助路由器发送路由表中无法查询的包. 如果设置了全零网络的路由, 路由表中无法查询的包都将送到全零网络的路由中去.

# 255.255.255.255 我理解首先这是一个主机地址, 最近网关内的广播地址, 广播给所有机器, 发给局域网内的所有主机

#系统缺少 route 等网络命令, 使用以下安装
$ sudo yum -y install net-tools
```



```shell
[chenchen@grpc01 ~]$ netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         gateway         0.0.0.0         UG        0 0          0 enp0s8
10.0.3.0        0.0.0.0         255.255.255.0   U         0 0          0 enp0s8
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.56.0    0.0.0.0         255.255.255.0   U         0 0          0 enp0s3
[chenchen@grpc01 ~]$ 
```

