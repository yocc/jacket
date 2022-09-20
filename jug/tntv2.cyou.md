# Linux & clash

----

[toc]



## structure

```shell
# Linux 上科学上网的结构: 
在 Linux 本地安装一个 Proxy 代理客户端(clash), 这个 clash 的角色是远端代理服务器的客户端. 
clash 具有配置文件, 就像一个本地数据库, 保存了当下可用的远端代理服务器列表和服务器配置.
启动 clash 后, 从本地角度看, clash 又充当了本地一个 daemon 服务. 本地的其他通讯, 都会被 clash daemon 服务代理.

总结: 
远端代理服务 <---> 本地 clash daemon <---> 本地通讯,

clash 扮演两个角色:
1. 对于 '远端代理服务' 来说, clash 是本地对接的客户端
2. 对于 '本地通讯' 来说, clash 是本地对接的服务端
```



## Linux & clash

```shell
1. https://tntv2.cyou/user/help, Linux 下需要使用 clash 软件来安装
2. Git, https://github.com/Dreamacro/clash, releases, 获取最新 clash, 直接获取二进制版本, amd64, clash-freebsd-amd64-v1.11.0.gz
	 386 = 32位 intel
	 amd64 = 64位 intel
	 arm = mac m1
3. gunzip clash-linux-amd64-v1.11.0.gz, 解压
4. mv clash-linux-amd64-v1.11.0 clash, 改名
5. chmod 755 clash
6. ./clash -v
7. ./clash --help
8. ./clash -h
9. wget -O config.yml 'https://sdawsdhiowjxgodwas.xn--vhqy75b93eqtl.com/link/ArcLTSS5VthRErME?clash=1&log-level=info' --no-check-certificate, 在线下载 config.yaml 配置文件
10. ./clash -t -f ./config.yml, 测试配置文件
11. ./clash -f ./config.yml, 直接指定配置文件启动, 独占前台
12. `./clash -d .`, 直接指定当前目录启动, 自动找到当前目录下的 config.yaml 配置文件, 独占前台
13. 启动后, 会有一个下载 Country.mmdb 文件的过程, 需要耐心等待, 文件不小, 没有此文件不行.
```



## config.yaml 文件

```shell
# https://github.com/Dreamacro/clash/wiki/configuration, 文档

mode: rule
# mode: global | rule | direct

log-level: info
# log-level: info / warning / error / debug / silent


```



## sudo systemctl status|stop|start|disable|enable clash

```shell

```











## See Also

https://tntv2.cyou/user

clash: https://github.com/Dreamacro/clash



