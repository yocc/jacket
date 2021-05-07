# rpm



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


