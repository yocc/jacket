# Hardware information for Linux



### CPU

```
# 查看 CPU 统计信息
[chenchen@dev3_10.211.21.18 ~]$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3
Thread(s) per core:    1
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 45
Model name:            Intel(R) Xeon(R) CPU E5-2609 0 @ 2.40GHz
Stepping:              7
CPU MHz:               1207.875
BogoMIPS:              4800.00
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              10240K
NUMA node0 CPU(s):     0-3
[chenchen@dev3_10.211.21.18 ~]$
```



```
# 查看每个 CPU 的信息
[chenchen@dev3_10.211.21.18 ~]$ cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 45
model name      : Intel(R) Xeon(R) CPU E5-2609 0 @ 2.40GHz
stepping        : 7
microcode       : 0x710
cpu MHz         : 1208.343
cache size      : 10240 KB
physical id     : 0
siblings        : 4
core id         : 0
cpu cores       : 4
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx lahf_lm arat pln pts dtherm tpr_shadow vnmi flexpriority ept vpid xsaveopt
bogomips        : 4800.00
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:

processor       : 1
vendor_id       : GenuineIntel
cpu family      : 6
model           : 45
model name      : Intel(R) Xeon(R) CPU E5-2609 0 @ 2.40GHz
stepping        : 7
microcode       : 0x710
cpu MHz         : 1213.875
cache size      : 10240 KB
physical id     : 0
siblings        : 4
core id         : 1
cpu cores       : 4
apicid          : 2
initial apicid  : 2
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx lahf_lm arat pln pts dtherm tpr_shadow vnmi flexpriority ept vpid xsaveopt
bogomips        : 4800.00
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:

processor       : 2
vendor_id       : GenuineIntel
cpu family      : 6
model           : 45
model name      : Intel(R) Xeon(R) CPU E5-2609 0 @ 2.40GHz
stepping        : 7
microcode       : 0x710
cpu MHz         : 1276.968
cache size      : 10240 KB
physical id     : 0
siblings        : 4
core id         : 2
cpu cores       : 4
apicid          : 4
initial apicid  : 4
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx lahf_lm arat pln pts dtherm tpr_shadow vnmi flexpriority ept vpid xsaveopt
bogomips        : 4800.00
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:

processor       : 3
vendor_id       : GenuineIntel
cpu family      : 6
model           : 45
model name      : Intel(R) Xeon(R) CPU E5-2609 0 @ 2.40GHz
stepping        : 7
microcode       : 0x710
cpu MHz         : 1258.312
cache size      : 10240 KB
physical id     : 0
siblings        : 4
core id         : 3
cpu cores       : 4
apicid          : 6
initial apicid  : 6
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx lahf_lm arat pln pts dtherm tpr_shadow vnmi flexpriority ept vpid xsaveopt
bogomips        : 4800.00
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:

[chenchen@dev3_10.211.21.18 ~]$
```



### Memory

```
# 查看内存概要情况
[root@dev3_10.211.21.18 ~]# free -m				# 单位MB, 总共 23901MB
              total        used        free      shared  buff/cache   available
Mem:          23901       15378         488        1230        8034        6825
Swap:          8191         531        7660
[root@dev3_10.211.21.18 ~]# 
[root@dev3_10.211.21.18 ~]# free -h				# 单位GB, human
              total        used        free      shared  buff/cache   available
Mem:            23G         15G        722M        1.2G        7.6G        6.7G
Swap:          8.0G        531M        7.5G
[root@dev3_10.211.21.18 ~]#
```



```
# 查看内存详细使用情况
[root@dev3_10.211.21.18 ~]# cat /proc/meminfo
MemTotal:       24475076 kB
MemFree:          458740 kB
MemAvailable:    6951196 kB
Buffers:          481140 kB
Cached:          6692092 kB
SwapCached:       159976 kB
Active:         17973792 kB
Inactive:        4546768 kB
Active(anon):   14777152 kB
Inactive(anon):  1830152 kB
Active(file):    3196640 kB
Inactive(file):  2716616 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:       8388604 kB
SwapFree:        7844856 kB
Dirty:               196 kB
Writeback:             0 kB
AnonPages:      15239132 kB
Mapped:           132348 kB
Shmem:           1259976 kB
Slab:            1057200 kB
SReclaimable:     948760 kB
SUnreclaim:       108440 kB
KernelStack:       16624 kB
PageTables:       209348 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:    20626140 kB
Committed_AS:   26248316 kB
VmallocTotal:   34359738367 kB
VmallocUsed:      337752 kB
VmallocChunk:   34358941692 kB
HardwareCorrupted:    24 kB
AnonHugePages:   1060864 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      365504 kB
DirectMap2M:    14268416 kB
DirectMap1G:    10485760 kB
[root@dev3_10.211.21.18 ~]#
```



```
# 查看内存硬件信息
[root@dev3_10.211.21.18 ~]# dmidecode -t memory
# dmidecode 3.0
Scanning /dev/mem for entry point.
SMBIOS 2.7 present.

Handle 0x1000, DMI type 16, 23 bytes
Physical Memory Array
        Location: System Board Or Motherboard
        Use: System Memory
        Error Correction Type: Multi-bit ECC
        Maximum Capacity: 768 GB
        Error Information Handle: Not Provided
        Number Of Devices: 24

Handle 0x1100, DMI type 17, 34 bytes
Memory Device
        Array Handle: 0x1000
        Error Information Handle: Not Provided
        Total Width: 72 bits
        Data Width: 64 bits
        Size: 4096 MB
        Form Factor: DIMM
        Set: 1
        Locator: DIMM_A1 
        Bank Locator: Not Specified
        Type: DDR3
        Type Detail: Synchronous Registered (Buffered)
        Speed: 1333 MHz
        Manufacturer: 00AD00B300AD
        Serial Number: 3BAF3672
        Asset Tag: 01124463
        Part Number: HMT351R7CFR4A-H9  
        Rank: 1
        Configured Clock Speed: 1067 MHz

Handle 0x1101, DMI type 17, 34 bytes
Memory Device
        Array Handle: 0x1000
        Error Information Handle: Not Provided
        Total Width: 72 bits
        Data Width: 64 bits
        Size: 4096 MB
        Form Factor: DIMM
......
```



### Disk

```
# 查看硬盘和分区分布
[root@dev3_10.211.21.18 ~]# lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 278.9G  0 disk 
├─sda1   8:1    0     1M  0 part 
├─sda2   8:2    0   200M  0 part /boot
├─sda3   8:3    0    12G  0 part /usr
├─sda4   8:4    0     1K  0 part 
├─sda5   8:5    0     8G  0 part [SWAP]
├─sda6   8:6    0     8G  0 part /tmp
├─sda7   8:7    0     8G  0 part /var
├─sda8   8:8    0     4G  0 part /
└─sda9   8:9    0 238.7G  0 part /data0
sdb      8:16   0 836.6G  0 disk 
└─sdb1   8:17   0 836.6G  0 part /data1
[root@dev3_10.211.21.18 ~]#
```



```
# 查看硬盘和分区的详细信息
[root@dev3_10.211.21.18 ~]# fdisk -l

Disk /dev/sda: 299.4 GB, 299439751168 bytes, 584843264 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000ca809

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1            2048        4095        1024   83  Linux
/dev/sda2   *        4096      413695      204800   83  Linux
/dev/sda3          413696    25581567    12583936   83  Linux
/dev/sda4        25581568   584843263   279630848    5  Extended
/dev/sda5        25583616    42360831     8388608   82  Linux swap / Solaris
/dev/sda6        42362880    59140095     8388608   83  Linux
/dev/sda7        59142144    75919359     8388608   83  Linux
/dev/sda8        75921408    84310015     4194304   83  Linux
/dev/sda9        84312064   584843263   250265600   83  Linux

Disk /dev/sdb: 898.3 GB, 898319253504 bytes, 1754529792 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xd7bb7d39

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048  1754529791   877263872   83  Linux
[root@dev3_10.211.21.18 ~]#
```



### 网卡

```
# 查看网卡硬件信息
```



```
# 查看系统的所有网络接口
[root@dev3_10.211.21.18 ~]# ifconfig -a
em1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.211.21.18  netmask 255.255.255.0  broadcast 10.211.21.255
        ether 90:b1:1c:20:1e:3d  txqueuelen 1000  (Ethernet)
        RX packets 41902874406  bytes 36685606382020 (33.3 TiB)
        RX errors 0  dropped 11614  overruns 0  frame 0
        TX packets 36652866516  bytes 16166245436436 (14.7 TiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 33  

em2: flags=4098<BROADCAST,MULTICAST>  mtu 1500
        ether 90:b1:1c:20:1e:3e  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 40  

em3: flags=4098<BROADCAST,MULTICAST>  mtu 1500
        ether 90:b1:1c:20:1e:3f  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 41  

em4: flags=4098<BROADCAST,MULTICAST>  mtu 1500
        ether 90:b1:1c:20:1e:40  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 42  

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1  (Local Loopback)
        RX packets 2357814819  bytes 3056491133241 (2.7 TiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2357814819  bytes 3056491133241 (2.7 TiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:df:fd:0e  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0-nic: flags=4098<BROADCAST,MULTICAST>  mtu 1500
        ether 52:54:00:df:fd:0e  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[root@dev3_10.211.21.18 ~]#
[root@dev3_10.211.21.18 ~]# ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: em1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT qlen 1000
    link/ether 90:b1:1c:20:1e:3d brd ff:ff:ff:ff:ff:ff
3: em2: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether 90:b1:1c:20:1e:3e brd ff:ff:ff:ff:ff:ff
4: em3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether 90:b1:1c:20:1e:3f brd ff:ff:ff:ff:ff:ff
5: em4: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether 90:b1:1c:20:1e:40 brd ff:ff:ff:ff:ff:ff
6: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:df:fd:0e brd ff:ff:ff:ff:ff:ff
7: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:df:fd:0e brd ff:ff:ff:ff:ff:ff
[root@dev3_10.211.21.18 ~]#
[root@dev3_10.211.21.18 ~]# cat /proc/net/dev
Inter-|   Receive                                                |  Transmit
 face |bytes    packets errs drop fifo frame compressed multicast|bytes    packets errs drop fifo colls carrier compressed
    lo: 3056504321622 2357825134    0    0    0     0          0         0 3056504321622 2357825134    0    0    0     0       0          0
virbr0-nic:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
virbr0:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
   em4:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
   em2:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
   em3:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
   em1: 36685607434894 41902878954    0 11614    0     0          0  35147195 16166249518657 36652872053    0    0    0     0       0          0
[root@dev3_10.211.21.18 ~]#
```



```
# 查看某个网络接口的详细信息
[root@dev3_10.211.21.18 ~]# ethtool em1
Settings for em1:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full 
                                100baseT/Half 100baseT/Full 
                                1000baseT/Half 1000baseT/Full 
        Supported pause frame use: No
        Supports auto-negotiation: Yes
        Advertised link modes:  10baseT/Half 10baseT/Full 
                                100baseT/Half 100baseT/Full 
                                1000baseT/Half 1000baseT/Full 
        Advertised pause frame use: Symmetric
        Advertised auto-negotiation: Yes
        Link partner advertised link modes:  10baseT/Half 10baseT/Full 
                                             100baseT/Half 100baseT/Full 
                                             1000baseT/Full 
        Link partner advertised pause frame use: No
        Link partner advertised auto-negotiation: Yes
        Speed: 1000Mb/s
        Duplex: Full
        Port: Twisted Pair
        PHYAD: 1
        Transceiver: internal
        Auto-negotiation: on
        MDI-X: on
        Supports Wake-on: g
        Wake-on: d
        Current message level: 0x000000ff (255)
                               drv probe link timer ifdown ifup rx_err tx_err
        Link detected: yes
[root@dev3_10.211.21.18 ~]#
```



### PCI

```
# 查看 PCI 信息, 即主板所有硬件槽信息
[root@dev3_10.211.21.18 ~]# lspci
00:00.0 Host bridge: Intel Corporation Xeon E5/Core i7 DMI2 (rev 07)
00:01.0 PCI bridge: Intel Corporation Xeon E5/Core i7 IIO PCI Express Root Port 1a (rev 07)
00:01.1 PCI bridge: Intel Corporation Xeon E5/Core i7 IIO PCI Express Root Port 1b (rev 07)
00:02.0 PCI bridge: Intel Corporation Xeon E5/Core i7 IIO PCI Express Root Port 2a (rev 07)
00:02.2 PCI bridge: Intel Corporation Xeon E5/Core i7 IIO PCI Express Root Port 2c (rev 07)
00:03.0 PCI bridge: Intel Corporation Xeon E5/Core i7 IIO PCI Express Root Port 3a in PCI Express Mode (rev 07)
00:05.0 System peripheral: Intel Corporation Xeon E5/Core i7 Address Map, VTd_Misc, System Management (rev 07)
00:05.2 System peripheral: Intel Corporation Xeon E5/Core i7 Control Status and Global Errors (rev 07)
00:11.0 PCI bridge: Intel Corporation C600/X79 series chipset PCI Express Virtual Root Port (rev 05)
00:16.0 Communication controller: Intel Corporation C600/X79 series chipset MEI Controller #1 (rev 05)
00:16.1 Communication controller: Intel Corporation C600/X79 series chipset MEI Controller #2 (rev 05)
00:1a.0 USB controller: Intel Corporation C600/X79 series chipset USB2 Enhanced Host Controller #2 (rev 05)
00:1c.0 PCI bridge: Intel Corporation C600/X79 series chipset PCI Express Root Port 1 (rev b5)
00:1c.7 PCI bridge: Intel Corporation C600/X79 series chipset PCI Express Root Port 8 (rev b5)
00:1d.0 USB controller: Intel Corporation C600/X79 series chipset USB2 Enhanced Host Controller #1 (rev 05)
00:1e.0 PCI bridge: Intel Corporation 82801 PCI Bridge (rev a5)
00:1f.0 ISA bridge: Intel Corporation C600/X79 series chipset LPC Controller (rev 05)
00:1f.2 SATA controller: Intel Corporation C600/X79 series chipset 6-Port SATA AHCI Controller (rev 05)
01:00.0 Ethernet controller: Broadcom Limited NetXtreme BCM5720 Gigabit Ethernet PCIe
01:00.1 Ethernet controller: Broadcom Limited NetXtreme BCM5720 Gigabit Ethernet PCIe
02:00.0 Ethernet controller: Broadcom Limited NetXtreme BCM5720 Gigabit Ethernet PCIe
02:00.1 Ethernet controller: Broadcom Limited NetXtreme BCM5720 Gigabit Ethernet PCIe
03:00.0 RAID bus controller: LSI Logic / Symbios Logic MegaRAID SAS 2208 [Thunderbolt] (rev 01)
08:00.0 PCI bridge: Renesas Technology Corp. SH7757 PCIe Switch [PS]
09:00.0 PCI bridge: Renesas Technology Corp. SH7757 PCIe Switch [PS]
09:01.0 PCI bridge: Renesas Technology Corp. SH7757 PCIe Switch [PS]
0a:00.0 PCI bridge: Renesas Technology Corp. SH7757 PCIe-PCI Bridge [PPB]
0b:00.0 VGA compatible controller: Matrox Electronics Systems Ltd. G200eR2
3f:08.0 System peripheral: Intel Corporation Xeon E5/Core i7 QPI Link 0 (rev 07)
3f:09.0 System peripheral: Intel Corporation Xeon E5/Core i7 QPI Link 1 (rev 07)
3f:0a.0 System peripheral: Intel Corporation Xeon E5/Core i7 Power Control Unit 0 (rev 07)
3f:0a.1 System peripheral: Intel Corporation Xeon E5/Core i7 Power Control Unit 1 (rev 07)
3f:0a.2 System peripheral: Intel Corporation Xeon E5/Core i7 Power Control Unit 2 (rev 07)
3f:0a.3 System peripheral: Intel Corporation Xeon E5/Core i7 Power Control Unit 3 (rev 07)
3f:0b.0 System peripheral: Intel Corporation Xeon E5/Core i7 Interrupt Control Registers (rev 07)
3f:0b.3 System peripheral: Intel Corporation Xeon E5/Core i7 Semaphore and Scratchpad Configuration Registers (rev 07)
3f:0c.0 System peripheral: Intel Corporation Xeon E5/Core i7 Unicast Register 0 (rev 07)
3f:0c.1 System peripheral: Intel Corporation Xeon E5/Core i7 Unicast Register 0 (rev 07)
3f:0c.6 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller System Address Decoder 0 (rev 07)
3f:0c.7 System peripheral: Intel Corporation Xeon E5/Core i7 System Address Decoder (rev 07)
3f:0d.0 System peripheral: Intel Corporation Xeon E5/Core i7 Unicast Register 0 (rev 07)
3f:0d.1 System peripheral: Intel Corporation Xeon E5/Core i7 Unicast Register 0 (rev 07)
3f:0d.6 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller System Address Decoder 1 (rev 07)
3f:0e.0 System peripheral: Intel Corporation Xeon E5/Core i7 Processor Home Agent (rev 07)
3f:0e.1 Performance counters: Intel Corporation Xeon E5/Core i7 Processor Home Agent Performance Monitoring (rev 07)
3f:0f.0 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Registers (rev 07)
3f:0f.1 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller RAS Registers (rev 07)
3f:0f.2 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Target Address Decoder 0 (rev 07)
3f:0f.3 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Target Address Decoder 1 (rev 07)
3f:0f.4 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Target Address Decoder 2 (rev 07)
3f:0f.5 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Target Address Decoder 3 (rev 07)
3f:0f.6 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Target Address Decoder 4 (rev 07)
3f:10.0 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Channel 0-3 Thermal Control 0 (rev 07)
3f:10.1 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Channel 0-3 Thermal Control 1 (rev 07)
3f:10.2 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller ERROR Registers 0 (rev 07)
3f:10.3 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller ERROR Registers 1 (rev 07)
3f:10.4 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Channel 0-3 Thermal Control 2 (rev 07)
3f:10.5 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller Channel 0-3 Thermal Control 3 (rev 07)
3f:10.6 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller ERROR Registers 2 (rev 07)
3f:10.7 System peripheral: Intel Corporation Xeon E5/Core i7 Integrated Memory Controller ERROR Registers 3 (rev 07)
3f:11.0 System peripheral: Intel Corporation Xeon E5/Core i7 DDRIO (rev 07)
3f:13.0 System peripheral: Intel Corporation Xeon E5/Core i7 R2PCIe (rev 07)
3f:13.1 Performance counters: Intel Corporation Xeon E5/Core i7 Ring to PCI Express Performance Monitor (rev 07)
3f:13.4 Performance counters: Intel Corporation Xeon E5/Core i7 QuickPath Interconnect Agent Ring Registers (rev 07)
3f:13.5 Performance counters: Intel Corporation Xeon E5/Core i7 Ring to QuickPath Interconnect Link 0 Performance Monitor (rev 07)
3f:13.6 System peripheral: Intel Corporation Xeon E5/Core i7 Ring to QuickPath Interconnect Link 1 Performance Monitor (rev 07)
[root@dev3_10.211.21.18 ~]#
[root@dev3_10.211.21.18 ~]# lspci -v					# 更详细信息
[root@dev3_10.211.21.18 ~]# lspci -vv
```



```
# 查看设备树
[root@dev3_10.211.21.18 ~]# lspci -t
-+-[0000:3f]-+-08.0
 |           +-09.0
 |           +-0a.0
 |           +-0a.1
 |           +-0a.2
 |           +-0a.3
 |           +-0b.0
 |           +-0b.3
 |           +-0c.0
 |           +-0c.1
 |           +-0c.6
 |           +-0c.7
 |           +-0d.0
 |           +-0d.1
 |           +-0d.6
 |           +-0e.0
 |           +-0e.1
 |           +-0f.0
 |           +-0f.1
 |           +-0f.2
 |           +-0f.3
 |           +-0f.4
 |           +-0f.5
 |           +-0f.6
 |           +-10.0
 |           +-10.1
 |           +-10.2
 |           +-10.3
 |           +-10.4
 |           +-10.5
 |           +-10.6
 |           +-10.7
 |           +-11.0
 |           +-13.0
 |           +-13.1
 |           +-13.4
 |           +-13.5
 |           \-13.6
 \-[0000:00]-+-00.0
             +-01.0-[02]--+-00.0
             |            \-00.1
             +-01.1-[01]--+-00.0
             |            \-00.1
             +-02.0-[04]--
             +-02.2-[03]----00.0
             +-03.0-[05]--
             +-05.0
             +-05.2
             +-11.0-[06]--
             +-16.0
             +-16.1
             +-1a.0
             +-1c.0-[07]--
             +-1c.7-[08-0c]----00.0-[09-0c]--+-00.0-[0a-0b]----00.0-[0b]----00.0
             |                               \-01.0-[0c]--
             +-1d.0
             +-1e.0-[0d]--
             +-1f.0
             \-1f.2
[root@dev3_10.211.21.18 ~]#
```



### USB

```
# 查看 USB 信息
[root@dev3_10.211.21.18 ~]# lsusb
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 005: ID 0624:0249 Avocent Corp. Virtual Keyboard/Mouse
Bus 001 Device 004: ID 0624:0248 Avocent Corp. Virtual Hub
Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
[root@dev3_10.211.21.18 ~]#
[root@dev3_10.211.21.18 ~]# lsusb -t					# 查看系统中 USB 拓扑, 类似 cat /sys/kernel/debug/usb/devices
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=ehci-pci/2p, 480M
    |__ Port 1: Dev 2, If 0, Class=Hub, Driver=hub/8p, 480M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=ehci-pci/2p, 480M
    |__ Port 1: Dev 2, If 0, Class=Hub, Driver=hub/6p, 480M
        |__ Port 6: Dev 4, If 0, Class=Hub, Driver=hub/6p, 480M
            |__ Port 1: Dev 5, If 0, Class=Human Interface Device, Driver=usbhid, 480M
            |__ Port 1: Dev 5, If 1, Class=Human Interface Device, Driver=usbhid, 480M
            |__ Port 1: Dev 5, If 2, Class=Human Interface Device, Driver=usbhid, 480M
[root@dev3_10.211.21.18 ~]#
[root@dev3_10.211.21.18 ~]# lspci -v					# 查看更多详细信息
```



```
# 更多设备商的 VID 信息
[root@dev3_10.211.21.18 ~]# cat /var/lib/usbutils/usb.ids
cat: /var/lib/usbutils/usb.ids: No such file or directory
[root@dev3_10.211.21.18 ~]#
```



### 查看所有硬件摘要信息

```
lshw -html > /hardware.html
```



### 查看 SCSI 控制器设备的信息

```
[root@dev3_10.211.21.18 ~]# lsscsi
[0:2:0:0]    disk    DELL     PERC H710        3.13  /dev/sda 
[0:2:1:0]    disk    DELL     PERC H710        3.13  /dev/sdb 
[root@dev3_10.211.21.18 ~]#
# 插入U盘后会多出一条信息
```



### 查看 BIOS 信息

```
# 查看 BIOS 信息
[root@dev3_10.211.21.18 ~]# dmidecode -t bios
# dmidecode 3.0
Scanning /dev/mem for entry point.
SMBIOS 2.7 present.

Handle 0x0000, DMI type 0, 24 bytes
BIOS Information
        Vendor: Dell Inc.
        Version: 1.3.6
        Release Date: 09/11/2012
        Address: 0xF0000
        Runtime Size: 64 kB
        ROM Size: 8192 kB
        Characteristics:
                ISA is supported
                PCI is supported
                PNP is supported
                BIOS is upgradeable
                BIOS shadowing is allowed
                Boot from CD is supported
                Selectable boot is supported
                EDD is supported
                Japanese floppy for Toshiba 1.2 MB is supported (int 13h)
                5.25"/360 kB floppy services are supported (int 13h)
                5.25"/1.2 MB floppy services are supported (int 13h)
                3.5"/720 kB floppy services are supported (int 13h)
                8042 keyboard services are supported (int 9h)
                Serial services are supported (int 14h)
                CGA/mono video services are supported (int 10h)
                ACPI is supported
                USB legacy is supported
                BIOS boot specification is supported
                Function key-initiated network boot is supported
                Targeted content distribution is supported
                UEFI is supported
        BIOS Revision: 1.3

Handle 0x0D00, DMI type 13, 22 bytes
BIOS Language Information
        Language Description Format: Long
        Installable Languages: 1
                en|US|iso8859-1
        Currently Installed Language: en|US|iso8859-1

[root@dev3_10.211.21.18 ~]#
```



```
# dmidecode 以一种可读的方式 dump 出机器的 DMI(Desktop Management Interface) 信息.
# 这些信息包括了硬件以及 BIOS, 既可以得到当前的配置, 也可以得到系统支持的最大配置, 比如说支持的最大内存数等.
[root@dev3_10.211.21.18 ~]# dmidecode -q					# 查看所有有用的信息, 里面包含了很多硬件信息.
BIOS Information
        Vendor: Dell Inc.
        Version: 1.3.6
        Release Date: 09/11/2012
        Address: 0xF0000
        Runtime Size: 64 kB
        ROM Size: 8192 kB
        Characteristics:
                ISA is supported
                PCI is supported
                PNP is supported
                BIOS is upgradeable
                BIOS shadowing is allowed
                Boot from CD is supported
                Selectable boot is supported
                EDD is supported
                Japanese floppy for Toshiba 1.2 MB is supported (int 13h)
                5.25"/360 kB floppy services are supported (int 13h)
                5.25"/1.2 MB floppy services are supported (int 13h)
                3.5"/720 kB floppy services are supported (int 13h)
                8042 keyboard services are supported (int 9h)
                Serial services are supported (int 14h)
                CGA/mono video services are supported (int 10h)
                ACPI is supported
                USB legacy is supported
                BIOS boot specification is supported
                Function key-initiated network boot is supported
                Targeted content distribution is supported
                UEFI is supported
        BIOS Revision: 1.3

System Information
        Manufacturer: Dell Inc.
        Product Name: PowerEdge R620
        Version: Not Specified
        Serial Number: H6XK4W1
        UUID: 4C4C4544-0036-5810-804B-C8C04F345731
        Wake-up Type: Power Switch
        SKU Number: SKU=NotProvided;ModelName=PowerEdge R620
        Family: Not Specified

Base Board Information
        Manufacturer: Dell Inc.
        Product Name: 036FVD
......
```

