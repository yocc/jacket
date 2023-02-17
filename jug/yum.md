# yum

---

[toc]

### 配置

```shell
YUM 配置
yum 的配置有两个地方:
1. (yum 软件的全局配置) 直接配置 /etc/yum.conf 文件
2. (具体仓库配置) 在 /etc/yum.repos.d 目录下的 .repo 文件, 用来放自己的仓库

1. 
include=http://repos.sina.cn/conf/yumconf.php    // 可以直接通过 include 的方式 引用 外部 yum.conf 文件, 来代替本地下面的文件
[main]
cachedir=/var/cache/yum         # yum下载的RPM包的缓存目录
keepcache=0             				# 缓存是否保存，1保存，0不保存。
debuglevel=2             				# 调试级别(0-10)，默认为2(具体调试级别的应用，我也不了解)。
logfile=/var/log/yum.log        # yum的日志文件所在的位置
exactarch=1             # 在更新的时候，是否允许更新不同版本的RPM包，比如是否在i386上更新i686的RPM包。
obsoletes=1             # 这是一个update的参数，具体请参阅yum(8)，简单的说就是相当于upgrade，允许更新陈旧的RPM包。
gpgcheck=1             	# 是否检查GPG(GNU Private Guard)，一种密钥方式签名。
plugins=1             	# 是否允许使用插件，默认是0不允许，但是我们一般会用yum-fastestmirror这个插件。
installonly_limit=3     # 允许保留多少个内核包。
exclude=selinux*        # 屏蔽不想更新的RPM包，可用通配符，多个RPM包之间使用空格分离。
#       This is the default, if you make this bigger yum won't see if the metadata
# is newer on the remote and so you'll "gain" the bandwidth of not having to
# download the new metadata and "pay" for it by yum not having correct
# information.
# It is esp. important, to have correct metadata, for distributions like
# Fedora which don't keep old packages around. If you don't like this checking
# interupting your command line usage, it's much better to have something
# manually check the metadata once an hour (yum-updatesd will do this).
# metadata_expire=90m
# PUT YOUR REPOS HERE or IN separate files named file.repo        // 自己的仓库放这里 /etc/yum.repos.d/xx.repo , 并用文件名区分 xx.repo
# in /etc/yum.repos.d

[main]
cachedir：yum缓存的目录，yum在此存储下载的rpm包和数据库，一般是/var/cache/yum。
debuglevel：除错级别，0-10,默认是2。
logfile：yum的日志文件，默认是/var/log/yum.log。
pkgpolicy：包的策略。一共有两个选项，newest和last，这个作用是如果你设置了多个repository，而同一软件在不同的repository中同时存在，yum应该安装哪一个，如果是newest，则yum会安装最新的那个版本。如果是last，则yum会将服务器id以字母表排序，并选择最后的那个服务器上的软件安装。一般都是选newest。
distroverpkg：指定一个软件包，yum会根据这个包判断你的发行版本，默认是redhat-release，也可以是安装的任何针对自己发行版的rpm包。
exactarch，有两个选项1和0,代表是否只升级和你安装软件包cpu体系一致的包，如果设为1，则如你安装了一个i386的rpm，则yum不会用1686的包来升级。
retries，网络连接发生错误后的重试次数，如果设为0，则会无限重试。
tolerent，也有1和0两个选项，表示yum是否容忍命令行发生与软件包有关的错误，比如你要安装1,2,3三个包，而其中3此前已经安装了，如果你设为1,则yum不会出现错误信息。默认是0。
除了上述之外，还有一些可以添加的选项，如:
exclude=，排除某些软件在升级名单之外，可以用通配符，列表中各个项目要用空格隔开，这个对于安装了诸如美化包，中文补丁的朋友特别有用。
gpgchkeck= 有1和0两个选择，分别代表是否是否进行gpg校验，如果没有这一项，默认好像也是检查的。

2. 
什么是 repo 文件？
repo文件是 Fedora 中 yum 源（软件仓库）的配置文件，通常一个repo文件定义了一个或者多个软件仓库的细节内容，例如我们将从哪里下载需要安装或者升级的软件包，repo文件中的设置内容将被yum读取和应用！
我们以一份系统自带的repo文件做为实例来探讨（#号后面是我加的注释）：
[fedora]      #方括号里面的是软件源的名称，将被yum取得并识别
name=Fedora $releasever - $basearch   #这里也定义了软件 仓库的名称，通常是为了方便阅读配置文件，一般没什么作用，$releasever变量定义了发行版本，通常是8，9，10等数字，$basearch变 量定义了系统的架构，可以是i386、x86_64、ppc等值，这两个变量根据当前系统的版本架构不同而有不同的取值，这可以方便yum升级的时候选择 适合当前系统的软件包，以下同……
failovermethod=priority   #failovermethod 有两个值可以选择，priority是默认值，表示从列出的baseurl中顺序选择镜像服务器地址，roundrobin表示在列出的服务器中随机选择
exclude=compiz* *compiz* fusion-icon* #exclude这个选项是后来我自己加上去的，用来禁止这个软件仓库中的某些软件包的安装和更新，可以使用通配符，并以空格分隔，可以视情况需要自行添加
#baseurl=http://download.fedoraproject.org/pub/fedora/linux/releases/$releasever/Everything/$basearch/os/
#上面的一行baseurl第一个字符是'#'表示该行已经被注释，将不会被读取，这一行的意思是指定一个baseurl（源的镜像服务器地址）
#mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=fedora-$releasever&arch=$basearch
#上面的这一行是指定一个镜像服务器的地址列表，通常是开启的，本例中加了注释符号禁用了，我们可以试试，将$releasever和$basearch替换成自己对应的版本和架构，例如10和i386，在浏览器中打开，我们就能看到一长串镜可用的镜像服务器地址列表。
选择自己访问速度较快的镜像服务器地址复制并粘贴到repo文件中，我们就能获得较快的更新速度了，格式如下baseurl所示：
baseurl=
ftp://ftp.sfc.wide.ad.jp/pub/Linux/Fedora/releases/10/Everything/i386/os
http://ftp.chg.ru/pub/Linux/fedora/linux/releases/10/Everything/i386/os
http://ftp.yz.yamagata-u.ac.jp/pub/linux/fedora/linux/releases/10/Everything/i386/os
http://mirror.nus.edu.sg/fedora/releases/10/Everything/i386/os
http://mirror.yandex.ru/fedora/linux/releases/10/Everything/i386/os
http://ftp.twaren.net/Linux/Fedora/linux/releases/10/Everything/i386/os
http://ftp.itu.edu.tr/Mirror/Fedora/linux/releases/10/Everything/i386/os

enabled=1		# 这个选项表示这个repo中定义的源是启用的，0为禁用
gpgcheck=1 	# 这个选项表示这个repo中下载的rpm将进行gpg的校验，已确定rpm包的来源是有效和安全的
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$basearch		# 定义用于校验的gpg密钥
```

```shell
// 例子: 
-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-
文件位置: /etc/yum.repos.d/nux-dextop.repo
[nux-dextop]
name=Nux.Ro RPMs for general desktop use
baseurl=http://li.nux.ro/download/nux/dextop/el7/$basearch/ http://mirror.li.nux.ro/li.nux.ro/nux/dextop/el7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-nux.ro
protect=0

[Steven]只是仓库名字，这个可以随意
Name=….这个你可以理解为仓库的描述,这个可以不写这一行
Baseurl＝file:///Media/Server，这里解释一下为什么是///三个/，file:// ftp:// http://大家是不是很熟悉，file://的意思是文件在本地，在Linux中一切都以根开始的那路径上要加个/，所以最后是file:///media/Server,意思是在本地的/media/Server下
Enable=1，这里是说是否用户仓库，1是启用，0是不启用
Gpgcheck=0, 是说是否检查软件的KEY，我一般都不检查，各位随意
Gpgkey=…这里是说你的KEY文件在哪里，我不启用，所以也无所谓了
protect=0, 默认为1; 只有那些拥有 protect=1 的软件库才能更新来获 protect=1 保护的软件库的软件。
-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-$-
```



### 插件

```shell
// 加速 yum 更新速度
// 安装 yum 的 fast mirror插件, 可以加快 CentOS/RHEL yum 的速度和提高稳定性, 效果显著.
# yum -y install yum-fastestmirror							// Centos5
# yum -y install yum-plugin-fastestmirror				// Centos4

# yum -y update			// 升级包括 kernel; 系统更新(更新所有可以升级的rpm包, 包括 kernel)

// FAQ
# yum install 的时候提示: Loaded plugins: fastestmirror
fastestmirror 是 yum 的一个加速插件, 这里是插件提示信息是插件不能用了.
不能用就先别用呗, 禁用掉, 先继续 yum 了再说. 
以下是如何禁用:
1. 修改插件的配置文件
# vi  /etc/yum/pluginconf.d/fastestmirror.conf  
enabled=1					// 由 1 改为 0, 禁用该插件
2. 修改yum的配置文件
# vi /etc/yum.conf
plugins=1						// 改为 0, 不使用插件
```



### 仓库

```shell
.repo 文件, 字段说明: 
[nux-dextop]: 仓库名字
name: 描述性文字
baseurl: 仓库地址
enabled: 是否默认开启, 不写默认为开启
gpgcheck:  是否检查GPG(GNU Private Guard)，一种密钥方式签名
gpgkey:
protect: 更新相关
```

```shell
直接找一个 repo 文件
cd /etc/yum.repos.d/
wget http://mirrors.163.com/.help/CentOS-Base-163.repo
yum makecache  //生成缓存
yum update
```

```shell
通过安装特定插件来自动找最快的镜像
安装好yum-fastestmirror后,每次用yum安装就会自动检查速度最快的镜像了
yum install yum-fastestmirror
yum clean all
安装好yum-fastestmirror后,每次用yum安装就会自动检查速度最快的镜像了
```

```shell
刷新仓库
# yum clean								// 
# yum clean all           // 刷新仓库配置
# yum clean 包名					// *yum暂存(/var/cache/yum/)的相关参数, 清除暂存中 rpm 包文件
# yum clean 或 # yum clean all			// 清除暂存中旧的 rpm headers(头)文件和包文件; 注: 相当于 yum clean packages + yum clean oldheaders

# yum clean headers				// 清除暂存中 rpm headers(头)文件
# yum clean oldheaders		// 清除暂存中旧的 rpm headers(头)文件
# yum repolist all        // 报告yum仓库的状态
```



### 查询

```shell
# yum											// 查看 软件 来自哪个 仓库

# yum repolist
# yum repolist all			  // 显示这个被配置的软件仓库
# yum repolist sinasrv3	  // 列出所有启用的软件仓库的ID, 名称及其包含的软件包的数量
# yum grouplist					  // 列出所有软件包组

// 搜索
# yum search vim				  // 查询 vim 软件包; 搜索匹配特定字符串的 rpm 包; 注: 在 rpm 包名, 包描述等中搜索
# yum search vim-enhanced.x86_64
# yum provides ~					// 列出软件包提供哪些文件; 搜索有包含特定文件名的rpm包

# yum list								// *.rpm 包列表; 列出资源库中所有可以安装或更新的 rpm 包
# yum list ～						 // 列出所指定软件包
# yum list | grep vim
# yum list mozilla				// 列出资源库中特定的可以安装或更新以及已经安装的 rpm 包
# yum list mozilla*				// 注: 可以在 rpm 包名中使用匹配符, 如列出所有以 mozilla 开头的 rpm 包
# yum list available			// 列出所有可用的包
# yum list updates			  // 列出所有可更新的软件包; 列出资源库中所有可以更新的 rpm 包
# yum list installed			// 列出所有已安装的软件包; 列出已经安装的所有的 rpm 包
# yum list extras					// 列出所有已安装但不在 Yum Repository 內的软件包; 注: 通过其它网站下载安装的 rpm 包

// info 同 list 参数
# yum info 								// 列出所有软件包的信息
# yum info ～						 // 使用YUM获取软件包信息
# yum info updates				// 列出所有可更新的软件包信息
# yum info installed			// 列出所有已安裝的软件包信息
# yum info extras					// 列出所有已安裝但不在Yum Repository 內的软件包信息
```

```shell
yum针对软件包操作常用命令：
1.使用YUM查找软件包命令：yum search 
2.列出所有可安装的软件包命令：yum list
3.列出所有可更新的软件包命令：yum list updates
4.列出所有已安装的软件包命令：yum list installed
5.列出所有已安装但不在 Yum Repository 内的软件包命令：yum list extras
6.列出所指定的软件包命令：yum list
7.使用YUM获取软件包信息命令：yum info
8.列出所有软件包的信息命令：yum info
9.列出所有可更新的软件包信息命令：yum info updates
10.列出所有已安装的软件包信息命令：yum info installed
11.列出所有已安装但不在 Yum Repository 内的软件包信息命令：yum info extras
12.列出软件包提供哪些文件命令：yum provides

重装软件包
[chenchen@grpc01 Python-3.11.1]$ sudo yum -y reinstall tk tk-devel
```



### 安装

```shell

# yum -y install ffmpeg ffmpeg-devel --enablerepo=nux-dextop		// 通过制定仓库来安装 FFmpeg 和 FFmpeg-devel 包
[root@Linux modules]# yum install subversion --enablerepo=WandiscoSVN			// 安装 SVN
# yum install vim-enhanced.x86_64
# yum install xmms-mp3																					// 安装rpm包,如xmms-mp3
[chenchen@localhost ~]$ sudo yum update bind-utils.x86_64				// 安装 dig
```



### 更新

```shell
# yum check-update					// *.rpm 包的更新, 检查可更新的 rpm 包; 查看那些可以升级, 返回 第二列是升级后的版本, 最后一列是来自哪里仓库

# yum update								// 升级全部; update改变软件设置和系统设置, 系统版本内核都升级; update主要是使软件达到最新, 会频繁的发布升级;
# yum update bash.x86_64
# yum update kernel kernel-source		// 更新指定的 rpm 包, 如更新 kernel 和 kernel source

# yum upgrade								// 大规模的版本升级, 与 yum update 不同的是, 连旧的淘汰的包也升级; upgrade不改变软件设置和系统设置, 系统版本升级, 内核不改变; upgrade 更侧重的是软件功能得到一个很大的提升, 不会频繁的升级;
```



### 卸载

```shell
[root@Linux modules]# yum remove subversion
# yum remove licq			// 删除 rpm 包, 包括与该包有倚赖性的包; 注: 同时会提示删除 licq-gnome, licq-qt, licq-text
```



### 服务

```shell
// 每天定期执行系统更新
# chkconfig yum on
# service yum start
```



### 杂项

```shell
安装 rz sz
yum -y install lrzsz
```



### 常用命令

```shell
yum安装常用软件的命令
#yum check-update
#yum remove 软件包名
#yum install 软件包名
#yum update 软件包名

    参数				说明

check-update	显示可升级的软件包
clean	删除下载后的旧的header。和clean all相同
clean oldheaders	删除旧的headers
clean packages	删除下载后的软件包
info	显示可用软件包信息
info 软件包名	显示指定软件包信息
install 软件包名	安装指定软件包
list	显示可用软件包
list installed	显示安装了的软件包
list updates	显示可升级的软件包
provides 软件包名	显示软件包所包含的文件
remove 软件包名	删除制定的软件包，确认判定指定软件包的依存关系。
 
search 关键字	利用关键字搜索软件包。搜索对象是，RPM文件名，Packager（包），Dummary，Description的各型
 
update	升级所有的可升级的软件包
update 软件包名	升级指定的软件包

yum -y install httpd　 ← 在线安装httpd Apache服务器及相关组件
yum -y install php　 ← 在线安装PHP
yum -y install mysql-server　 ← 安装MySQL
yum -y install php-mysql　 ← 安装php-mysql
```



### 案例 1

```shell
# 查看本地系统已经安装的软件
[chenchen@grpc01 ~]$ sudo rpm -qa | grep ssl
openssl-1.0.2k-24.el7_9.x86_64
openssl-devel-1.0.2k-24.el7_9.x86_64
openssl-libs-1.0.2k-24.el7_9.x86_64

# 查看系统本
[chenchen@grpc01 ~]$ cat /proc/version
Linux version 3.10.0-1062.el7.x86_64 (mockbuild@kbuilder.bsys.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC) ) #1 SMP Wed Aug 7 18:08:02 UTC 2019
[chenchen@grpc01 ~]$ cat /etc/redhat-release
CentOS Linux release 7.7.1908 (Core)
```

### 案例 2

```shell
# 机器上以前手动删除了 yum 安装的 python3, 导致 新装python3不管是源码编译安装还是 yum 安装均有问题. 同时 OpenSSL 可能也有问题, 因为同一机器上, 除了系统自带的 OpenSSL 之外, 还有自己安装的 OpenSSL3.x 最新版版. 本案例就是为了解决这些混合问题, 我们会从分析开始, 一步一步最终让整个系统健康起来.

1. 先看一下本地系统 yum, rpm 都装了啥
[chenchen@grpc01 ~]$ sudo rpm -qa | grep python
...
python3-3.6.8-18.el7.x86_64
...

2. 试图绕过原有系统安装的 python3-3.6.8-18.el7.x86_64 而直接安装一个新的 python3-3.6.8-10.el7.i686.rpm
	 提示错误, 依赖一系列库或者软件.
[chenchen@grpc01 tmp]$ sudo rpm -ivh python3-3.6.8-10.el7.i686.rpm
error: Failed dependencies:
        libc.so.6 is needed by python3-3.6.8-10.el7.i686
        libc.so.6(GLIBC_2.0) is needed by python3-3.6.8-10.el7.i686
        libc.so.6(GLIBC_2.3.4) is needed by python3-3.6.8-10.el7.i686
        libdl.so.2 is needed by python3-3.6.8-10.el7.i686
        libm.so.6 is needed by python3-3.6.8-10.el7.i686
        libpthread.so.0 is needed by python3-3.6.8-10.el7.i686
        libpython3.6m.so.1.0 is needed by python3-3.6.8-10.el7.i686
        libutil.so.1 is needed by python3-3.6.8-10.el7.i686
        python3-libs(x86-32) = 3.6.8-10.el7 is needed by python3-3.6.8-10.el7.i686
[chenchen@grpc01 tmp]$ 

3. 查看 python3-3.6.8-10.el7.i686.rpm 软件包的全部依赖关系.
	 可以看到依赖了很多, 第二步报错依赖失败中只是一部分, 另一部依赖是没问题的.
[chenchen@grpc01 tmp]$ sudo rpm -qpR python3-3.6.8-10.el7.i686.rpm
libc.so.6
libc.so.6(GLIBC_2.0)
libc.so.6(GLIBC_2.3.4)
libdl.so.2
libm.so.6
libpthread.so.0
libpython3.6m.so.1.0
libutil.so.1
python3-libs(x86-32) = 3.6.8-10.el7
python3-pip
python3-setuptools
rpmlib(CompressedFileNames) <= 3.0.4-1
rpmlib(FileDigests) <= 4.6.0-1
rpmlib(PartialHardlinkSets) <= 4.0.4-1
rpmlib(PayloadFilesHavePrefix) <= 4.0-1
rtld(GNU_HASH)
rpmlib(PayloadIsXz) <= 5.2-1

[chenchen@grpc01 tmp]$ sudo rpm -Uvh python3-3.6.8-18.el7.x86_64
error: open of python3-3.6.8-18.el7.x86_64 failed: No such file or directory

4. 第二步中的依赖失败, 逐一解决.
	 搜索找到, libc.so.6，该库对应的软件包名称为 glibc
	 
	 # 查找该软件包
	 yum list glibc*     // *.rpm 包列表; 列出资源库中所有可以安装或更新的 rpm 包

# 列出资源库中所有可以安装或更新的 rpm 包, glibc*
[chenchen@grpc01 tmp]$ sudo yum list glibc*
Loaded plugins: fastestmirror
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
Determining fastest mirrors
 * base: mirrors.huaweicloud.com
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.huaweicloud.com
Installed Packages
glibc.x86_64													2.17-324.el7_9										@updates
glibc-common.x86_64										2.17-324.el7_9										@updates
glibc-devel.x86_64										2.17-324.el7_9										@updates
glibc-headers.x86_64									2.17-324.el7_9										@updates
Available Packages
glibc.i686														2.17-325.el7_9										updates 
glibc.x86_64													2.17-325.el7_9										updates 
glibc-common.x86_64										2.17-325.el7_9										updates 
glibc-devel.i686											2.17-325.el7_9										updates 
glibc-devel.x86_64										2.17-325.el7_9										updates 
glibc-headers.x86_64									2.17-325.el7_9										updates 
glibc-static.i686											2.17-325.el7_9										updates 
glibc-static.x86_64										2.17-325.el7_9										updates 
glibc-utils.x86_64										2.17-325.el7_9										updates 
[chenchen@grpc01 tmp]$ 

# 用 yum 安装 glibc.i686
[chenchen@grpc01 tmp]$ sudo yum install glibc.i686
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.huaweicloud.com
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.huaweicloud.com
base															| 3.6 kB  00:00:00
docker-ce-stable															| 3.5 kB  00:00:00     
extras															| 2.9 kB  00:00:00     
updates															| 2.9 kB  00:00:00     
(1/3): docker-ce-stable/7/x86_64/primary_db															|  80 kB  00:00:01     
(2/3): extras/7/x86_64/primary_db															| 247 kB  00:00:07     
(3/3): updates/7/x86_64/primary_db															|  16 MB  00:16:47     
Resolving Dependencies
--> Running transaction check
---> Package glibc.i686 0:2.17-326.el7_9 will be installed
--> Processing Dependency: glibc-common = 2.17-326.el7_9 for package: glibc-2.17-326.el7_9.i686
--> Processing Dependency: libfreebl3.so(NSSRAWHASH_3.12.3) for package: glibc-2.17-326.el7_9.i686
--> Processing Dependency: libfreebl3.so for package: glibc-2.17-326.el7_9.i686
--> Running transaction check
---> Package glibc-common.x86_64 0:2.17-324.el7_9 will be updated
--> Processing Dependency: glibc-common = 2.17-324.el7_9 for package: glibc-2.17-324.el7_9.x86_64
---> Package glibc-common.x86_64 0:2.17-326.el7_9 will be an update
---> Package nss-softokn-freebl.x86_64 0:3.44.0-5.el7 will be updated
---> Package nss-softokn-freebl.i686 0:3.67.0-3.el7_9 will be installed
--> Processing Dependency: nss-util >= 3.67.0-1 for package: nss-softokn-freebl-3.67.0-3.el7_9.i686
--> Processing Dependency: nspr >= 4.30.0 for package: nss-softokn-freebl-3.67.0-3.el7_9.i686
---> Package nss-softokn-freebl.x86_64 0:3.67.0-3.el7_9 will be an update
--> Running transaction check
---> Package glibc.x86_64 0:2.17-324.el7_9 will be updated
--> Processing Dependency: glibc = 2.17-324.el7_9 for package: glibc-devel-2.17-324.el7_9.x86_64
--> Processing Dependency: glibc = 2.17-324.el7_9 for package: glibc-headers-2.17-324.el7_9.x86_64
---> Package glibc.x86_64 0:2.17-326.el7_9 will be an update
---> Package nspr.x86_64 0:4.21.0-1.el7 will be updated
---> Package nspr.x86_64 0:4.32.0-1.el7_9 will be an update
---> Package nss-util.x86_64 0:3.44.0-3.el7 will be updated
---> Package nss-util.x86_64 0:3.67.0-1.el7_9 will be an update
--> Running transaction check
---> Package glibc-devel.x86_64 0:2.17-324.el7_9 will be updated
---> Package glibc-devel.x86_64 0:2.17-326.el7_9 will be an update
---> Package glibc-headers.x86_64 0:2.17-324.el7_9 will be updated
---> Package glibc-headers.x86_64 0:2.17-326.el7_9 will be an update
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================================================
 Package                                                             Arch                                                    Version                                                          Repository                                                Size
=============================================================================================================
Installing:
 glibc									i686							2.17-326.el7_9					updates					4.3 M
Installing for dependencies:
 nss-softokn-freebl			i686							3.67.0-3.el7_9					updates					325 k
Updating for dependencies:
 glibc					x86_64					2.17-326.el7_9					updates					3.6 M
 glibc-common					x86_64					2.17-326.el7_9					updates					12 M
 glibc-devel					x86_64					2.17-326.el7_9					updates					1.1 M
 glibc-headers					x86_64					2.17-326.el7_9					updates					691 k
 nspr					x86_64					4.32.0-1.el7_9					updates					127 k
 nss-softokn-freebl					x86_64					3.67.0-3.el7_9					updates					337 k
 nss-util					x86_64					3.67.0-1.el7_9					updates					79 k

Transaction Summary
=============================================================================================================
Install  1 Package  (+1 Dependent package)
Upgrade             ( 7 Dependent packages)

Total download size: 22 M
Is this ok [y/d/N]: 
Is this ok [y/d/N]: y
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/9): glibc-common-2.17-326.el7_9.x86_64.rpm					|  12 MB  00:00:06     
(2/9): nspr-4.32.0-1.el7_9.x86_64.rpm					| 127 kB  00:00:00     
(3/9): nss-softokn-freebl-3.67.0-3.el7_9.i686.rpm					| 325 kB  00:00:00     
(4/9): nss-softokn-freebl-3.67.0-3.el7_9.x86_64.rpm					| 337 kB  00:00:00     
(5/9): nss-util-3.67.0-1.el7_9.x86_64.rpm					|  79 kB  00:00:00     
(6/9): glibc-headers-2.17-326.el7_9.x86_64.rpm					| 691 kB  00:00:06     
(7/9): glibc-devel-2.17-326.el7_9.x86_64.rpm					| 1.1 MB  00:00:08     
(8/9): glibc-2.17-326.el7_9.x86_64.rpm					| 3.6 MB  00:00:10     
(9/9): glibc-2.17-326.el7_9.i686.rpm					| 4.3 MB  00:00:30     
-------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                                        730 kB/s |  22 MB  00:00:30     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Warning: RPMDB altered outside of yum.
** Found 5 pre-existing rpmdb problem(s), 'yum check' output follows:
python3-libs-3.6.8-18.el7.x86_64 has missing requires of python(abi) = ('0', '3.6', None)
python3-pip-9.0.3-8.el7.noarch has missing requires of /usr/bin/python3
python3-pip-9.0.3-8.el7.noarch has missing requires of python(abi) = ('0', '3.6', None)
python3-setuptools-39.2.0-10.el7.noarch has missing requires of /usr/bin/python3
python3-setuptools-39.2.0-10.el7.noarch has missing requires of python(abi) = ('0', '3.6', None)
  Updating   : nss-softokn-freebl-3.67.0-3.el7_9.x86_64					1/16 
  Updating   : glibc-common-2.17-326.el7_9.x86_64					2/16 
  Updating   : glibc-2.17-326.el7_9.x86_64					3/16 
  Updating   : nspr-4.32.0-1.el7_9.x86_64					4/16 
  Updating   : nss-util-3.67.0-1.el7_9.x86_64					5/16 
  Updating   : glibc-headers-2.17-326.el7_9.x86_64					6/16 
  Installing : nss-softokn-freebl-3.67.0-3.el7_9.i686					7/16 
  Installing : glibc-2.17-326.el7_9.i686					8/16 
  Updating   : glibc-devel-2.17-326.el7_9.x86_64					9/16 
  Cleanup    : glibc-devel-2.17-324.el7_9.x86_64					10/16 
  Cleanup    : glibc-headers-2.17-324.el7_9.x86_64					11/16 
  Cleanup    : nspr-4.21.0-1.el7.x86_64					12/16 
  Cleanup    : nss-util-3.44.0-3.el7.x86_64					13/16 
  Cleanup    : nss-softokn-freebl-3.44.0-5.el7.x86_64					14/16 
  Cleanup    : glibc-common-2.17-324.el7_9.x86_64					15/16 
  Cleanup    : glibc-2.17-324.el7_9.x86_64					16/16 
  Verifying  : glibc-common-2.17-326.el7_9.x86_64					1/16 
  Verifying  : nss-softokn-freebl-3.67.0-3.el7_9.x86_64					2/16 
  Verifying  : glibc-2.17-326.el7_9.x86_64					3/16 
  Verifying  : glibc-devel-2.17-326.el7_9.x86_64					4/16 
  Verifying  : nss-util-3.67.0-1.el7_9.x86_64					5/16 
  Verifying  : nspr-4.32.0-1.el7_9.x86_64					6/16 
  Verifying  : glibc-headers-2.17-326.el7_9.x86_64					7/16 
  Verifying  : glibc-2.17-326.el7_9.i686					8/16 
  Verifying  : nss-softokn-freebl-3.67.0-3.el7_9.i686					9/16 
  Verifying  : glibc-devel-2.17-324.el7_9.x86_64					10/16 
  Verifying  : glibc-2.17-324.el7_9.x86_64					11/16 
  Verifying  : glibc-common-2.17-324.el7_9.x86_64					12/16 
  Verifying  : nspr-4.21.0-1.el7.x86_64					13/16 
  Verifying  : glibc-headers-2.17-324.el7_9.x86_64					14/16 
  Verifying  : nss-softokn-freebl-3.44.0-5.el7.x86_64					15/16 
  Verifying  : nss-util-3.44.0-3.el7.x86_64					16/16 

Installed:
  glibc.i686 0:2.17-326.el7_9                                                                                                                                                                                                                                

Dependency Installed:
  nss-softokn-freebl.i686 0:3.67.0-3.el7_9                                                                                                                                                                                                                   

Dependency Updated:
  glibc.x86_64 0:2.17-326.el7_9 glibc-common.x86_64 0:2.17-326.el7_9 glibc-devel.x86_64 0:2.17-326.el7_9 glibc-headers.x86_64 0:2.17-326.el7_9 nspr.x86_64 0:4.32.0-1.el7_9 nss-softokn-freebl.x86_64 0:3.67.0-3.el7_9 nss-util.x86_64 0:3.67.0-1.el7_9

Complete!
[chenchen@grpc01 tmp]$ 

# 重新再次安装 python3-3.6.8-10.el7.i686.rpm, 发现还有依赖, 这两个依赖搜一通, 没有搜到, 还不行, 遂放弃.
[chenchen@grpc01 tmp]$ sudo rpm -ivh python3-3.6.8-10.el7.i686.rpm
error: Failed dependencies:
        libpython3.6m.so.1.0 is needed by python3-3.6.8-10.el7.i686
        python3-libs(x86-32) = 3.6.8-10.el7 is needed by python3-3.6.8-10.el7.i686
[chenchen@grpc01 tmp]$ 

# 继续重新安装 yum 默认安装的 pyton3, 提示此 python3.x86_64 0:3.6.8-18.el7 已经安装, 其实是被手动删除过的, 不可用的.
[chenchen@grpc01 tmp]$ sudo yum install python3
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.huaweicloud.com
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.huaweicloud.com
Resolving Dependencies
--> Running transaction check
---> Package python3.x86_64 0:3.6.8-18.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================================================
 Package									Arch									Version									Repository									Size
=============================================================================================================
Installing:
 python3									x86_64									3.6.8-18.el7									updates									70 k

Transaction Summary
=============================================================================================================
Install  1 Package

Total download size: 70 k
Installed size: 39 k
Is this ok [y/d/N]: y
Downloading packages:
python3-3.6.8-18.el7.x86_64.rpm                                                                                                                                                                                                       |  70 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : python3-3.6.8-18.el7.x86_64                                                                                                                                                                                                               1/1 
  Verifying  : python3-3.6.8-18.el7.x86_64                                                                                                                                                                                                               1/1 

Installed:
  python3.x86_64 0:3.6.8-18.el7                                                                                                                                                                                                                              

Complete!

# 试着通过 yum 删除 python3.x86_64 0:3.6.8-18.el7, 然后再重新全新安装. 结果虽然损坏了, 但删除软件包成功了
[chenchen@grpc01 tmp]$ sudo yum remove python3.x86_64 0:3.6.8-18.el7
Loaded plugins: fastestmirror
No Match for argument: 0:3.6.8-18.el7
Resolving Dependencies
--> Running transaction check
...
...
...
...
Removed:
  python3.x86_64 0:3.6.8-18.el7                                                                                                                                                                                                                              

Dependency Removed:
  python3-libs.x86_64 0:3.6.8-18.el7                                                python3-pip.noarch 0:9.0.3-8.el7                                                python3-setuptools.noarch 0:39.2.0-10.el7                                               

Complete!

# 重新通过 yum 安装 python3.x86_64 0:3.6.8-18.el7, 也成功了
[chenchen@grpc01 tmp]$ sudo yum install python3.x86_64 0:3.6.8-18.el7
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.huaweicloud.com
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.huaweicloud.com
No package 0:3.6.8-18.el7 available.
Resolving Dependencies
--> Running transaction check
---> Package python3.x86_64 0:3.6.8-18.el7 will be installed
--> Processing Dependency: python3-libs(x86-64) = 3.6.8-18.el7 for package: python3-3.6.8-18.el7.x86_64
--> Processing Dependency: python3-setuptools for package: python3-3.6.8-18.el7.x86_64
--> Processing Dependency: python3-pip for package: python3-3.6.8-18.el7.x86_64
--> Processing Dependency: libpython3.6m.so.1.0()(64bit) for package: python3-3.6.8-18.el7.x86_64
--> Running transaction check
---> Package python3-libs.x86_64 0:3.6.8-18.el7 will be installed
---> Package python3-pip.noarch 0:9.0.3-8.el7 will be installed
---> Package python3-setuptools.noarch 0:39.2.0-10.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================================================
 Package									Arch									Version									Repository									Size
=============================================================================================================
Installing:
 python3									x86_64									3.6.8-18.el7									updates									70 k
Installing for dependencies:
 python3-libs						x86_64									3.6.8-18.el7									updates									6.9 M
 python3-pip						noarch						9.0.3-8.el7						base						1.6 M
 python3-setuptools						noarch						39.2.0-10.el7						base						629 k

Transaction Summary
=============================================================================================================
Install  1 Package (+3 Dependent packages)

Total download size: 9.3 M
Installed size: 47 M
Is this ok [y/d/N]: y
Downloading packages:
(1/4): python3-3.6.8-18.el7.x86_64.rpm                                                                                                                                                                                                |  70 kB  00:00:00     
(2/4): python3-pip-9.0.3-8.el7.noarch.rpm                                                                                                                                                                                             | 1.6 MB  00:00:01     
(3/4): python3-setuptools-39.2.0-10.el7.noarch.rpm                                                                                                                                                                                    | 629 kB  00:00:08     
(4/4): python3-libs-3.6.8-18.el7.x86_64.rpm                                                                                                                                                                                           | 6.9 MB  00:01:21     
-------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                                        116 kB/s | 9.3 MB  00:01:21     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : python3-libs-3.6.8-18.el7.x86_64                                                                                                                                                                                                          1/4 
  Installing : python3-3.6.8-18.el7.x86_64                                                                                                                                                                                                               2/4 
  Installing : python3-setuptools-39.2.0-10.el7.noarch                                                                                                                                                                                                   3/4 
  Installing : python3-pip-9.0.3-8.el7.noarch                                                                                                                                                                                                            4/4 
  Verifying  : python3-setuptools-39.2.0-10.el7.noarch                                                                                                                                                                                                   1/4 
  Verifying  : python3-libs-3.6.8-18.el7.x86_64                                                                                                                                                                                                          2/4 
  Verifying  : python3-3.6.8-18.el7.x86_64                                                                                                                                                                                                               3/4 
  Verifying  : python3-pip-9.0.3-8.el7.noarch                                                                                                                                                                                                            4/4 

Installed:
  python3.x86_64 0:3.6.8-18.el7                                                                                                                                                                                                                              

Dependency Installed:
  python3-libs.x86_64 0:3.6.8-18.el7                                                python3-pip.noarch 0:9.0.3-8.el7                                                python3-setuptools.noarch 0:39.2.0-10.el7                                               

Complete!
[chenchen@grpc01 tmp]$ python3 -V
Python 3.6.8

# 再次重新检查没有问题了, 
# sudo rpm -Va, 是校验所有的 RPM 软件包, 查找丢失的文件
[chenchen@grpc01 tmp]$ sudo rpm -Va
S.5....T.  c /etc/sudoers
.M.......  g /etc/pki/ca-trust/extracted/java/cacerts
.M.......  g /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
.M.......  g /etc/pki/ca-trust/extracted/pem/email-ca-bundle.pem
.M.......  g /etc/pki/ca-trust/extracted/pem/objsign-ca-bundle.pem
.M.......  g /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
S.5....T.  c /etc/sysctl.conf
.......T.    /usr/bin/yum
.M.......  g /etc/lvm/cache/.cache
....L....  c /etc/pam.d/fingerprint-auth
....L....  c /etc/pam.d/password-auth
....L....  c /etc/pam.d/postlogin
....L....  c /etc/pam.d/smartcard-auth
....L....  c /etc/pam.d/system-auth
S.5....T.  c /etc/security/limits.conf
S.5....T.  c /etc/security/limits.d/20-nproc.conf
.......T.  c /etc/selinux/targeted/contexts/customizable_types
[chenchen@grpc01 tmp]$ 

# 之前检查时有 missing 错误. 很多
# sudo rpm -Va, 是校验所有的 RPM 软件包, 查找丢失的文件, 可见丢失了很多文件都是 python 的, 这符合手动删除的结果
[chenchen@grpc01 ~]$ sudo rpm -Va
S.5....T.  c /etc/sudoers
missing     /usr/lib/python3.6
missing     /usr/lib/python3.6/site-packages
missing     /usr/lib/python3.6/site-packages/__pycache__
.M.......  g /etc/pki/ca-trust/extracted/java/cacerts
.M.......  g /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
.M.......  g /etc/pki/ca-trust/extracted/pem/email-ca-bundle.pem
.M.......  g /etc/pki/ca-trust/extracted/pem/objsign-ca-bundle.pem
.M.......  g /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
missing     /usr/lib/python3.6/site-packages/pip
missing     /usr/lib/python3.6/site-packages/pip-9.0.3.dist-info
missing     /usr/lib/python3.6/site-packages/pip-9.0.3.dist-info/INSTALLER
.......
.......

```

### Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist

```shell
问题:
[root@28cdf5453e1e ~]# yum search vim  
Failed to set locale, defaulting to C.UTF-8
CentOS Linux 8 - AppStream                                                                                                                                                                         42  B/s |  38  B     00:00    
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist

原因:
便是 CentOS 已经停止维护的问题。2020 年 12 月 8 号，CentOS 官方宣布了停止维护 CentOS Linux 的计划，并推出了 CentOS Stream 项目，CentOS Linux 8 作为 RHEL 8 的复刻版本，生命周期缩短，于 2021 年 12 月 31 日停止更新并停止维护（EOL），更多的信息可以查看 CentOS 官方公告。如果需要更新 CentOS，需要将镜像从 mirror.centos.org 更改为 vault.centos.org


解决:
[root@28cdf5453e1e ~]# cd /etc/yum.repos.d/
[root@28cdf5453e1e yum.repos.d]# sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
[root@28cdf5453e1e yum.repos.d]# sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
[root@28cdf5453e1e yum.repos.d]# yum makecache
Failed to set locale, defaulting to C.UTF-8
CentOS Linux 8 - AppStream                                                                                                                                                                         44 kB/s | 8.4 MB     03:17    
CentOS Linux 8 - BaseOS                                                                                                                                                                            86 kB/s | 4.6 MB     00:54    
CentOS Linux 8 - Extras                                                                                                                                                                           5.2 kB/s |  10 kB     00:02    
Last metadata expiration check: 0:00:01 ago on Fri Feb 10 12:26:22 2023.
Metadata cache created.
[root@28cdf5453e1e yum.repos.d]# yum update -y				# 更新东西很多, 时间较长
[root@28cdf5453e1e yum.repos.d]# yum -y install vim
以上均能完美完成.
参考: https://blog.csdn.net/weixin_43252521/article/details/124409151
```



### See Also

https://blog.csdn.net/SUNbrightness/article/details/80642925
