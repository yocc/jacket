# VirtualBox

---



## 目录

[TOC]



## 新建虚拟电脑

##### 虚拟电脑名称和系统类型

    类型: Linux
    版本: Other Linux (64-bit)

> 在‘版本’下拉选项下如果只能看到‘32-bit’, 看不到‘64-bit’的话.
> 你可能需要开启 BIOS 里的虚拟技术(VT)的开关, 打开它.
> 前提是 CPU 支持虚拟技术. 一般 Intel, AMD 都支持虚拟技术. BIOS 里的位置在 高级/CPU 设置有关的选项里.

##### 虚拟硬盘

    现在创建虚拟硬盘

##### 虚拟硬盘文件类型

    VDI (VirtualBox 磁盘映像)

##### 存储在物理硬盘上

    动态分配

##### 文件位置和大小

    点击右侧小文件夹图标, 在本地保存 .vdi 文件

## 设置

### 全局设定

##### 工具 -> 全局设定

##### 网络

    添加新的NAT网络
    勾选 ‘启用网络’
    网络名称: NatNetwork
    网络 CIDR: 10.0.2.0/24
    网络选项: 勾选 ‘支持DHCP’

### 每个虚拟机设置

##### 存储

    1. 选择 ‘没有盘片’
    2. 点‘光盘图标’选择一个虚拟光盘文件, 选择 CentOS-8.1.1911-x86_86-boot.iso

##### 网络

    网卡1
        勾选 ‘启用网络连接’
        连接方式: NAT 网络
        界面名称: NatNetwork                            # 事先在 ’全局设定‘ 中创建的 ’NAT 网络‘ 的名称
        高级
        控制芯片: Intel PRO/1000 MT 桌面 (82540EM)
        混杂模式: 拒绝
        MAC 地址: 0800275FEF58                          # 不能重复
        勾选 ‘接入网线’
    
    网卡2
        勾选 ‘启用网络连接’
        连接方式: 桥接网卡
        界面名称: Hyper-V Virtual Ethernet Adapter
        高级
        控制芯片: Intel PRO/1000 MT 桌面 (82540EM)
        混杂模式: 拒绝
        MAC 地址: 0800274B8849                          # 不能重复
        勾选 ‘接入网线’



## 启动和安装 OS

##### 启动

    选择 ’Test this media & install CentOS Linux 8‘

##### 选择安装时的语言

    English > English (United States)

##### 安装摘要 (消除所有叹号)

    Keyboard: English (US)
    Language Support: English (United States)
    Time & Date: Asia/Shanghai timezone
    Installation Destination: 不做修改; 存储配置选择‘Automatic’
    Network & Host Name: enpOs3 和 enp0s8 开启连接 ‘Connected’
    Installation Source: closest mirror 附近镜像保持空
    Software Selection: 左侧基本环境选择 ‘Server’; 右侧什么都不选;

##### 安装过程中的用户名和密码设置

    Root Password: root007
    User Creation: 不用创建新用户, 等系统装完, 从系统下再增加用户

##### 安装完成 & 重启

    重启前 ’移除虚拟盘‘

##### 添加新用户

    # useradd jeffchen
    # passwd jeffchen

##### 修改 /etc/suduers

```
chenchen    ALL=(ALL)       NOPASSWD: ALL    # 增加sudo权限，免密码
Defaults always_set_home                     # 注释掉该行，表示sudo后不重置home目录？？即home目录pwd不变
Defaults env_reset改为Defaults !env_reset     # 以上两步完成 sudo -s 之后 ~ 目录保持自己的 HOME 中目录
# Defaults env_keep += "HOME"                 # 去掉此行的注释
Defaults secure_path = /usr/local/mysql/bin:/usr/local/sinasrv2/bin:/usr/local/node/bin:/usr/local/sbin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sinasrv2/bin:/usr/local/go/bin                # 将需要自动加载的shell搜索目录添加到这里，以便在【sudo -s】之后还可以用
```

##### 配置 \$HOME/.bashrc

```
以下是 $HOMT/.bashrc 文件配置新增：
# User specific environment
PATH="$HOME/.local/bin:$HOME/bin:$PATH"
export PATH

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
export LS_OPTIONS='--color=auto'
eval "`dircolors`"
alias ls='ls $LS_OPTIONS'
alias ll='ls $LS_OPTIONS -l'
#alias l='ls $LS_OPTIONS -lA'
alias l='ls $LS_OPTIONS -alrthi'
alias lm='ls $LS_OPTIONS -alrthi | more'
export TERM='xterm-256color'
# export TERM='screen-256color'
#
# Some more alias to avoid making mistakes:
# alias rm='rm -i'
# alias cp='cp -i'
# alias mv='mv -i'

HISTSIZE=4500
ISTFILESIZE=4500
以上是 $HOMT/.bashrc 文件配置新增. end
```

##### 网络拓扑

###### VirtualBox 虚拟机系统全局设置

工具 -> 全局设置 -> 新建 NAT 网络 -> NAT 网络明细, 启用网络, 网络名称: NatNetwork, 网络 CIDR: 10.0.2.0/24, 支持 DHCP.

###### 单个虚拟机设置

每个虚机 -> 设置 -> 网络 -> 启用两块网卡:
网卡 1, 启用网络链接, 链接方式: NAT 网络, 界面名称: NatNetwork(上一步全局设置中新建的 NAT 网络名称), MAC 地址刷新两下防止不重复;
网卡 2, 启用网络链接, 链接方式: 桥接网卡, 界面名称: 选择宿主机实际联网的那个界面名称(控制面板\网络和 Internet\网络连接). 可以连通外网. 桥接和 MAC 地址有关, 相当于 HUB, 数据链路层实现.

###### 相关指令

    $ ip a                                  # 查看网络配置
    
    $ nmcli d s                             # 查看所有设备
    $ nmcli c s                             # 查看所有连接
    $ nmcli c d 链接名字                     # down 指定链接
    $ nmcli c u 链接名字                     # up 指定链接
    $ nmcli d c 设备名字                     # 同 up(c u), 重启设备, nmcli device connect --help
    
    $ sudo nmcli c c 链接名字 新链接名字       # 克隆一个链接, 产生新文件, sudo (network-scripts)
    $ sudo nmcli c delete 链接名字           # 删除一个链接, 删除链接文件, sudo
    
    $ route -nee                            # 查看路由表
    $ traceroute -n baidu.com               # 跟踪路由
    
    $ sudo yum -y install net-tools
    $ sudo yum -y install traceroute

###### 连通性

- 有线办公网络本地宿主机 -> 虚拟机, (通)桥接, (不通)NAT 网络
- 有线办公网络 -> 虚拟机, (通)桥接, (不通)NAT 网络
- 无线办公网络 -> 虚拟机, (不通)桥接, (不通)NAT 网络
- 公网 -> 虚拟机, (不通)桥接, (通)NAT 网络
- 虚拟机之间, (通)桥接, (通)NAT 网络
- 虚拟机 -> 本地宿主机, (通)桥接, (不通)NAT 网络
- 虚拟机 -> 有线办公网络, (通)桥接, (不通)NAT 网络
- 虚拟机 -> 无线办公网络, (不通)桥接, (不通)NAT 网络
- 虚拟机 -> 公网, (不通)桥接, (通)NAT 网络



## FAQ

##### 为什么要设置两块网卡, 一个 ‘NAT 网络’ 另一个 ‘桥接网卡’

    事实上常规环境下, 一个 ‘桥接网卡’ 就够用了, 但由于我们的办公网络存在WEB认证, 所以无法在无人工干预的情况下“自动”完成认证. 所以发现通过 “NAT 网络” 是不需要WEB认证. “桥接网络” 相当于 HUB, 数据链路层.

##### 路由优先级

    $ route -nee, 自上而下的顺序, 先遇到先执行.
    通过 nmcli c d 和 nmcli c u, 调整
    固定静态路由, 另见.

##### 连接名为 Wired connection 1

    这里有个插曲，当虚机 CentOS 系统安装时选择了网卡联网, 那么安装后的系统里的 ‘桥接’ 的那块网卡通过 nmcli d s 看会有个设备名为 enp0s8 的他的连接名为 ‘Wired connection 1’, 这个设备的配置文件并不在 /etc/sysconfig/network-script/ifcnf-enp0s8, 没有这个文件, 我也找了 /etc 目录下也没有这个文件, 我查询了很多文档其中有说这是系统自动默认的一个配置, 但并没有提配置文件的具体位置, 我理解是内存中的.
    然后按通常做法在 /etc/sysconfig/network-script/ifcnf-enp0s3 复制一个新的 ifcnf-enp0s8 (通过 sudo nmcli c c enp0s3 enp0s8 克隆, 也能生成文件ifcnf-enp0s8), 然后修改其中必要的字段值.
    然后重启连接 nmcli c d enp0s8；nmcli c u enp0s8，然后 nmcli c s 查看, 并未生效. 重启设备 nmcli d c enp0s8, 也不生效. reboot 系统之后查看生效了.

##### /etc/sysconfig/network-script/ifcnf-enp0s3 文件

```
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="d5f73c61-be9d-4f32-a035-f2219d8f1896"
DEVICE="enp0s3"
ONBOOT="yes"                                        // yes 开启
```



## See Also

    https://www.virtualbox.org/
