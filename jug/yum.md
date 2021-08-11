# yum

---



### 配置

```
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

```
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

```
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

```
.repo 文件, 字段说明: 
[nux-dextop]: 仓库名字
name: 描述性文字
baseurl: 仓库地址
enabled: 是否默认开启, 不写默认为开启
gpgcheck:  是否检查GPG(GNU Private Guard)，一种密钥方式签名
gpgkey:
protect: 更新相关
```

```
直接找一个 repo 文件
cd /etc/yum.repos.d/
wget http://mirrors.163.com/.help/CentOS-Base-163.repo
yum makecache  //生成缓存
yum update
```

```
通过安装特定插件来自动找最快的镜像
安装好yum-fastestmirror后,每次用yum安装就会自动检查速度最快的镜像了
yum install yum-fastestmirror
yum clean all
安装好yum-fastestmirror后,每次用yum安装就会自动检查速度最快的镜像了
```

```
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

```
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



### 安装

```

# yum -y install ffmpeg ffmpeg-devel --enablerepo=nux-dextop		// 通过制定仓库来安装 FFmpeg 和 FFmpeg-devel 包
[root@Linux modules]# yum install subversion --enablerepo=WandiscoSVN			// 安装 SVN
# yum install vim-enhanced.x86_64
# yum install xmms-mp3																					// 安装rpm包,如xmms-mp3
[chenchen@localhost ~]$ sudo yum update bind-utils.x86_64				// 安装 dig
```



### 更新

```
# yum check-update					// *.rpm 包的更新, 检查可更新的 rpm 包; 查看那些可以升级, 返回 第二列是升级后的版本, 最后一列是来自哪里仓库

# yum update								// 升级全部; update改变软件设置和系统设置, 系统版本内核都升级; update主要是使软件达到最新, 会频繁的发布升级;
# yum update bash.x86_64
# yum update kernel kernel-source		// 更新指定的 rpm 包, 如更新 kernel 和 kernel source

# yum upgrade								// 大规模的版本升级, 与 yum update 不同的是, 连旧的淘汰的包也升级; upgrade不改变软件设置和系统设置, 系统版本升级, 内核不改变; upgrade 更侧重的是软件功能得到一个很大的提升, 不会频繁的升级;
```



### 卸载

```
[root@Linux modules]# yum remove subversion
# yum remove licq			// 删除 rpm 包, 包括与该包有倚赖性的包; 注: 同时会提示删除 licq-gnome, licq-qt, licq-text
```



### 服务

```
// 每天定期执行系统更新
# chkconfig yum on
# service yum start
```



### 杂项

```
安装 rz sz
yum -y install lrzsz
```

