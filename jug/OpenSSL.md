# OpenSSL

---

[toc]



## Overview

在安装 Python3.10.2 时遇到需要让系统 openssl 版本在 1.1.2 之上, 此时系统自带 openssl 时 1.0.2.k, 即便 yum update 也只能最高升级到1.0.2.k, 此时只能手动源码安装 OpenSSL 了.

```shell
1. 官网
https://www.openssl.org/

2. 下载
wget https://www.openssl.org/source/openssl-1.1.1m.tar.gz --no-check-certificate

3. 解压
tar zxvf openssl-1.1.1m.tar.gz -C ./
cd openssl-1.1.1m
less INSTALL
less README

4. ./config
./config --prefix=/usr/local --libdir=/usr/local --openssldir=/usr/local/ssl
$ ./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl '-Wl,-rpath,$(LIBRPATH)' (不用)
$ ./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl '-Wl,--enable-new-dtags,-rpath,$(LIBRPATH)' (不用)
$ ./config --prefix=/usr/local/openssl111m --openssldir=/usr/local/ssl111m --libdir=lib (成功)

--prefix=/usr/local, 安装本体目录, 但我认为应该是 /usr/local/openssl111m
--openssldir=/usr/local/ssl111m, 放置配置文件, 证书, 密钥
--libdir=lib, 本体目录下的目录名字, 不是本体根目录整体, 即 /usr/local/openssl111m/lib, 放置库文件

--prefix=DIR, 物理安装目录的分割位置, 默认, /usr/local, 表示安装在/usr/local目录下的子目录树中, 比如实际二进制在/usr/local/bin/openssl

--libdir=DIR, libraries 库安装的位置, 默认为 /usr/local, 默认在 --prefix 目录树下的lib子目录(32位系统), 64位系统为lib64, 即/usr/local/lib64/libssl.so.1.1, /usr/local/lib64/libcrypto.so.1.1
[chenchen@grpc01 lib64]$ l
total 11M
  4754331 -rwxr-xr-x.  1 root root  3.3M Mar  8 18:09 libcrypto.so.1.1
  5378771 -rwxr-xr-x.  1 root root  674K Mar  8 18:09 libssl.so.1.1
  5941420 -rw-r--r--.  1 root root  5.4M Mar  8 18:09 libcrypto.a
  5941421 -rw-r--r--.  1 root root 1003K Mar  8 18:09 libssl.a
  5941419 lrwxrwxrwx.  1 root root    16 Mar  8 18:09 libcrypto.so -> libcrypto.so.1.1
  5941422 lrwxrwxrwx.  1 root root    13 Mar  8 18:09 libssl.so -> libssl.so.1.1
415237526 drwxr-xr-x.  2 root root    61 Mar  8 18:09 pkgconfig
  4194907 drwxr-xr-x.  4 root root   159 Mar  8 18:09 .
419432762 drwxr-xr-x.  2 root root    39 Mar  8 18:09 engines-1.1
 12583335 drwxr-xr-x. 19 root root  4.0K Mar  8 18:09 ..
[chenchen@grpc01 lib64]$ pwd
/usr/local/lib64

--openssldir=DIR, OpenSSL 配置文件的目录, 以及默认证书和密钥存储. 默认值目录为: /usr/local/ssl

Every Unix system has its own set of default locations for shared libraries, such as /lib, /usr/lib or possibly /usr/local/lib.  
共享库: 
/lib, 			/usr/lib, 			/usr/local/lib
/lib64, 		/usr/lib64, 	  /usr/local/lib64
/无				 /libexec, 			 /usr/local/libexec
/无				 /share, 				 /usr/local/share
/无				 /include, 			 /usr/local/include
/bin, 			/usr/bin, 	  	/usr/local/bin
/sbin, 			/usr/sbin, 	  	/usr/local/sbin
/etc, 			/usr/etc, 	  	/usr/local/etc

5. make

6. make test

7. make install

8. openssl
[chenchen@grpc01 ld.so.conf.d]$ openssl version
OpenSSL 1.1.1m  14 Dec 2021
```



## 本体目录说明

```shell
本体包含的所有组件都安装在 --prefix 参数下或者不加参数而走软件默认位置. 我始终用显式的参数来指定本体安装的位置.
bin/	目录, 含有二进制openssl和其他功能脚本
include/openssl		包含头文件, 这些头文件是为用户提供编译其他软件时可能需要的, 比如, libcrypto 或者 libssl
lib		包含 OpenSSL 库文件
lib/engines		包含 OpenSSL 动态可加载的引擎
share/man/man1		包含 OpenSSL 命令行 man 页
share/man/man3		包含 OpenSSL 库调用 man 页
share/man/man5		包含 OpenSSL 配置格式 man 页
share/man/man7		包含 OpenSSL 其他杂项 man 页
share/doc/openssl/html/man1		包含 HTML man 页
share/doc/openssl/html/man3
share/doc/openssl/html/man5
share/doc/openssl/html/man7

--openssldir 参数目录下的结构说明
certs		刚安装完时是空目录, 放置证书文件
private		刚安装完时是空目录, 放置私钥文件
misc		刚安装完时是空目录, 放置各种脚本

预装了 OpenSSL 已经作为操作系统的一部分, 建议不要覆盖系统版本, 而是新安装到其他地方.

```



## Custom OpenSSL

```shell
# 自定义安装 OpenSSL, 参考: https://docs.python.org/3/using/unix.html
1. 为了使用供应商的 OpenSSL 配置和操作系统信任的存储位置, 请在 /etc 中找到包含 openssl.cnf 文件或 符号链接 的目录. 在大多数分发中, 文件要么在 /etc/ssl 中, 要么在 /etc/pki/tls 中. 该目录还应包含 cert.pem 文件和/或证书目录。

$ find /etc/ -name openssl.cnf -printf "%h\n"
/etc/ssl

[chenchen@grpc01 certs]$ sudo find /etc/ -name openssl.cnf -printf "%h\n"
/etc/pki/tls

2. 下载、构建和安装 OpenSSL. 确保使用 install_sw 而不是 install. install_sw 目标不会覆盖 openssl.cnf.

$ curl -O https://www.openssl.org/source/openssl-VERSION.tar.gz
   $ tar xzf openssl-VERSION
   $ pushd openssl-VERSION
   $ ./config \
        --prefix=/usr/local/custom-openssl \
        --libdir=lib \
        --openssldir=/etc/ssl
   $ make -j1 depend
   $ make -j8
   $ make install_sw
   $ popd

# make -j 带一个参数, 可以把项目在进行并行编译, 比如在一台双核的机器上, 完全可以用 make -j4, 让 make 最多允许4个编译命令同时执行, 这样可以更有效的利用 CPU 资源. 但并行的任务不宜太多, 一般是以CPU的核心数目的两倍为宜.
# --libdir 是在 --prefix 下面的目录名字(推荐 lib), --openssldir 放置配置文件, 证书, 密钥


wget https://www.openssl.org/source/openssl-1.1.1m.tar.gz --no-check-certificate
tar zxvf openssl-1.1.1m.tar.gz -C ./
cd openssl-1.1.1m
less INSTALL
less README
./config --prefix=/usr/local/openssl-1.1.1m --libdir=/usr/local/openssl-1.1.1m --openssldir=/etc/pki/tls
make -j1 depend
make -j8
make install_sw


3. 使用自定义 OpenSSL 构建 Python
$ pushd python-3.x.x
$ ./configure --prefix=$HOME/tmp/python37 --enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/usr/local/openssl111m (成功, --with-openssl=OpenSSL本体根目录)
$ make -j8
$ make altinstall

4. 注意 OpenSSL 的修补程序版本具有向后兼容的 ABI. 你不需要重新编译 Python 来更新 OpenSSL. 将自定义 OpenSSL 安装替换为较新版本就足够了.
```



## FAQ

```shell
问题:
ldconfig: Can't create temporary cache file /etc/ld.so.cache~: Permission denied
[chenchen@grpc01 ld.so.conf.d]$ openssl
openssl: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory

原因:
原系统已经有了 1.0 版本, 二进制位置在 /usr/bin/openssl, library 位置在 /usr/lib64/
新安装 1.1 版本, 二进制位置在 /usr/local/bin/openssl, library 位置在 /usr/local/lib64/
在 env 里 1.1 版本的二进制比 1.0 版本的二进制更靠前, 优先被执行, 而执行时 1.1 版本找到的 library 是 1.0 版本的或者根本找不到 1.1 版本的库, 所以提示了上面的错误.

[chenchen@grpc01 bin]$ l | grep openssl
 12802540 -rwxr-xr-x.   1 root root     543K Jan 18 21:56 openssl
[chenchen@grpc01 bin]$ pwd
/usr/bin

[chenchen@grpc01 lib64]$ l | grep ssl
   218884 -rwxr-xr-x.  1 root root 362K Aug 12  2019 libssl3.so
   192187 drwxr-xr-x.  3 root root   21 Jan 18 21:56 openssl
    31642 -rwxr-xr-x.  1 root root 460K Jan 18 21:56 libssl.so.1.0.2k
    31638 -rw-r--r--.  1 root root   65 Jan 18 21:56 .libssl.so.1.0.2k.hmac
   492477 -rw-r--r--.  1 root root 745K Jan 18 21:56 libssl.a
    31639 lrwxrwxrwx.  1 root root   22 Mar  5 13:22 .libssl.so.10.hmac -> .libssl.so.1.0.2k.hmac
   595493 lrwxrwxrwx.  1 root root   16 Mar  5 13:22 libssl.so.10 -> libssl.so.1.0.2k
    31749 lrwxrwxrwx.  1 root root   16 Mar  5 13:22 libssl.so -> libssl.so.1.0.2k
[chenchen@grpc01 lib64]$ pwd
/usr/lib64

[chenchen@grpc01 bin]$ l
total 744K
  627567 -rwxr-xr-x.  1 root root 732K Mar  8 18:09 openssl
  627568 -rwxr-xr-x.  1 root root 6.1K Mar  8 18:09 c_rehash
     122 drwxr-xr-x.  2 root root   37 Mar  8 18:09 .
12583335 drwxr-xr-x. 19 root root 4.0K Mar  8 18:09 ..
[chenchen@grpc01 bin]$ pwd
/usr/local/bin

[chenchen@grpc01 lib64]$ l
total 11M
  4754331 -rwxr-xr-x.  1 root root  3.3M Mar  8 18:09 libcrypto.so.1.1
  5378771 -rwxr-xr-x.  1 root root  674K Mar  8 18:09 libssl.so.1.1
  5941420 -rw-r--r--.  1 root root  5.4M Mar  8 18:09 libcrypto.a
  5941421 -rw-r--r--.  1 root root 1003K Mar  8 18:09 libssl.a
  5941419 lrwxrwxrwx.  1 root root    16 Mar  8 18:09 libcrypto.so -> libcrypto.so.1.1
  5941422 lrwxrwxrwx.  1 root root    13 Mar  8 18:09 libssl.so -> libssl.so.1.1
415237526 drwxr-xr-x.  2 root root    61 Mar  8 18:09 pkgconfig
  4194907 drwxr-xr-x.  4 root root   159 Mar  8 18:09 .
419432762 drwxr-xr-x.  2 root root    39 Mar  8 18:09 engines-1.1
 12583335 drwxr-xr-x. 19 root root  4.0K Mar  8 18:09 ..
[chenchen@grpc01 lib64]$ pwd
/usr/local/lib64

解决:
同 Python3.md 中的 FAQ 的解决办法1.

方法1: (采用的, 行之有效的)
cd  /etc/ld.so.conf.d
-> sudo vim libssl.so.1.1.conf
-> 编辑 添加1.1.1库文件路径 /usr/local/openssl111m/lib, 重点, 这里写入 OpenSSL 库的目录路径, 而不是 OpenSSL 的安装的根部目录, 而是根目录下的lib(lib是由)
-> 退出保存
-> 运行 sudo ldconfig -vvv, 刷新缓存

这里更加详细的给以说明:
/etc/ 下有一个 ld.so.conf.d 目录 和 一个 ld.so.conf 文件, 其中 ld.so.conf 文件中写有:
include ld.so.conf.d/*.conf
表明, 这里并不做配置, 而是将配置放到 include 指定的目录中的 .conf 文件里,
.conf 文件名随便起, 惯例是 .so 文件的全名, 即, libssl.so.1.1.conf
新建 libssl.so.1.1.conf 文件, 在其中写有:
/usr/local/lib64
文件路径, 保存退出,
需要 sudo 权限执行 ldconfig -vvv 刷新 .so 文件位置对照表, -vvv参数是 verbose 详情的意思.
经过以上解决, 可以试验得到, 不管在 1.0 还是 1.1 均可正常使用了.

[chenchen@grpc01 ld.so.conf.d]$ openssl version							# 由于 env 配置靠前, 优先被执行
OpenSSL 1.1.1m  14 Dec 2021
[chenchen@grpc01 ld.so.conf.d]$ /usr/bin/openssl version		# 需要指定位置绝对路径
OpenSSL 1.0.2k-fips  26 Jan 2017
```





## See Also

[OpenSSL]:(https://www.openssl.org/)

https://docs.python.org/3/using/unix.html