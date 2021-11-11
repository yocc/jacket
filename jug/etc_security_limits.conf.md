# /etc/security/limits.conf





##### /etc/security/limits.conf

linux 资源限制配置文件是 /etc/security/limits.conf, 限制用户进程的数量对于 linux 系统的稳定性非常重要.
limits.conf 文件限制着用户可以使用的最大文件数, 最大线程, 最大内存等资源使用量.

```shell
* soft nofile 655350		# 任何用户可以打开的最大的文件描述符数量, 默认1024, 这里的数值会限制tcp连接
* hard nofile 655350
* soft nproc  655350		# 任何用户可以打开的最大进程数
* hard nproc  650000

* soft nofile 1048576		# C1000k, 100w
* hard nofile 1048576

@student hard nofile 65535
@student soft nofile 4096
@student hard nproc 50		# 学生组中的任何人不能拥有超过50个进程，并且会在拥有30个进程时发出警告
@student soft nproc 30

1048576(2^20)
```

> hard 和 soft 两个值都代表什么意思呢?
> soft 是一个警告值, 而 hard 则是一个真正意义的阀值, 超过就会报错
> soft 指的是当前系统生效的设置值. hard 表明系统中所能设定的最大值
> nofile – 单进程打开文件的最大数目
> 星号 表示针对所有用户, 若仅针对某个用户登录ID, 请替换星号



##### 端口范围:

```shell
# linux系统端口范围为 0 ~ 65536, 系统提供了默认的端口范围:
cat /proc/sys/net/ipv4/ip_local_port_range 
32768 61000

# 故当前端口使用范围为 61000-32768 约为 28200 个, 将端口范围扩大, 修改/etc/sysctl.conf, 增加一行:
net.ipv4.ip_local_port_range= 1024 65535
# 保存, 使之生效
sysctl -p
接着在测试端程序发出的连接数量大于某个值(大概为40万时)是, 通过 dmesg 命令查看会得到大量警告信息: [warn]socket: Too many openfiles in system
此时需要修改 file-max, 表示系统所有进程最多允许同时打开所有的文件句柄数, 系统级硬限制. 添加 fs.file-max = 1048576 到 /etc/sysctl.conf 中, sysctl -p 保存并使之生效。
在服务端连接达到一定数量时，通过查看dmesg命令查看，发现大量TCP: toomany of orphaned sockets错误，此时需要调整tcp socket参数，添加：

net.ipv4.tcp_mem= 786432 2097152 3145728
net.ipv4.tcp_rmem= 4096 4096 16777216
net.ipv4.tcp_wmem= 4096 4096 16777216
net.ipv4.tcp_max_orphans= 131072 

net.ipv4.tcp_rmem用来配置读缓冲的大小，三个值，第一个是这个读缓冲的最小值，第三个是最大值，中间的是默认值。我们可以在程序中修改读缓冲的大小，但是不能超过最小与最大。为了使每个socket所使用的内存数最小，我这里设置默认值为4096。
net.ipv4.tcp_wmem用来配置写缓冲的大小。
读缓冲与写缓冲的大小，直接影响到socket在内核中内存的占用。
而net.ipv4.tcp_mem则是配置tcp的内存大小，其单位是页，1页等于4096字节。三个值分别为low, pressure, high
·low：当TCP使用了低于该值的内存页面数时，TCP不会考虑释放内存。
·pressure：当TCP使用了超过该值的内存页面数量时，TCP试图稳定其内存使用，进入pressure模式，当内存消耗低于low值时则退出pressure状态。
·high：允许所有tcp sockets用于排队缓冲数据报的页面量，当内存占用超过此值，系统拒绝分配socket，后台日志输出“TCP: too many of orphaned sockets”。

一般情况下这些值是在系统启动时根据系统内存数量计算得到的。 根据当前tcp_mem最大内存页面数是1864896，当内存为(1864896*4)/1024K=7284.75M时，系统将无法为新的socket连接分配内存，即TCP连接将被拒绝。实际测试环境中，据观察大概在99万个连接左右的时候(零头不算)，进程被杀死，触发outof socket memory错误（dmesg命令查看获得）。每一个连接大致占用7.5K内存（下面给出计算方式），大致可算的此时内存占用情况（990000 * 7.5/ 1024K = 7251M)。这样和tcp_mem最大页面值数量比较吻合，因此此值也需要修改。

另外net.ipv4.tcp_max_orphans这个值也要设置一下，这个值表示系统所能处理不属于任何进程的 socket数量，当我们需要快速建立大量连接时，就需要关注下这个值了。当不属于任何进程的socket的数量大于这个值时，dmesg就会看 到”too many of orphaned sockets”。

综上, 服务端需要配置的内容做个汇总:
/etc/sysctl.conf 添加:
fs.file-max= 10485760
net.ipv4.ip_local_port_range= 1024 65535
net.ipv4.tcp_mem= 786432 2097152 3145728
net.ipv4.tcp_rmem= 4096 4096 16777216
net.ipv4.tcp_wmem= 4096 4096 16777216
net.ipv4.tcp_max_orphans= 131072 

/etc/security/limits.conf 修改:
* soft nofile 1048576
* hardnofile 1048576

```





知道了 /etc/security/limits.conf 中的参数含义之后, 那么如何配置nofile, 确定nofile的最大值呢?
解答: 使用 ulimit -n 命令进行测试, 如果小于系统允许的最大值, 设置成功, 大于最大值, 系统会报错提示.

```shell
$ ulimit -n 1100000
-bash: ulimit: open files: cannot modify limit: Operation not permitted
$ ulimit -n 1048576
$ ulimit -n 1048577
-bash: ulimit: open files: cannot modify limit: Operation not permitted
$ ulimit -n 1048575
$ ulimit -n 1048576
```






##### ulimit -a/n/H/S 都有什么含义

```shell
ulimit -a 显示当前所有的资源限制
ulimit -H 设置硬件资源限制
ulimit -S 设置软件资源限制
ulimit -n 设置进程最大打开文件描述符数

ulimit -u <程序数目> 　用户最多可开启的程序数目
```





##### 总结

````shell
a. 所有进程打开的文件描述符数不能超过 /proc/sys/fs/file-max (操作系统最大打开文件描述符数)
b. 单个进程打开的文件描述符数不能超过 user limit 中 nofile 的 soft limit (ulimit -n)
c. nofile 的 soft limit 不能超过其 hard limit (ulimit -Hn)
d. nofile 的 hard limit 不能超过 /proc/sys/fs/nr_open
````



汇总, 1048576(2^20)

```shell
1. 操作系统最大打开文件描述符数量: 
## 查看(两种方法查看)
[chenchen@localhost etc]$ cat /proc/sys/fs/file-max		# 返回一个数, 
183642
[chenchen@localhost etc]$ cat /proc/sys/fs/file-nr		# 返回三个数, 其中第三个数同上
1248    0       183642
## 修改
1.1. 编辑 sudo vim /etc/sysctl.conf 文件, 添加以下三行配置:
fs.file-max = 1020000
net.ipv4.ip_conntrack_max = 1020000
net.ipv4.netfilter.ip_conntrack_max = 1020000
1.2. 重启系统服务使之生效:
$ sudo sysctl -p /etc/sysctl.conf

2. 每个进程最大打开文件描述符数量:
## 查看, 每个进程可以打开的最大文件描述符数量
[chenchen@dev3_10.211.21.19 ~]$ ulimit -n			# 查看的是 soft limit
1024
[chenchen@dev3_10.211.21.19 ~]$ ulimit -Hn		# 查看的是 hard limit
4096
## 修改
2.1. 临时修改
# 通过 ulimit -Sn 设置最大打开文件描述符数的 soft limit, 注意 soft limit 必须小于 hard limit
$ ulimit -Sn 160000
# 同时设置 soft limit 和 hard limit. 对于非 root 用户只能设置比原来小的 hard limit, -n 后是要设置的新值, 不写就是查看
$ ulimit -n 180000
2.2. 永久修改
# root 权限下, 在 /etc/security/limits.conf 中添加如下两行, 表示所有用户最大打开文件描述符数的 soft limit 为 102400, hard limit为104800. 重新登录服务器生效.
* soft nofile 1048576
* hard nofile 1048576
# soft 是一个警告值, 而 hard 则是一个真正意义的阀值, 超过就会报错
# soft 指的是当前系统生效的设置值. hard 表明系统中所能设定的最大值
# nofile – 单进程打开文件的最大数目
# 星号 表示针对所有用户, 若仅针对某个用户登录ID, 请替换星号

## 注意: 设置 nofile 的 hard limit 还有一点要注意的就是 hard limit 不能大于 /proc/sys/fs/nr_open, 假如 hard limit 大于 nr_open, 注销后将无法正常登录
```





## 操作系统

##### 查看操作系统用户创建的进程数:

```shell
[chenchen@dev3_10.211.21.19 ~]$ ps h -Led -o user | sort | uniq -c | sort -n
      1 tcpdump
      3 qilin5
      3 redis
     10 chenchen
     16 mysql
     21 sysmon
     62 jintao5
     97 mongod
    117 www
    138 root
    241 es
[chenchen@dev3_10.211.21.19 ~]$ 
```

##### 查看操作系统当前正在使用的打开文件描述符数:

```shell
[chenchen@dev3_10.211.21.19 ~]$ cat /proc/sys/fs/file-nr
7680    0       2436537
# 其中第一个数表示当前系统已分配使用的打开文件描述符数, 第二个数为分配后已释放的(目前已不再使用), 第三个数等于 file-max
```

##### 查看操作系统最大打开文件描述符数:

```shell
[chenchen@dev3_10.211.21.19 ~]$ cat /proc/sys/fs/file-max  
2436537
[chenchen@dev3_10.211.21.19 ~]$ 
```

##### 修改操作系统最大打开文件描述符数: (未证实)

```shell
[chenchen@dev3_10.211.21.19 ~]$ sudo vim /etc/sysctl.conf
fs.file-max = 6553600
```





## 进程

##### 查看每个进程最大打开文件描述符数:

```shell
[chenchen@dev3_10.211.21.19 ~]$ ulimit -n			# 查看的是 soft limit
1024
[chenchen@dev3_10.211.21.19 ~]$ ulimit -Hn		# 查看的是 hard limit
4096
```

##### 设置每个进程最大打开文件描述符数:

```shell
## 临时
# 通过 ulimit -Sn 设置最大打开文件描述符数的 soft limit, 注意 soft limit 必须小于 hard limit
$ ulimit -Sn 160000

# 同时设置 soft limit 和 hard limit. 对于非 root 用户只能设置比原来小的 hard limit
$ ulimit -n 180000

## 永久
# root 权限下, 在 /etc/security/limits.conf 中添加如下两行, 表示所有用户最大打开文件描述符数的 soft limit 为 102400, hard limit为104800. 重启生效
* soft nofile 102400
* hard nofile 104800

## 注意: 设置 nofile 的 hard limit 还有一点要注意的就是 hard limit 不能大于 /proc/sys/fs/nr_open, 假如 hard limit 大于 nr_open, 注销后将无法正常登录
```





































