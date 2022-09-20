# netstat, ss, watch

---

[toc]

## netstat

### 安装

```shell
# yum -y install net-tools
```



### 参数

```shell
netstat -ntlpu

-t, 列出 tcp 端口
-u, 列出 udp 端口
-n, 列出端口信息
-l, 列出 listen 监听状态 端口
-p, 列出 processes 进程信息


```



```shell
[chenchen@dev3_10.211.21.18 ~]$ netstat -tulpn
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 10.211.21.18:27018      0.0.0.0:*               LISTEN      -                   
tcp        0      0 10.211.21.18:9004       0.0.0.0:*               LISTEN      -                   
tcp        0      0 10.211.21.18:6380       0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp        0      0 10.211.21.18:7600       0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:4369            0.0.0.0:*               LISTEN      -                   
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 10.211.21.18:8088       0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      -                   
tcp        0      0 10.211.21.18:8873       0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::4369                 :::*                    LISTEN      -                   
udp        0      0 10.211.21.18:7600       0.0.0.0:*                           -                   
udp        0      0 192.168.122.1:53        0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:67              0.0.0.0:*                           -                   
udp        0      0 127.0.0.1:323           0.0.0.0:*                           -                   
udp6       0      0 ::1:323                 :::*                                -                   
[chenchen@dev3_10.211.21.18 ~]$ 

看不到 processes 信息是因为权限不够, sudo netstat -ntlpu 就能看到了
Local Address, 0.0.0.0 是指监听本地这个端口的所有ip地址
State, 处于 LISTEN 状态的
```



## ss

### 参数

```shell
sudo ss -ntlpu
同 netstat -ntlpu 一样
```



```shell
[chenchen@dev3_10.211.21.18 ~]$ ss -ntlpu
Netid  State      Recv-Q Send-Q                                                                           Local Address:Port                                                                                          Peer Address:Port              
udp    UNCONN     0      0                                                                                 10.211.21.18:7600                                                                                                     *:*                  
udp    UNCONN     0      0                                                                                192.168.122.1:53                                                                                                       *:*                  
udp    UNCONN     0      0                                                                                     *%virbr0:67                                                                                                       *:*                  
udp    UNCONN     0      0                                                                                    127.0.0.1:323                                                                                                      *:*                  
udp    UNCONN     0      0                                                                                          ::1:323                                                                                                     :::*                  
tcp    LISTEN     0      128                                                                               10.211.21.18:27018                                                                                                    *:*                  
tcp    LISTEN     0      128                                                                               10.211.21.18:9004                                                                                                     *:*                  
tcp    LISTEN     0      128                                                                               10.211.21.18:6380                                                                                                     *:*                  
tcp    LISTEN     0      128                                                                                          *:80                                                                                                       *:*                  
tcp    LISTEN     0      128                                                                               10.211.21.18:7600                                                                                                     *:*                  
tcp    LISTEN     0      128                                                                                          *:8081                                                                                                     *:*                  
tcp    LISTEN     0      128                                                                                          *:4369                                                                                                     *:*                  
tcp    LISTEN     0      5                                                                                192.168.122.1:53                                                                                                       *:*                  
tcp    LISTEN     0      128                                                                                          *:22                                                                                                       *:*                  
tcp    LISTEN     0      128                                                                               10.211.21.18:8088                                                                                                     *:*                  
tcp    LISTEN     0      128                                                                                          *:443                                                                                                      *:*                  
tcp    LISTEN     0      5                                                                                 10.211.21.18:8873                                                                                                     *:*                  
tcp    LISTEN     0      128                                                                                         :::4369                                                                                                    :::*                  
[chenchen@dev3_10.211.21.18 ~]$ 
```



## watch

### 参数

```shell
sudo watch netstat -ntlpu
每两秒自动刷新一次

sudo ss -ntlpu
同理 watch 也能用于 ss 命令
```

```
Every 2.0s: netstat -ntlpu                                                  Tue May  3 15:56:11 2022

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 10.211.21.18:27018      0.0.0.0:*               LISTEN      31899/bin/mongod
tcp        0      0 10.211.21.18:9004       0.0.0.0:*               LISTEN      17450/./collection_
tcp        0      0 10.211.21.18:6380       0.0.0.0:*               LISTEN      11043/redis-server
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      9528/nginx: master
tcp        0      0 10.211.21.18:7600       0.0.0.0:*               LISTEN      1164/memcached
tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      9528/nginx: master
tcp        0      0 0.0.0.0:4369            0.0.0.0:*               LISTEN      4952/epmd
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      2457/dnsmasq
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1142/sshd
tcp        0      0 10.211.21.18:8088       0.0.0.0:*               LISTEN      16941/./pushser
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      9528/nginx: master
tcp        0      0 10.211.21.18:8873       0.0.0.0:*               LISTEN      13468/rsync
tcp6       0      0 :::4369                 :::*                    LISTEN      4952/epmd
udp        0      0 10.211.21.18:7600       0.0.0.0:*                           1164/memcached
udp        0      0 192.168.122.1:53        0.0.0.0:*                           2457/dnsmasq
udp        0      0 0.0.0.0:67              0.0.0.0:*                           2457/dnsmasq
udp        0      0 127.0.0.1:323           0.0.0.0:*                           784/chronyd
udp6       0      0 ::1:323                 :::*                                784/chronyd
```



## See Also
