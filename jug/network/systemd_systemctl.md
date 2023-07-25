# systemd & systemctl

----

[toc]







## 通过部分查看全称
```shell
sudo systemctl list-unit-files --type=service | grep enabled | grep Network
sudo systemctl list-unit-files --type=service | grep Network
sudo systemctl list-unit-files --type=service | grep -i network
```



## 停止服务, 重启服务器还会保持原有设置的状态
```shell
sudo systemctl stop NetworkManager-wait-online.service
```



## 关闭服务, 重启也是关闭的
```shell
sudo systemctl disable NetworkManager-wait-online.service
sudo systemctl mask NetworkManager-wait-online.service
```



## 启动服务, 重启服务器还会保持启动状态
```shell
sudo systemctl enable NetworkManager-wait-online.service
```



## 总结, 重启服务 和 重启服务器

```shell
# 临时性启动服务
systemctl start NetworkManager
# 临时性停止服务
systemctl stop NetworkManager

systemctl restart NetworkManager
systemctl reload NetworkManager
systemctl status NetworkManager

# 永久性开机启动服务
systemctl enable NetworkManager
# 永久性开机停止服务
systemctl disable NetworkManager
```



## 注销一个服务(service), systemctl mask 是注销服务的意思

```shell
systemctl mask 是注销服务的意思
注销服务意味着：
该服务在系统重启的时候不会启动
该服务无法进行做systemctl start/stop操作
该服务无法进行systemctl enable/disable操作

取消注销服务(service)
systemctl unmask firewalld
```





## Systemd 是什么?
Systemd 是一个系统管理 守护进程、工具 和 库 的 集合, 用于取代 System V 初始进程. Systemd 的功能是用于 集中管理 和 配置 类UNIX系统.
在 Linux 生态系统中, Systemd 被部署到了大多数的标准 Linux 发行版中, 只有为数不多的几个发行版尚未部署.

Systemd 通常是所有其它守护进程的父进程, 但并非总是如此.
ps -ef f
PID=1
/usr/lib/systemd/systemd --switched-root --system --deserialize 22



## Systemctl 是什么?
Systemctl 是一个 systemd 工具, 主要负责控制 systemd 系统 和 服务管理器.

以下旨在阐明在运行 systemd 的系统上 "如何 控制 系统 和 服务"




## 一, Systemd 初体验 和 Systemctl 基础

### 1. 查看当前系统中是否安装了 systemd, 并确认其版本
```shell
# 清晰表明此系统安装了 219 版本的 systemd
[chenchen@dev3_10.211.21.18 ~]$ systemctl --version
systemd 219
+PAM +AUDIT +SELINUX +IMA -APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ -LZ4 -SECCOMP +BLKID +ELFUTILS +KMOD +IDN
```



### 2. 查看 systemd 和 systemctl 的 二进制文件 和 库 文件的安装位置

```shell
[chenchen@dev3_10.211.21.18 ~]$ whereis systemd 
systemd: /usr/lib/systemd /etc/systemd /usr/share/systemd /usr/share/man/man1/systemd.1.gz
[chenchen@dev3_10.211.21.18 ~]$ whereis systemctl
systemctl: /usr/bin/systemctl /usr/share/man/man1/systemctl.1.gz
```



### 3. 查看 systemd 是否运行

```shell
[chenchen@dev3_10.211.21.18 ~]$ ps -ef f | grep [s]ystemd
root         1     0  0  2021 ?        Ss   200:15 /usr/lib/systemd/systemd --system --deserialize 15
root       407     1  0  2021 ?        Ss    53:39 /usr/lib/systemd/systemd-journald
root       440     1  0  2021 ?        Ss     0:00 /usr/lib/systemd/systemd-udevd
dbus       756     1  0  2021 ?        Ss   253:18 /bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
root       765     1  0  2021 ?        Ss   119:00 /usr/lib/systemd/systemd-logind

注意: systemd 是作为父进程 (PID=1)运行的. 
在上面带(-e)参数的 ps 命令输出中, 选择所有进程, 
(-a)选择除会话前导外的所有进程, 
并使用(-f)参数输出完整格式列表 (即 -eaf)
```



### 4. 分析 systemd 启动进程
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemd-analyze
Startup finished in 2.433s (kernel) + 1.885s (initrd) + 17.079s (userspace) = 21.398s
```



### 5. 分析启动时各个进程花费的时间
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemd-analyze blame
          7.142s kdump.service
          5.202s network.service
          2.159s docker.service
          2.137s postfix.service
          1.516s systemd-udev-settle.service
          1.339s polkit.service
          1.232s lvm2-monitor.service
          1.112s dev-sda8.device
           880ms libvirtd.service
           551ms rsyslog.service
           236ms php-fpm.service
           222ms dev-hugepages.mount
           219ms dev-mqueue.mount
           190ms sshd.service
           177ms systemd-remount-fs.service
           167ms systemd-journald.service
           164ms rhel-dmesg.service
           152ms systemd-tmpfiles-clean.service
           152ms proc-sys-fs-binfmt_misc.mount
           147ms memcached.service
						...
            63ms systemd-vconsole-setup.service
            62ms systemd-fsck-root.service
            53ms systemd-journal-flush.service
            50ms systemd-random-seed.service
            45ms iscsi-shutdown.service
            37ms xinetd.service
             2ms sys-kernel-config.mount
             2ms var-lib-nfs-rpc_pipefs.mount
           973us docker.socket
           864us blk-availability.service
lines 15-78/78 (END)

[chenchen@dev3_10.211.21.18 ~]$ systemd-analyze blame | grep -i Network
          5.202s network.service
```



### 6. 分析启动时的关键链     
```shell
[chenchen@dev3_10.211.21.18 ~]$ sudo systemd-analyze critical-chain
The time after the unit is active or started is printed after the "@" character.
The time the unit takes to start is printed after the "+" character.

multi-user.target @17.075s
└─libvirtd.service @9.924s +880ms
  └─remote-fs.target @9.923s
    └─remote-fs-pre.target @9.923s
      └─iscsi-shutdown.service @9.877s +45ms
        └─network.target @9.855s
[chenchen@dev3_10.211.21.18 ~]$ 
单元激活或启动后的时间打印在 "@" 字符之后
设备启动所需的时间打印在 "+" 字符之后

重要:
单元: Systemctl 接受,  服务(.service), 挂载点().mount), 套接口(.socket)和 设备(.device)作为单元.
```



### 7. 列出所有可用单元
```shell
[chenchen@dev3_10.211.21.18 ~]$ sudo systemctl list-unit-files
UNIT FILE                                     STATE   
proc-sys-fs-binfmt_misc.automount             static  
dev-hugepages.mount                           static  
dev-mqueue.mount                              static  
proc-fs-nfsd.mount                            static  
proc-sys-fs-binfmt_misc.mount                 static  
sys-fs-fuse-connections.mount                 static  
sys-kernel-config.mount                       static  
sys-kernel-debug.mount                        static  
tmp.mount                                     disabled
var-lib-nfs-rpc_pipefs.mount                  static  
brandbot.path                                 disabled
systemd-ask-password-console.path             static  
systemd-ask-password-plymouth.path            static  
session-5050700.scope                         static  
session-71.scope                              static  
abrt-ccpp.service                             enabled 
abrt-oops.service                             enabled 
abrt-pstoreoops.service                       disabled
abrt-vmcore.service                           enabled 
abrt-xorg.service                             enabled 
abrtd.service                                 enabled 
arp-ethers.service                            disabled
atd.service                                   enabled 
auditd.service                                enabled 
sysinit.target                                static  
system-update.target                          static  
time-sync.target                              static  
timers.target                                 static  
umount.target                                 static  
vsftpd.target                                 disabled
chrony-dnssrv@.timer                          disabled
fstrim.timer                                  disabled
mdadm-last-resort@.timer                      static  
systemd-readahead-done.timer                  indirect
systemd-tmpfiles-clean.timer                  static  
unbound-anchor.timer                          disabled

391 unit files listed.
[chenchen@dev3_10.211.21.18 ~]$ sudo systemctl list-unit-files | grep -i network
dbus-org.freedesktop.network1.service         bad     
NetworkManager-dispatcher.service             disabled
NetworkManager-wait-online.service            disabled
NetworkManager.service                        disabled
systemd-networkd.socket                       disabled
network-online.target                         static  
network-pre.target                            static  
network.target                                static  
[chenchen@dev3_10.211.21.18 ~]$ 
```



### 8. 列出所有运行中单元
```shell
    [chenchen@dev3_10.211.21.18 ~]$ sudo systemctl list-units
    UNIT                                                                                             LOAD   ACTIVE SUB       DESCRIPTION
    proc-sys-fs-binfmt_misc.automount                                                                loaded active running   Arbitrary Executable File Formats File System Automount Point
    sys-devices-pci0000:00-0000:00:01.0-0000:02:00.0-net-em3.device                                  loaded active plugged   NetXtreme BCM5720 Gigabit Ethernet PCIe
    sys-devices-pci0000:00-0000:00:01.0-0000:02:00.1-net-em4.device                                  loaded active plugged   NetXtreme BCM5720 Gigabit Ethernet PCIe
    sys-devices-pci0000:00-0000:00:01.1-0000:01:00.0-net-em1.device                                  loaded active plugged   NetXtreme BCM5720 Gigabit Ethernet PCIe
...
    sys-devices-virtual-net-docker0.device                                                           loaded active plugged   /sys/devices/virtual/net/docker0
    sys-devices-virtual-net-virbr0.device                                                            loaded active plugged   /sys/devices/virtual/net/virbr0
    sockets.target                                                                                   loaded active active    Sockets
    swap.target                                                                                      loaded active active    Swap
    sysinit.target                                                                                   loaded active active    System Initialization
    timers.target                                                                                    loaded active active    Timers
    systemd-readahead-done.timer                                                                     loaded active elapsed   Stop Read-Ahead Data Collection 10s After Completed Startup
    systemd-tmpfiles-clean.timer                                                                     loaded active waiting   Daily Cleanup of Temporary Directories

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

180 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.

LOAD = 反映是否已正确加载单位定义.
ACTIVE = 高级单元激活状态, 即 SUB 的泛化.
SUB = 低级单元激活状态, 值取决于单元类型.

列出 180 个已加载单元. 通过 --all 也可查看已加载但未活动的单位.
要显示所有已安装的单元文件, 请使用 "systemctl list-unit-files".
```



### 9. 列出所有失败单元
```shell
[chenchen@dev3_10.211.21.18 ~]$ sudo systemctl --failed
    UNIT            LOAD   ACTIVE SUB    DESCRIPTION
● docker.service  loaded failed failed Docker Application Container Engine
● postfix.service loaded failed failed Postfix Mail Transport Agent
● rngd.service    loaded failed failed Hardware RNG Entropy Gatherer Daemon
● rpcbind.socket  loaded failed failed RPCbind Server Activation Socket

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

4 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.
[chenchen@dev3_10.211.21.18 ~]$ 
```



### 10. 检查某个单元（如 cron.service）是否启用
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl is-enabled NetworkManager.service
disabled
```



### 11. 检查某个单元或服务是否运行
```shell
    [chenchen@dev3_10.211.21.18 ~]$ systemctl status NetworkManager.service
    ● NetworkManager.service - Network Manager
      Loaded: loaded (/usr/lib/systemd/system/NetworkManager.service; disabled; vendor preset: enabled)
      Active: inactive (dead)
     Docs: man:NetworkManager(8)
```



## 二, 使用Systemctl控制并管理服务
### 12. 列出所有服务(包括启用的和禁用的)
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl list-unit-files --type=service
[chenchen@dev3_10.211.21.18 ~]$ systemctl list-unit-files --type=service | grep -i network
dbus-org.freedesktop.network1.service         bad     
NetworkManager-dispatcher.service             disabled
NetworkManager-wait-online.service            disabled
NetworkManager.service                        disabled
```



### 13. Linux中如何启动、重启、停止、重载服务以及检查服务 (如 httpd.service) 状态
```shell
# systemctl start httpd.service
# systemctl restart httpd.service
# systemctl stop httpd.service
# systemctl reload httpd.service
# systemctl status httpd.service
[chenchen@dev3_10.211.21.18 ~]$ systemctl list-unit-files --type=service | grep -i tengine
tengine.service 

[chenchen@dev3_10.211.21.18 ~]$ systemctl status tengine.service
● tengine.service - Tengine
   Loaded: loaded (/usr/lib/systemd/system/tengine.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2022-06-28 11:08:39 CST; 6 days ago
  Process: 25544 ExecStop=/usr/local/sinasrv5/sbin/tengine -s stop (code=exited, status=0/SUCCESS)
  Process: 31572 ExecReload=/usr/local/sinasrv5/sbin/tengine -s reload -c /usr/local/sinasrv5/etc/tengine.conf (code=exited, status=0/SUCCESS)
  Process: 25547 ExecStart=/usr/local/sinasrv5/sbin/tengine -c /usr/local/sinasrv5/etc/tengine.conf (code=exited, status=0/SUCCESS)
 Main PID: 25548 (tengine)
   Memory: 279.1M
   CGroup: /system.slice/tengine.service
           ├─25548 nginx: master process /usr/local/sinasrv5/sbin/tengine -c /usr/local/sinasrv5/etc/tengine.conf
           ├─25549 nginx: worker process
           ├─25550 nginx: worker process
           ├─25551 nginx: worker process
           ├─25552 nginx: worker process
           └─25553 nginx: cache manager process

[chenchen@dev3_10.211.21.18 ~]$ systemctl status tengine.service
[chenchen@dev3_10.211.21.18 ~]$ systemctl start tengine.service
[chenchen@dev3_10.211.21.18 ~]$ systemctl restart tengine.service
[chenchen@dev3_10.211.21.18 ~]$ systemctl stop tengine.service
[chenchen@dev3_10.211.21.18 ~]$ systemctl reload tengine.service
注意: 当使用 systemctl 的start, restart, stop 和 reload 命令时, 我们不会从终端获取到任何输出内容 ,只有 status 命令可以打印输出.
```



### 14. 如何激活服务并在系统启动时启用或禁用服务(即系统启动时自动启动服务)
```shell
14、19、23中的 systemctl is-active 并不是用于激活，而是查询运行状态。
和 systemctl enable、systemctl disable 配对的应该是 systemctl is-enabled
[chenchen@dev3_10.211.21.18 ~]$ systemctl is-active tengine.service
active
[chenchen@dev3_10.211.21.18 ~]$ systemctl is-active NetworkManager.service
inactive
[chenchen@dev3_10.211.21.18 ~]$ systemctl is-enabled tengine.service
enabled
[chenchen@dev3_10.211.21.18 ~]$ systemctl is-enabled NetworkManager.service
disabled
# systemctl is-active httpd.service
# systemctl enable httpd.service
# systemctl disable httpd.service
```



### 15. 如何屏蔽(让它不能启动)或显示服务(如 httpd.service)
```shell
# systemctl mask httpd.service
ln -s '/dev/null' '/etc/systemd/system/httpd.service'

# systemctl unmask httpd.service
rm '/etc/systemd/system/httpd.service'
```



### 16. 使用 systemctl 命令杀死服务
```shell
# systemctl kill httpd
# systemctl status httpd
httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled)
   Active: failed (Result: exit-code) since Tue 2015-04-28 18:01:42 IST; 28min ago
 Main PID: 2881 (code=exited, status=0/SUCCESS)
   Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
```



## 三, 使用 Systemctl 控制并管理挂载点
### 17. 列出所有系统挂载点
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl list-unit-files --type=mount
UNIT FILE                     STATE   
dev-hugepages.mount           static  
dev-mqueue.mount              static  
proc-fs-nfsd.mount            static  
proc-sys-fs-binfmt_misc.mount static  
sys-fs-fuse-connections.mount static  
sys-kernel-config.mount       static  
sys-kernel-debug.mount        static  
tmp.mount                     disabled
var-lib-nfs-rpc_pipefs.mount  static  

9 unit files listed.
```



### 18. 挂载、卸载、重新挂载、重载系统挂载点并检查系统中挂载点状态
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl status tmp.mount
● tmp.mount - /tmp
      Loaded: loaded (/etc/fstab; disabled; vendor preset: disabled)
      Active: active (mounted) since Mon 2021-08-02 21:44:25 CST; 11 months 0 days ago
    Where: /tmp
     What: /dev/sda6
     Docs: man:fstab(5)
           man:systemd-fstab-generator(8)
      Memory: 0B
[chenchen@dev3_10.211.21.18 ~]$ 

# systemctl start tmp.mount
# systemctl stop tmp.mount
# systemctl restart tmp.mount
# systemctl reload tmp.mount
# systemctl status tmp.mount
```



### 19. 在启动时激活、启用或禁用挂载点(系统启动时自动挂载)
```shell
# systemctl is-active tmp.mount
# systemctl enable tmp.mount
# systemctl disable  tmp.mount
```



### 20. 在 Linux 中屏蔽(让它不能启用)或可见挂载点
```shell
# systemctl mask tmp.mount
ln -s '/dev/null' '/etc/systemd/system/tmp.mount'

# systemctl unmask tmp.mount
rm '/etc/systemd/system/tmp.mount'
```



## 四, 使用 Systemctl 控制并管理套接口
### 21. 列出所有可用系统套接口
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl list-unit-files --type=socket
UNIT FILE                    STATE   
dbus.socket                  static  
dm-event.socket              enabled 
docker.socket                disabled
epmd.socket                  disabled
epmd@.socket                 disabled
iscsid.socket                enabled 
iscsiuio.socket              enabled 
lldpad.socket                disabled
lvm2-lvmetad.socket          enabled 
lvm2-lvmpolld.socket         enabled 
rpcbind.socket               enabled 
rsyncd.socket                disabled
sshd.socket                  disabled
syslog.socket                static  
systemd-initctl.socket       static  
systemd-journald.socket      static  
systemd-networkd.socket      disabled
systemd-shutdownd.socket     static  
systemd-udevd-control.socket static  
systemd-udevd-kernel.socket  static  
virtlockd.socket             enabled 
virtlogd.socket              enabled 

### 22 unit files listed.
```


### 22. 在 Linux 中启动、重启、停止、重载套接口并检查其状态
```shell
# systemctl start cups.socket
# systemctl restart cups.socket
# systemctl stop cups.socket
# systemctl reload cups.socket
# systemctl status cups.socket
```



### 23. 在启动时激活套接口, 并启用或禁用它(系统启动时自启动)
```shell
# systemctl is-active cups.socket
# systemctl enable cups.socket
# systemctl disable cups.socket
```



### 24. 屏蔽(使它不能启动)或显示套接口
```shell
# systemctl mask cups.socket
ln -s '/dev/null' '/etc/systemd/system/cups.socket'

# systemctl unmask cups.socket
rm '/etc/systemd/system/cups.socket'
```



## 五, 服务的CPU利用率(分配额)
### 25. 获取当前某个服务的CPU分配额(如httpd)
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl show -p CPUShares tengine.service
CPUShares=18446744073709551615
注意: 各个服务的默认 CPU 分配份额 = 1024, 你可以 增加/减少 某个进程的 CPU 分配份额.
```



### 26. 将某个服务(httpd.service)的CPU分配份额限制为 2000 CPUShares/
```shell
# systemctl set-property httpd.service CPUShares=2000
# systemctl show -p CPUShares httpd.service
CPUShares=2000
注意: 当你为某个服务设置 CPUShares, 会自动创建一个以服务名命名的目录(如 httpd.service), 里面包含了一个名为 90-CPUShares.conf 的文件, 该文件含有 CPUShare 限制信息 ,你可以通过以下方式查看该文件:
# vi /etc/systemd/system/httpd.service.d/90-CPUShares.conf 
[Service]
CPUShares=2000
```



### 27. 检查某个服务的所有配置细节
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl show tengine
Type=forking
Restart=no
NotifyAccess=none
RestartUSec=100ms
TimeoutStartUSec=1min 30s
TimeoutStopUSec=1min 30s
WatchdogUSec=0
WatchdogTimestamp=Tue 2022-06-28 11:08:39 CST
WatchdogTimestampMonotonic=28473860821035
StartLimitInterval=10000000
StartLimitBurst=5
StartLimitAction=none
FailureAction=none
PermissionsStartOnly=no
RootDirectoryStartOnly=no
RemainAfterExit=no
GuessMainPID=yes
MainPID=25548
ControlPID=0
FileDescriptorStoreMax=0
StatusErrno=0
Result=success
ExecMainStartTimestamp=Tue 2022-06-28 11:08:39 CST
ExecMainStartTimestampMonotonic=28473860821014
ExecMainExitTimestampMonotonic=0
ExecMainPID=25548
ExecMainCode=0
ExecMainStatus=0
ExecStart={ path=/usr/local/sinasrv5/sbin/tengine ; argv[]=/usr/local/sinasrv5/sbin/tengine -c /usr/local/sinasrv5/etc/tengine.conf ; ignore_errors=no ; start_time=[Tue 2022-06-28 11:08:39 CST] ; stop_time=[Tue 2022-06-28 11:08:39 CST] ; pid=2554
ExecReload={ path=/usr/local/sinasrv5/sbin/tengine ; argv[]=/usr/local/sinasrv5/sbin/tengine -s reload -c /usr/local/sinasrv5/etc/tengine.conf ; ignore_errors=no ; start_time=[Tue 2022-04-19 16:58:15 CST] ; stop_time=[Tue 2022-04-19 16:58:18 CST]
ExecStop={ path=/usr/local/sinasrv5/sbin/tengine ; argv[]=/usr/local/sinasrv5/sbin/tengine -s stop ; ignore_errors=no ; start_time=[Tue 2022-06-28 11:08:37 CST] ; stop_time=[Tue 2022-06-28 11:08:39 CST] ; pid=25544 ; code=exited ; status=0 }
Slice=system.slice
ControlGroup=/system.slice/tengine.service
MemoryCurrent=294658048
Delegate=no
CPUAccounting=no
CPUShares=18446744073709551615
StartupCPUShares=18446744073709551615
CPUQuotaPerSecUSec=infinity
BlockIOAccounting=no
BlockIOWeight=18446744073709551615
StartupBlockIOWeight=18446744073709551615
MemoryAccounting=no
MemoryLimit=18446744073709551615
DevicePolicy=auto
UMask=0022
LimitCPU=18446744073709551615
LimitFSIZE=18446744073709551615
LimitDATA=18446744073709551615
LimitSTACK=18446744073709551615
LimitCORE=18446744073709551615
LimitRSS=18446744073709551615
LimitNOFILE=4096
LimitAS=18446744073709551615
LimitNPROC=95530
LimitMEMLOCK=65536
LimitLOCKS=18446744073709551615
LimitSIGPENDING=95530
LimitMSGQUEUE=819200
LimitNICE=0
LimitRTPRIO=0
LimitRTTIME=18446744073709551615
OOMScoreAdjust=0
Nice=0
IOScheduling=0
CPUSchedulingPolicy=0
CPUSchedulingPriority=0
TimerSlackNSec=50000
CPUSchedulingResetOnFork=no
NonBlocking=no
StandardInput=null
StandardOutput=journal
StandardError=inherit
TTYReset=no
TTYVHangup=no
TTYVTDisallocate=no
SyslogPriority=30
SyslogLevelPrefix=yes
SecureBits=0
CapabilityBoundingSet=18446744073709551615
MountFlags=0
PrivateTmp=no
PrivateNetwork=no
PrivateDevices=no
ProtectHome=no
ProtectSystem=no
SameProcessGroup=no
IgnoreSIGPIPE=yes
NoNewPrivileges=no
SystemCallErrorNumber=0
RuntimeDirectoryMode=0755
KillMode=control-group
KillSignal=15
SendSIGKILL=yes
SendSIGHUP=no
Id=tengine.service
Names=tengine.service
Requires=basic.target
Wants=system.slice
WantedBy=multi-user.target
Conflicts=shutdown.target
Before=multi-user.target shutdown.target
After=network.target basic.target systemd-journald.socket system.slice
Description=Tengine
LoadState=loaded
ActiveState=active
SubState=running
FragmentPath=/usr/lib/systemd/system/tengine.service
UnitFileState=enabled
UnitFilePreset=disabled
InactiveExitTimestamp=Tue 2022-06-28 11:08:39 CST
InactiveExitTimestampMonotonic=28473860734564
ActiveEnterTimestamp=Tue 2022-06-28 11:08:39 CST
ActiveEnterTimestampMonotonic=28473860821054
ActiveExitTimestamp=Tue 2022-06-28 11:08:37 CST
ActiveExitTimestampMonotonic=28473858619455
InactiveEnterTimestamp=Tue 2022-06-28 11:08:39 CST
InactiveEnterTimestampMonotonic=28473860728590
CanStart=yes
CanStop=yes
CanReload=yes
CanIsolate=no
StopWhenUnneeded=no
RefuseManualStart=no
RefuseManualStop=no
AllowIsolate=no
DefaultDependencies=yes
OnFailureJobMode=replace
IgnoreOnIsolate=no
IgnoreOnSnapshot=no
NeedDaemonReload=no
JobTimeoutUSec=0
JobTimeoutAction=none
ConditionResult=yes
AssertResult=yes
ConditionTimestamp=Tue 2022-06-28 11:08:39 CST
ConditionTimestampMonotonic=28473860733876
AssertTimestamp=Tue 2022-06-28 11:08:39 CST
AssertTimestampMonotonic=28473860733877
Transient=no
[chenchen@dev3_10.211.21.18 ~]$ 
```



### 28. 分析某个服务(httpd)的关键链
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemd-analyze critical-chain tengine.service
The time after the unit is active or started is printed after the "@" character.
The time the unit takes to start is printed after the "+" character.

tengine.service +86ms
└─network.target @9.855s
[chenchen@dev3_10.211.21.18 ~]$ 
```



### 29. 获取某个服务(httpd)的依赖性列表
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl list-dependencies tengine.service
tengine.service
● ├─system.slice
● └─basic.target
●   ├─microcode.service
●   ├─rhel-autorelabel-mark.service
●   ├─rhel-autorelabel.service
●   ├─rhel-configure.service
●   ├─rhel-dmesg.service
●   ├─rhel-loadmodules.service
●   ├─selinux-policy-migrate-local-changes@targeted.service
●   ├─paths.target
●   ├─slices.target
●   │ ├─-.slice
●   │ └─system.slice
●   ├─sockets.target
●   │ ├─dbus.socket
●   │ ├─dm-event.socket
●   │ ├─iscsid.socket
●   │ ├─iscsiuio.socket
●   │ ├─rpcbind.socket
●   │ ├─systemd-initctl.socket
●   │ ├─systemd-journald.socket
●   │ ├─systemd-shutdownd.socket
●   │ ├─systemd-udevd-control.socket
●   │ ├─systemd-udevd-kernel.socket
●   │ ├─virtlockd.socket
●   │ └─virtlogd.socket
●   ├─sysinit.target
●   │ ├─dev-hugepages.mount
●   │ ├─dev-mqueue.mount
●   │ ├─dmraid-activation.service
●   │ ├─iscsi.service
●   │ ├─kmod-static-nodes.service
●   │ ├─lvm2-lvmetad.socket
●   │ ├─lvm2-lvmpolld.socket
●   │ ├─lvm2-monitor.service
●   │ ├─multipathd.service
●   │ ├─plymouth-read-write.service
●   │ ├─plymouth-start.service
●   │ ├─proc-sys-fs-binfmt_misc.automount
●   │ ├─sys-fs-fuse-connections.mount
●   │ ├─sys-kernel-config.mount
●   │ ├─sys-kernel-debug.mount
●   │ ├─systemd-ask-password-console.path
●   │ ├─systemd-binfmt.service
●   │ ├─systemd-firstboot.service
●   │ ├─systemd-hwdb-update.service
●   │ ├─systemd-journal-catalog-update.service
●   │ ├─systemd-journal-flush.service
●   │ ├─systemd-journald.service
●   │ ├─systemd-machine-id-commit.service
●   │ ├─systemd-modules-load.service
●   │ ├─systemd-random-seed.service
●   │ ├─systemd-sysctl.service
●   │ ├─systemd-tmpfiles-setup-dev.service
●   │ ├─systemd-tmpfiles-setup.service
●   │ ├─systemd-udev-trigger.service
●   │ ├─systemd-udevd.service
●   │ ├─systemd-update-done.service
●   │ ├─systemd-update-utmp.service
●   │ ├─systemd-vconsole-setup.service
●   │ ├─cryptsetup.target
●   │ ├─local-fs.target
●   │ │ ├─-.mount
●   │ │ ├─boot.mount
●   │ │ ├─data0.mount
●   │ │ ├─data1.mount
●   │ │ ├─rhel-import-state.service
●   │ │ ├─rhel-readonly.service
●   │ │ ├─systemd-fsck-root.service
●   │ │ ├─systemd-remount-fs.service
●   │ │ ├─tmp.mount
●   │ │ └─var.mount
●   │ └─swap.target
●   │   └─dev-disk-by\x2duuid-18c21f3a\x2d7f6a\x2d470c\x2d8f71\x2d0900918f9798.swap
●   └─timers.target
●     └─systemd-tmpfiles-clean.timer
```



### 30. 按等级列出控制组
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemd-cgls
├─1 /usr/lib/systemd/systemd --system --deserialize 15
├─user.slice
│ ├─user-1020.slice
│ │ └─session-3214129.scope
│ │   └─13468 /usr/bin/rsync --port 8873 --daemon
│ ├─user-1006.slice
│ │ └─session-1380856.scope
│ │   ├─16856 tmux
│ │   └─16857 -bash
│ ├─user-1009.slice
│ │ ├─session-5042769.scope
│ │ │ ├─15741 sshd: liyong5 [priv
│ │ │ ├─15743 sshd: liyong5@pts/
│ │ │ └─15744 -bash
│ │ ├─session-5042748.scope
```



### 31. 按CPU、内存、输入和输出, 列出控制组
```shell
Path												Tasks   %CPU   Memory  Input/s Output/s

/												92  354.9    18.5G        -        -
/user.slice												64  346.0    15.1G        -        -
/system.slice												-    0.5     2.9G        -        -
/system.slice/containerd.service												1    0.2    29.6M        -        -
/system.slice/php-fpm.service												273    0.2   569.1M        -        -
/system.slice/memcached.service												1    0.1     2.9M        -        -
/system.slice/tengine.service												6    0.0   281.3M        -        -
/system.slice/tuned.service												1    0.0        -        -        -
/docker												-      -     2.7M        -        -
/system.slice/abrt-oops.service												1      -        -        -        -
/system.slice/abrtd.service												1      -    10.7M        -        -
/system.slice/atd.service												1      -        -        -        -
/system.slice/auditd.service												1      -     2.0M        -        -
/system.slice/chronyd.service												1      -   368.0K        -        -
/system.slice/crond.service												1      -     2.0M        -        -
/system.slice/dbus.service												1      -     2.2M        -        -
/system.slice/gssproxy.service												1      -        -        -        -
/system.slice/irqbalance.service												1      -        -        -        -
/system.slice/ksmtuned.service												2      -     1.2M        -        -
/system.slice/libstoragemgmt.service												1      -        -        -        -
/system.slice/libvirtd.service												3      -     1.0M        -        -
/system.slice/lvm2-lvmetad.service												1      -        -        -        -
/system.slice/mcelog.service												1      -        -        -        -
/system.slice/polkit.service												1      -    68.0K        -        -
/system.slice/rsyslog.service												1      -   870.6M        -        -
/system.slice/smartd.service												1      -   136.0K        -        -
/system.slice/sshd.service												1      -    11.9M        -        -
/system.slice/system-getty.slice/getty@tty1.service												1      -        -        -        -
/system.slice/systemd-journald.service												1      -     1.1G        -        -
/system.slice/systemd-logind.service												1      -    80.0K        -        -
/system.slice/systemd-udevd.service												1      -   924.0K        -        -
/system.slice/xinetd.service												1      -        -        -        -
```



## 六, 控制系统运行等级
### 32. 启动系统救援模式
```shell
# systemctl rescue
Broadcast message from root@tecmint on pts/0 (Wed 2015-04-29 11:31:18 IST):
The system is going down to rescue mode NOW!
```



### 33. 进入紧急模式
```shell
# systemctl emergency
Welcome to emergency mode! After logging in, type "journalctl -xb" to view
system logs, "systemctl reboot" to reboot, "systemctl default" to try again
to boot into default mode.
```



### 34. 列出当前使用的运行等级
```shell
[chenchen@dev3_10.211.21.18 ~]$ systemctl get-default
multi-user.target
```



### 35. 启动运行等级5，即图形模式
```shell
# systemctl isolate runlevel5.target
或
# systemctl isolate graphical.target
```



### 36. 启动运行等级3，即多用户模式(命令行)
```shell
# systemctl isolate runlevel3.target
或
# systemctl isolate multiuser.target

### 36. 设置多用户模式或图形模式为默认运行等级
# systemctl set-default runlevel3.target
# systemctl set-default runlevel5.target
```



### 37. 重启、停止、挂起、休眠系统或使系统进入混合睡眠
```shell
# systemctl reboot
# systemctl halt
# systemctl suspend
# systemctl hibernate
# systemctl hybrid-sleep
对于不知运行等级为何物的人, 说明如下: 
Runlevel 0 : 关闭系统
Runlevel 1 : 救援？维护模式
Runlevel 3 : 多用户, 无图形系统
Runlevel 4 : 多用户, 无图形系统
Runlevel 5 : 多用户, 图形化系统
Runlevel 6 : 关闭并重启机器
```



## journalctl -xe

```shell
$ journalctl -xe
```






## See Also

https://blog.csdn.net/omaidb/article/details/120123405

systemd.service — Service unit configuration

https://www.freedesktop.org/software/systemd/man/systemd.service.html
