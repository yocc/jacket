# rpm

---

[toc]

### 介绍

```
rpm 是 redhat 公司的一种软件包管理机制, 直接通过 rpm 命令进行安装删除等操作, 最大的优点是自己内部自动处理了各种软件包可能的依赖关系.
*.rpm 形式的二进制软件包
```



### 查询

##### 查询已经安装的软件包

```
# rpm -qi 已安装的包名			// 查看一个包的详细信息, 服务安装后查看已经安装包的名字
# rpm -qi 未安装的包名			// 后面如果跟一个未安装的包名，会显示什么信息？// 包名 is not installed
# rpm -qc 包名						 // 显示软件包中的配置文件, 这些文件一般是安装后需要用户手工修改的, 例如：sendmail.cf,passwd,inittab等
# rpm -qpi 包名						 // 如果用户得到一个新的RPM文件, 却不清楚它的内容; 或想了解某个文件包将会在系统里安装那些文件

# rpm -qa					 // 查询所有已经安装的包
// 服务安装后查看已经安装包的名字
[root@dev_10.211.21.18 redis_6379]# rpm -qa | grep redis			// 查询某个已经安装的包
sinasrv2-python27-redis-2.9.1-1.x86_64
sinasrv2-lua-redis-2.0.4-1.x86_64
rc-redis-3.0.7-1.x86_64
sinasrv2-php-redis-2.2.7-1.x86_64
```



##### 查询软件包安装位置

```
[root@dev_10.211.21.18 redis_6379]# rpm -ql rc-redis-3.0.7-1.x86_64			// 用查到的安装包的名字全称查找安装目录, 查看一个包安装了哪些文件
/usr/local/rc-redis-3.0.7/.gitignore
/usr/local/rc-redis-3.0.7/00-RELEASENOTES
# rpm -qf 文件名			// 查看一个文件是由哪个包安装的
```



##### 通过进程名字获取软件包名称

```
如果用户碰到一个认不出来的文件, 想要知道它是属于那一个软件包:
[chenchen@hathor122 sbin]$ rpm -qf /usr/sbin/sshd
openssh-server-6.6.1p1-11.el7.x86_64

[chenchen@hathor122 sbin]$ rpm -qf /usr/local/sbin/sshd
sinaopenssh-6.7p1-1.x86_64
```



### 校验软件包

```
rpm -Vf 需要验证到包
```



### 安装软件包

```
rpm -i 包名 
rpm -i example.rpm			  // 安装 example.rpm 包；
rpm -iv example.rpm			  // 安装 example.rpm 包并在安装过程中显示正在安装的文件信息；
rpm -ivh example.rpm			// 安装 example.rpm 包并在安装过程中显示正在安装的文件信息及安装进度
rpm --install 包名
rpm -ivh *.rpm
rpm -ivh 包名
rpm -i --nodeps 包名			 // 有依赖关系时, 忽略依赖关系, 强制安装该包
```



### 升级软件包

```
rpm -Uvh 包名

用户要注意的是：rpm会自动卸载相应软件包的老版本。如果老版本软件的配置文件通新版本的不兼容，rpm会自动将其保存为另外一个文件，用户会看到下面的信息：
saving /etc/example.conf as /etc/example.conf.rpmsave
这样用户就可以自己手工去更改相应的配置文件。

另外如果用户要安装老版本的软件，用户就会看到下面的出错信息：
# rpm -Uvh example.rpm
examle packag example-2.0-l(which is newer) is already installed
error:example.rpm cannot be installed
 
如果用户要强行安装就使用-oldpackage参数。
```



### 卸载已经安装的软件包

```
卸载所有匹配名字的包:
一般在发现安装有包冲突版本号不对时, 
就会先卸载已有的包, 重新让yum自动安装依赖包.
rpm -e --allmatchs e2fsprogs			// 卸载匹配
rpm -e goaccess-0.6-1.el6.x86_64        // 卸载指定准确包名的软件
rpm -e 包名 
rpm --erase 包名


```



### 查看系统未安装软件相关命令

```shell
首先要这个未安装rpm包存在，我们才能查看相关信息

1. 查看软件包的详细信息
#rpm -qpi rpm包
2. 查看软件包所包含的目录和文件
#rpm -qpl rpm包
3. 查看软件包的文档所在的位置
#rpm -qpd rpm包
4. 查看软件包的配置文件（若没有，则标准输出就为空）
#rpm -qpc rpm包
5. 查看软件包的依赖关系
#rpm -qpR rpm包
```



### FAQ

```shell
问题:
[chenchen@grpc01 bin]$ sudo rpm -e python-2.7.5-86.el7.x86_64
error: Failed dependencies:
......

解决:
# --force (强制) 和 --nodeps (不查找依赖关系)
sudo rpm -e python-2.7.5-86.el7.x86_64 --nodeps
```

```shell
问题:
删除了 python2.7 导致 yum不可用

原因:
删除了系统中不该删出的软件, 比如删除了 python2.7, 而 python2.7 是 yum 脚本使用的.
即, vim /usr/bin/yum 第一行是 #!/usr/bin/python, 也就是 yum 是 python 脚本

解决:
思路是需要重新装回 python2.7
1. 需要先找到之前通过 sudo rpm -e python-2.7.5-86.el7.x86_64 --nodeps 卸载掉的软件包
通过官网 https://vault.centos.org/7.7.1908/os/x86_64/Packages/ 来搜索严格对应的软件包
其中的 CentOS 系统版本号, 由一下方法拿到.
[chenchen@grpc01 ~]$ cat /etc/redhat-release
CentOS Linux release 7.7.1908 (Core)
2. 下载找到的软件包
wget https://vault.centos.org/7.7.1908/os/x86_64/Packages/python-2.7.5-86.el7.x86_64.rpm
3. 通过 rpm 安装本地 rpm包
sudo rpm -ivh python-2.7.5-86.el7.x86_64.rpm
4. 检验 python 是否正常
[chenchen@grpc01 ~]$ python --version
Python 2.7.5
[chenchen@grpc01 ~]$ yum list installed | grep  python
audit-libs-python.x86_64               2.8.5-4.el7                     @base    
dbus-python.x86_64                     1.1.1-9.el7                     @anaconda
libselinux-python.x86_64               2.5-15.el7                      @base    
libsemanage-python.x86_64              2.5-14.el7                      @base    
libxml2-python.x86_64                  2.9.1-6.el7_9.6                 @updates 
newt-python.x86_64                     0.52.15-4.el7                   @anaconda
policycoreutils-python.x86_64          2.5-34.el7                      @base    
python.x86_64                          2.7.5-86.el7                    @anaconda
python-IPy.noarch                      0.75-6.el7                      @base    
python-chardet.noarch                  2.2.1-3.el7                     @base    
python-configobj.noarch                4.7.2-7.el7                     @anaconda
python-decorator.noarch                3.4.0-3.el7                     @anaconda
python-firewall.noarch                 0.6.3-2.el7                     @anaconda
python-gobject-base.x86_64             3.22.0-1.el7_4.1                @anaconda
python-iniparse.noarch                 0.4-9.el7                       @anaconda
python-kitchen.noarch                  1.1.1-5.el7                     @base    
python-libs.x86_64                     2.7.5-86.el7                    @anaconda
python-linux-procfs.noarch             0.4.11-4.el7                    @anaconda
python-perf.x86_64                     3.10.0-1062.el7                 @anaconda
python-pycurl.x86_64                   7.19.0-19.el7                   @anaconda
python-pyudev.noarch                   0.15-9.el7                      @anaconda
python-rpm-macros.noarch               3-34.el7                        @base    
python-schedutils.x86_64               0.4-6.el7                       @anaconda
python-slip.noarch                     0.4.0-4.el7                     @anaconda
python-slip-dbus.noarch                0.4.0-4.el7                     @anaconda
python-srpm-macros.noarch              3-34.el7                        @base    
python-urlgrabber.noarch               3.10-9.el7                      @anaconda
python3.x86_64                         3.6.8-18.el7                    @updates 
python3-libs.x86_64                    3.6.8-18.el7                    @updates 
python3-pip.noarch                     9.0.3-8.el7                     @base    
python3-setuptools.noarch              39.2.0-10.el7                   @base    
rpm-python.x86_64                      4.11.3-48.el7_9                 @updates 
[chenchen@grpc01 ~]$ 
```

