# nmcli

---



## 目录

[TOC]





## 总揽

```shell
参数: 
d, 设备 device
c, 链接 connection
s, show 的缩写
status, 统计 status 全称, 没有缩写
d, 关闭 down
u, 启动 up

# d s, 的 s 是 show, 写全了是 status, 有 2 个 s 开头的; c s, 的 s 是 show, 没有 status

$ nmcli d s
$ nmcli c s

$ nmcli c d enp0s3 		# c, 链接名字; d, down
$ nmcli c u enp0s3 		# c, 链接名字; u, down


NetworkManager 主要管理2个对象: 
Connection（网卡连接配置） 和 Device（网卡设备）, 他们之间是多对一的关系(多connection对1device), 但是同一时刻只能有一个 Connection 对于 Device 才生效.
```

```shell
2. 命令行工具: nmcli
链接WiFi网络: nmcli dev wifi connectpassword
通过wlan1接口连接 WiFi 网络: nmcli dev wifi connectpasswordiface wlan1 [profile name]
断开一个接口: nmcli dev disconnect iface eth0
重新连接一个标记为已断开的接口: nmcli con up uuid
获得 UUID 列表: nmcli con show
查看网络设备及其状态列表: nmcli dev
关闭 WiFi: nmcli r wifi off

NetworkManager 命令
1. nmcli connection 网络连接管理
$ nmcli connection show        # 查看所有网卡配置
$ nmcli connection reload       # 重新加载网卡配置, 不会立即生效
$ nmcli connection down ens160 && nmcli connection up ens160        # 立即生效Connection配置
$ nmcli connection add type ethernet con-name ens160-con ifname ens160 ipv4.addr 1.1.1.2/24 ipv4.gateway 1.1.1.1 ipv4.method manual        # 为device创建connection
$ nmcli connection add type ethernet con-name ens160-con ifname ens160 ipv.method auto        # dhcp
$ nmcli connection modify ens160-con ipv.addr 1.1.1.3/24 && nmcli connection up ens160-con    # 修改IP地址并立即生效
2. 交互方式修改IP
$ nmcli connection edit ens160-con
3. nmcli device 网卡设备管理
$ nmcli device status                    # 查看所有网卡设备状态
$ nmcli device show ens160        # 查看网卡配置
$ nmcli device reapply ens160    # 立即生效网卡配置
```

```shell
3. 命令行GUI
nmtui 是一个基于curses的图形化前端, 直接输入nmtui就可以进入简单的命令行GUI进行操作
```

