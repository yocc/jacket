# Clash

----

[toc]



## Overview

在 CentOS7 上安装 Clash, 并通过命令行使用.

### Clash 安装

```shell
1. 从 GitHub 获取
Github: https://github.com/Dreamacro/clash
Release:
https://github.com/Dreamacro/clash/releases/download/v1.11.8/clash-linux-amd64-v1.11.8.gz 正确使用
https://github.com/Dreamacro/clash/releases/download/v1.11.8/clash-linux-amd64-v3-v1.11.8.gz  此程序只能在支持 v3 微体系结构的 AMD64 处理器上运行

2. 解压缩
2.1. 获得 clash 应用程序
直接解压解包, 就一个文件, 直接修改 clash-linux-amd64-v1.11.8 名字位 clash, chmod 755
直接 ./clash -v, 看看是不是正常
[chenchen@grpc01 clash]$ pwd
/home/chenchen/tmp/clash
~/tmp/clash/
469824553 -rw-rw-r--.  1 chenchen chenchen 3.8M Aug 10  2021 Country.mmdb
469771887 -rw-rw-r--.  1 chenchen chenchen  16K Jul  6 10:29 cache.db
   595690 drwxrwxr-x. 33 chenchen chenchen 4.0K Jul  7 03:51 ..
469824551 -rwxr-xr-x.  1 chenchen chenchen 8.8M Sep 26 10:32 clash
469824552 -rw-rw-r--.  1 chenchen chenchen 130K Sep 26 10:36 config.yaml
469771883 drwxrwxr-x.  2 chenchen chenchen  264 Sep 26 10:46 .
将 /home/chenchen/tmp/clash/clash 拷贝到 /usr/local/bin/clash

2.2. 获取 ./config.yaml 配置, 即 Clash订阅链接(TNTV2首页订阅信息中)
sudo wget -O config.yaml [订阅链接]
[chenchen@grpc01 clash]$ wget -O ./config.yaml 'https://linkuserssnk.xxyjx.cc/link/ArcLTSS5VthRErME?clash=1' --no-check-certificate
注意: 这里扩展名是 yaml, 而不是 yml.

2.3. 获取 ./Country.mmdb ip数据库文件
[chenchen@grpc01 clash]$ wget 'https://dl.ssrss.club/Country.mmdb' --no-check-certificate

2.4. 将 config.yaml 和 Country.mmdb 拷贝到 服务参数目录中(Systemd中配置, 见下)
[chenchen@grpc01 clash]$ sudo cp config.yaml /etc/clash/
[chenchen@grpc01 clash]$ sudo cp Country.mmdb /etc/clash/
[chenchen@grpc01 clash]$ pwd
/home/chenchen/tmp/clash
因为: 
configuration file /home/chenchen/.config/clash/config.yaml test is successful
所以:
[chenchen@grpc01 clash]$ sudo cp config.yaml ~/.config/clash/config.yaml
```



### 配置, 调试, 踩坑

```shell
[chenchen@grpc01 clash]$ clash -v
Clash v1.11.8 linux amd64 with go1.19 Fri Aug 26 13:20:30 UTC 2022
[chenchen@grpc01 clash]$ clash -h
Usage of clash:
  -d string
        set configuration directory
  -ext-ctl string
        override external controller address
  -ext-ui string
        override external ui directory
  -f string
        specify configuration file
  -secret string
        override secret for RESTful API
  -t    test configuration and exit
  -v    show current version of clash
[chenchen@grpc01 clash]$ /usr/local/bin/clash -t -d /etc/clash
INFO[0000] Start initial compatible provider 🍎苹果服务      
INFO[0000] Start initial compatible provider 🎬国外媒体      
INFO[0000] Start initial compatible provider ⚓️其他流量     
INFO[0000] Start initial compatible provider 🔰国外流量      
INFO[0000] Start initial compatible provider 🎬Youtube   
INFO[0000] Start initial compatible provider ✈️Telegram 
INFO[0000] Start initial compatible provider 🎬哔哩哔哩      
INFO[0000] Start initial compatible provider 🎬Netflix   
INFO[0000] Start initial compatible provider 🚀直接连接      
configuration file /etc/clash/config.yaml test is successful
[chenchen@grpc01 clash]$ 
# 这里 -t 测试要注意, 不能只 -t, 不加 -d /etc/clash, 否则测试加载的 config.yaml 文件不对
# 配置文件扩展名 yaml, 我用此扩展名跑起来, yml 没试过

# 配置文件官方文档:
See Also: https://github.com/Dreamacro/clash/wiki/Configuration#all-configuration-options
调试服务和配置文件 config.yaml 时有要注意的地方:
1. config.yaml 配置文件
allow-lan: true					# 改为 true, 原因没深究都让改
mode: rule							# 三种选择, rule, global, direct
log-level: debug				# info / warning / error / debug / silent, debug 要配合命令行运行服务在终端前台才能看到调试

2. 服务调试
[chenchen@localhost clash]$ pwd
/home/chenchen/tmp/clash
[chenchen@localhost clash]$ sudo ./clash -d ./
# 通过临时的 命令行 在前台 指定 -d 配置文件的目录 来前台启动服务, 这时 clash 服务启动后会占据终端前台, 导致不能干别的事儿
# 然后通过另一个终端, 来配置调试, 这里要注意, 另一个终端调试时, 要调试 80 服务, 而不是 ping 服务. 即:
curl www.google.com
curl www.google.com -i			# 返回全部 header + body
curl www.google.com -I			# 仅 header
curl www.google.com -s
curl www.google.com -I -s
# 不能
ping www.google.com
# 待到调试没问题了, 再将 clash 服务用 systemd 管理起来, 自动运行什么


```



### config.yaml

```shell
port: 7890
socks-port: 7891
redir-port: 7892
allow-lan: true
mode: rule
log-level: debug
external-controller: '0.0.0.0:9090'
secret: ''
dns:
proxies:
proxy-groups: # 重点在这里, 手动配置这里上面的优先选择, 唯一选择. 调整代理顺序, 将会优化结果. https://tntv2.cyou/user/node 从这里查看 节点 负载情况.
rules:
```



### Systemd 配置

```shell
# Running Clash as a service
See Also: https://github.com/Dreamacro/clash/wiki/Running-Clash-as-a-service

[chenchen@grpc01 clash]$ l /etc/systemd/system/clash.service
12799247 -rw-r--r--. 1 root root 201 Jul  6 04:28 /etc/systemd/system/clash.service

1. (采用)
[Unit]
Description=Clash daemon, A rule-based proxy in Go.
After=network.target

[Service]
Type=simple
Restart=always
ExecStart=/usr/local/bin/clash -d /etc/clash

[Install]
WantedBy=multi-user.target

# 说明: Restart=always 杀不掉, 随时杀随时重启服务
# ExecStart=/usr/local/bin/clash -d /etc/clash 这里指定了各个起效的配置文件

2. 
[Unit]
Description=clash

[Service]
TimeoutStartSec=0
ExecStart=/opt/clash/clash -d /opt/clash

[Install]
WantedBy=multi-user.target

```



### ~/.bashrc

```shell
[chenchen@grpc01 clash]$ vim ~/.bashrc
# clash 增加
export https_proxy=http://127.0.0.1:7890
export http_proxy=http://127.0.0.1:7890
# export all_proxy=socks5://127.0.0.1:7891

# 重启生效
[chenchen@grpc01 clash]$ . ~/.bashrc


### 代理配置番外篇 ###
1. 
环境变量: http_proxy			
描述: 为http变量设置代理；默认不填开头以http协议传输
示例:
10.0.0.51:8080
user:pass@10.0.0.10:8080
socks4://10.0.0.51:1080
socks5://192.168.1.1:1080

2.
环境变量: https_proxy
描述: 为https变量设置代理

3. 
环境变量: ftp_proxy
描述: 为ftp变量设置代理

4. 
环境变量: all_proxy
描述: 全部变量设置代理，设置了这个时候上面的不用设置

5.
环境变量: no_proxy
描述: 无需代理的主机或域名; 可以使用通配符; 多个时使用“,”号分隔;
示例:
*.aiezu.com,10.*.*.*,192.168.*.*,
*.local,localhost,127.0.0.1 

# 示例:
export proxy="http://192.168.5.14:8118"
export http_proxy=$proxy
export https_proxy=$proxy
export ftp_proxy=$proxy
export no_proxy="localhost, 127.0.0.1, ::1"

# 取消:
unset http_proxy
unset https_proxy
unset ftp_proxy
unset no_proxy

# 查看
env | grep proxy
```



### Systemctl 管理

```shell
启动 Clash: systemctl start clash
关闭 Clash: systemctl stop clash

查看状态(可以用来检测是否成功启动): systemctl status clash

设置开机自启: systemctl enable clash
取消开机自启: systemctl disable clash

e.g.:
[chenchen@grpc01 clash]$ sudo systemctl start clash
[chenchen@grpc01 clash]$ sudo systemctl status clash
● clash.service - Clash daemon, A rule-based proxy in Go.
   Loaded: loaded (/etc/systemd/system/clash.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2022-09-26 11:01:49 CST; 10s ago
 Main PID: 9467 (clash)
    Tasks: 5
   Memory: 8.6M
   CGroup: /system.slice/clash.service
           └─9467 /usr/local/bin/clash -d /etc/clash

Sep 26 11:01:49 grpc01 systemd[1]: Started Clash daemon, A rule-based proxy in Go..
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider ⚓️其他流量"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider 🍎苹果服务"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider ✈️Telegram"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider 🔰国外流量"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider 🎬哔哩哔哩"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider 🎬Youtube"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider 🎬Netflix"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider 🎬国外媒体"
Sep 26 11:01:49 grpc01 clash[9467]: time="2022-09-26T11:01:49+08:00" level=info msg="Start initial compatible provider 🚀直接连接"
[chenchen@grpc01 clash]$ 
```



### 关闭 clash 服务后, 关闭本地代理配置

```shell
[chenchen@grpc01 clash]$ unset http_proxy
[chenchen@grpc01 clash]$ unset https_proxy
```





## FAQ & troubleshooting



## See Also

https://github.com/Dreamacro/clash

https://github.com/Dreamacro/clash/wiki/Configuration#all-configuration-options
