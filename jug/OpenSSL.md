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

```sh

[chenchen@dev3_10.211.21.18 ld.so.conf.d]$ openssl version -a
OpenSSL 1.0.2k-fips  26 Jan 2017
built on: reproducible build, date unspecified
platform: linux-x86_64
options:  bn(64,64) md2(int) rc4(16x,int) des(idx,cisc,16,int) idea(int) blowfish(idx) 
compiler: gcc -I. -I.. -I../include  -fPIC -DOPENSSL_PIC -DZLIB -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DKRB5_MIT -m64 -DL_ENDIAN -Wall -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches   -m64 -mtune=generic -Wa,--noexecstack -DPURIFY -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_MONT5 -DOPENSSL_BN_ASM_GF2m -DRC4_ASM -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DAES_ASM -DVPAES_ASM -DBSAES_ASM -DWHIRLPOOL_ASM -DGHASH_ASM -DECP_NISTZ256_ASM
OPENSSLDIR: "/etc/pki/tls"
engines:  dynamic 
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

## openss-l.1.1.1u

```sh
[chenchen@dev3_10.211.21.18 openssl111u]$ pwd
/usr/home/chenchen/htdocs/openssl111u
[chenchen@dev3_10.211.21.18 clash]$ sudo ./clash -d ./
[chenchen@dev3_10.211.21.18 openssl111u]$ wget https://github.com/openssl/openssl/releases/download/OpenSSL_1_1_1u/openssl-1.1.1u.tar.gz
[chenchen@dev3_10.211.21.18 openssl111u]$ tar zxvf openssl-1.1.1u.tar.gz -C ./

# 自定义安装规划
# 同一系统并且同一环境下, 安装多个 OpenSSL, 个版本的共享库会冲突, 避免冲突需要 no-shared 参数.
$ ./config no-shared --prefix=/usr/home/chenchen/htdocs/openssl111u --openssldir=/usr/home/chenchen/htdocs/openssl111u/ssl --libdir=/usr/home/chenchen/htdocs/openssl111u/lib

--prefix=DIR      安装目录树(installation directory tree)的顶部. 默认值为: /usr/local
                  (/data1/www/htdocs/chenchen/openssl111u)
--openssldir=DIR  OpenSSL 配置文件的目录, 以及默认证书和密钥存储. 默认值为: /usr/local/ssl
                  (/data1/www/htdocs/chenchen/openssl111u/ssl)                  
--libdir=DIR      安装目录树(installation directory tree)的顶部下的目录名称(请参阅 --prefix 选项), 其中将安装库. 默认情况下, 这是 "lib".
                  (/data1/www/htdocs/chenchen/openssl111u/lib)

installation directory tree/      => --prefix=DIR       => --prefix=/data1/www/htdocs/chenchen/openssl111u          => 顶部安装目录

installation directory tree/ssl   => --openssldir=DIR   => --openssldir=/data1/www/htdocs/chenchen/openssl111u/ssl  => 配置文件, 证书, 密钥, *权限

installation directory tree/lib   => --libdir=DIR       => --libdir=/data1/www/htdocs/chenchen/openssl111u/lib      => 库


# Unix 安装后目录结果说明:
bin/            包含 openssl 二进制文件和其他一些实用程序脚本. (由 --prefix=DIR 控制)
include/openssl 包含想要构建自己的使用 libcrypto 或 libssl 的程序所需的头文件. (--prefix=DIR)

lib             包含 OpenSSL 库文件 (--libdir=DIR)
lib/engines     包含 OpenSSL 动态可加载引擎 (--libdir=DIR)

ssl/            OpenSSL 配置文件的目录, 以及默认证书和密钥存储. (--openssldir=DIR )

share/man/man1 包含 OpenSSL 命令行手册页 (--prefix=DIR)
share/man/man3 包含 OpenSSL 库调用手册页 (--prefix=DIR)
share/man/man5 包含 OpenSSL 配置格式手册页 (--prefix=DIR)
share/man/man7 包含 OpenSSL 其他杂项手册页 (--prefix=DIR)

share/doc/openssl/html/man1 (--prefix=DIR)
share/doc/openssl/html/man3 (--prefix=DIR)
share/doc/openssl/html/man5 (--prefix=DIR)
share/doc/openssl/html/man7 包含手册页的 HTML 格式副本  (--prefix=DIR)

查看 make 的结果
$ ./configdata.pm --help

[chenchen@dev3_10.211.21.18 openssl-1.1.1u]$ pwd
/usr/home/chenchen/htdocs/openssl111u/openssl-1.1.1u
[chenchen@dev3_10.211.21.18 openssl-1.1.1u]$ ./config no-shared --prefix=/usr/home/chenchen/htdocs/openssl111u --openssldir=/usr/home/chenchen/htdocs/openssl111u/ssl --libdir=/usr/home/chenchen/htdocs/openssl111u/lib
Operating system: x86_64-whatever-linux2
Configuring OpenSSL version 1.1.1u (0x1010115fL) for linux-x86_64
Using os-specific seed configuration
Creating configdata.pm
Creating Makefile

**********************************************************************
***                                                                ***
***   OpenSSL has been successfully configured                     ***
***                                                                ***
***   If you encounter a problem while building, please open an    ***
***   issue on GitHub <https://github.com/openssl/openssl/issues>  ***
***   and include the output from the following command:           ***
***                                                                ***
***       perl configdata.pm --dump                                ***
***                                                                ***
***   (If you are new to OpenSSL, you might want to consult the    ***
***   'Troubleshooting' section in the INSTALL file first)         ***
***                                                                ***
**********************************************************************
[chenchen@dev3_10.211.21.18 openssl-1.1.1u]$ 

[chenchen@dev3_10.211.21.18 openssl-1.1.1u]$ make
[chenchen@dev3_10.211.21.18 openssl-1.1.1u]$ make test
make depend && make _tests
...
[chenchen@dev3_10.211.21.18 openssl-1.1.1u]$ make install

# Note
1. no-shared 问题:
默认是 shared, 支持共享库. 共享库由于是共享的, 所以在同一个系统并且同一环境下, 只能共享一个库. 此时如果有不同的程序需要使用不同版本的 OpenSSL 时, 就会出问题. 
具体表现是, bin/openssl version -a, 会返回与 bin/openssl 本身不同的 OPENSSLDIR 和 ENGINESDIR 等.
解决办法:
采用非共享库安装各自的 OpenSSL. 安装后通过 bin/openssl version -a 检查, 应该返回各自的独立的内容. 而不是返回 OPENSSLDIR 相同的内容.

2. --prefix=DIR, --openssldir=DIR, --libdir=DIR 问题:
看本块前面部分, 有详细说明, 结论是: 这三个参数确实有效果, 如有问题一定是自己的问题.

3. perl-Test-Simple 问题:
在 make test 时返回 Dubious, test returned 2 (wstat 512, 0x200) No subtests run 大量错误, 并且测试失败. 这是因为测试使用的是 perl 脚本, 需要对应的 perl 模块支持. 缺少模块. 直接 yum 安装.
sudo yum -y install perl-Test-Simple
满足 make test, 需要安装以下模块.
yum install -y gcc pam-devel zlib-devel perl expat-devel perl-Time-HiRes perl-Test-Harness perl-Test-Simple
主要是 perl 相关的几个: 
expat-devel
perl
perl-Time-HiRes
perl-Test-Harness
perl-Test-Simple
查看:
rpm -qa | grep perl
rpm -ql perlxxx

4. LD_LIBRARY_PATH 问题:
# 当修改完 ~/.bashrc 环境变量后, ./openssl version -a, 返回的是正常预期的 openssldir 和 libdir了
vim ~/.bashrc
#export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/openssl111m/lib  #.so file path
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/home/chenchen/htdocs/openssl111u/lib  #.so file path
以上方法仅是在环境变量层面控制共享库位置, 并且还没有用系统 /etc/ld.so.conf 控制, 但这种方法仅能在同一个环境下控制一个 OpenSSL, 如果多个版本就不行了.
那么问题来了, 如何在一个环境下共存多个 OpenSSL. (只能采用非共享库方式安装了)
当环境变量有多个设置时, 优先前面的. 也就是说, 一个环境变量仅对应一个lib指定(优先遇到的, 起效openssl111u, 而不是 openssl111m). 
LD_LIBRARY_PATH=:/usr/home/chenchen/htdocs/openssl111u/lib:/usr/local/openssl111m/lib
```

## openssl-3.1.1

```sh
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ pwd
/usr/home/chenchen/htdocs/openssl311/openssl-3.1.1
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ wget https://github.com/openssl/openssl/releases/download/openssl-3.1.1/openssl-3.1.1.tar.gz
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ tar zxvf openssl-3.1.1.tar.gz -C ./

./Configure no-shared --prefix=/usr/home/chenchen/htdocs/openssl311 --openssldir=/usr/home/chenchen/htdocs/openssl311/ssl --libdir=/usr/home/chenchen/htdocs/openssl311/lib				(采用)

# 问题出现:
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ ./Configure no-shared --prefix=/usr/home/chenchen/htdocs/openssl311 --openssldir=/usr/home/chenchen/htdocs/openssl311/ssl --libdir=/usr/home/chenchen/htdocs/openssl311/lib
Can't locate IPC/Cmd.pm in @INC (@INC contains: /data1/www/htdocs/chenchen/openssl311/openssl-3.1.1/util/perl /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 . /data1/www/htdocs/chenchen/openssl311/openssl-3.1.1/external/perl/Text-Template-1.56/lib) at /data1/www/htdocs/chenchen/openssl311/openssl-3.1.1/util/perl/OpenSSL/config.pm line 19.
BEGIN failed--compilation aborted at /data1/www/htdocs/chenchen/openssl311/openssl-3.1.1/util/perl/OpenSSL/config.pm line 19.
Compilation failed in require at ./Configure line 23.
BEGIN failed--compilation aborted at ./Configure line 23.
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ sudo yum -y install perl-IPC-Cmd		(解决)

[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ ./Configure no-shared --prefix=/usr/home/chenchen/htdocs/openssl311 --openssldir=/usr/home/chenchen/htdocs/openssl311/ssl --libdir=/usr/home/chenchen/htdocs/openssl311/lib
Configuring OpenSSL version 3.1.1 for target linux-x86_64
Using os-specific seed configuration
Created configdata.pm
Running configdata.pm
Created Makefile.in
Created Makefile
Created include/openssl/configuration.h

**********************************************************************
***                                                                ***
***   OpenSSL has been successfully configured                     ***
***                                                                ***
***   If you encounter a problem while building, please open an    ***
***   issue on GitHub <https://github.com/openssl/openssl/issues>  ***
***   and include the output from the following command:           ***
***                                                                ***
***       perl configdata.pm --dump                                ***
***                                                                ***
***   (If you are new to OpenSSL, you might want to consult the    ***
***   'Troubleshooting' section in the INSTALL.md file first)      ***
***                                                                ***
**********************************************************************
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ 

[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ make -j4
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ make test
[chenchen@dev3_10.211.21.18 openssl-3.1.1]$ make install
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









## OpenSSL3.x 分支

注意: 最新的稳定版本是支持到 2026 年 9 月 7 日的 3.0 系列. 这也是长期支持 (LTS) 版本. 之前的 LTS 版本(1.1.1 系列)也可用, 并且支持到 2023 年 9 月 11 日. 所有旧版本(包括 1.1.0、1.0.2、1.0.0 和 0.9.8)现在都不再支持, 应该不被使用. 鼓励这些旧版本的用户尽快升级到 3.0. 提供了对 1.0.2 的扩展支持, 以访问该版本的安全修复程序.

OpenSSL 3.0 是 OpenSSL 的最新主要版本. OpenSSL FIPS 对象模块 (FOM) 3.0 是 OpenSSL 3.0 下载的集成部分. 您无需单独下载 3.0 FOM. 参考下载里面的安装说明, 使用 “enable-fips” 编译时配置选项来构建它.

```shell
1. 开始前的必要准备条件
. make 工具
. Perl 5 带有核心模块
. Perl 模块 Text::Template
. ANSI C 编译器
. 带有 C头文件 和 开发库 的开发环境
. 被支持的操作系统

2. 快速开始和安装(默认位置)
### Unix / Linux / macOS
    $ ./Configure
    $ make
    $ make test
上面的命令会将 OpenSSL 安装到系统默认位置.

出于安全原因, 默认系统位置默认不可写对于非特权用户. 因此, 对于最终安装步骤管理特权是必需的. 
默认系统位置和过程获得管理权限取决于操作系统. 
建议使用 普通用户权限编译 和 测试 OpenSSL 并仅对 最终安装 步骤使用 管理权限. 
# 普通用户权限: 编译 和 测试
#	管理员权限: 安装

在某些平台上, OpenSSL 作为操作系统的一部分预安装. 
在这种情况下, 强烈建议不要覆盖系统版本, 因为其他应用程序或库可能依赖于它.
要避免破坏其他应用程序, 请将 OpenSSL 的副本安装到[different location](#installing-to-a-different-location)
[不同地点](#installing到不同位置)不在系统库的全局搜索路径.
### Unix / Linux / macOS

根据您的发行版, 您需要将以下命令作为 root 用户或命令前面附加的 "sudo":
    $ sudo make install
默认情况下, OpenSSL 将安装到
    /usr/local
更准确地说, 文件将安装到子目录中
    /usr/local/bin
    /usr/local/lib
    /usr/local/include
    ...
取决于文件类型, 因为它是在 类Unix 操作系统上自定义的.

3. 安装到其他位置
将 OpenSSL 安装到其他位置(例如, 安装到 home 用于测试目的的目录) 运行 "配置", 如下所示, 例子 
选项 `--prefix` and `--openssldir` 在下面的 [Directories](#directories) 中有更详细的解释, 这里使用的值只是示例.
在 Unix 上: 
    $ ./Configure --prefix=/opt/openssl --openssldir=/usr/local/ssl

目录:
### libdir
    --libdir=DIR
此目录是 库安装的位置 在顶级目录的下面, 顶级目录是 `--prefix` 选项指定的位置. 默认叫做 `lib`. 
一些构建目标在构建配置中设置了 multilib 后缀. 对于这些目标, 默认的 libdir 是 `lib< multilib-postfix>`. 
如果不希望添加后缀, 那么, 请用 `--libdir=lib` 覆盖 `libdir`.
## openssldir
    --openssldir=DIR
OpenSSL 配置文件的目录, 以及默认证书和密钥库. 默认值为 `Unix: /usr/local/ssl`
### prefix
    --prefix=DIR
安装目录的根目录, 默认: `Unix: /usr/local`

4. 构建失败
    $ make clean		# Unix

5. 共享库注意事项
在大多数 POSIX 平台上, 共享库被命名为 `libcrypto.so.1.1` 和 `libssl.so.1.1`.

```





```shell
# OpenSSL3.x 
[chenchen@grpc01 tmp]$ wget https://www.openssl.org/source/openssl-3.0.4.tar.gz.sha256 --no-check-certificate
[chenchen@grpc01 tmp]$ sha256sum openssl-3.0.4.tar.gz
2831843e9a668a0ab478e7020ad63d2d65e51f72977472dc73efcefbafc0c00f  openssl-3.0.4.tar.gz
[chenchen@grpc01 tmp]$ wget https://www.openssl.org/source/openssl-3.0.4.tar.gz.sha256 --no-check-certificate
[chenchen@grpc01 tmp]$ less openssl-3.0.4.tar.gz.sha256
[chenchen@grpc01 tmp]$ tar -zxvf openssl-3.0.4.tar.gz -C ./
[chenchen@grpc01 tmp]$ cd openssl-3.0.4
[chenchen@grpc01 tmp]$ less README.md
[chenchen@grpc01 tmp]$ less INSTALL.md
[chenchen@grpc01 tmp]$ ./Configure --prefix=/usr/local/openssl-3.0.4 --libdir=lib --openssldir=/usr/local/ssl
[chenchen@grpc01 tmp]$ make
[chenchen@grpc01 tmp]$ make test
[chenchen@grpc01 tmp]$ sudo make install
[chenchen@grpc01 tmp]$ make clean

```

```shell
# Troubleshooting
[chenchen@grpc01 openssl-3.0.4]$ ./Configure --prefix=/usr/local/openssl-3.0.4 --libdir=lib --openssldir=/usr/local/ssl
Can't locate IPC/Cmd.pm in @INC (@INC contains: /home/chenchen/tmp/openssl-3.0.4/util/perl /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 . /home/chenchen/tmp/openssl-3.0.4/external/perl/Text-Template-1.56/lib) at /home/chenchen/tmp/openssl-3.0.4/util/perl/OpenSSL/config.pm line 18.
BEGIN failed--compilation aborted at /home/chenchen/tmp/openssl-3.0.4/util/perl/OpenSSL/config.pm line 18.
Compilation failed in require at ./Configure line 24.
BEGIN failed--compilation aborted at ./Configure line 24.
[chenchen@grpc01 openssl-3.0.4]$ less 
Missing filename ("less --help" for help)
[chenchen@grpc01 openssl-3.0.4]$ less NOTES-PERL.md

# Required Perl modules
* Text::Template this is required *for building*
* `Test::More` this is required *for testing*

# Notes on installing a Perl module
Install using CPAN.  This is very easy, but usually requires `root` access:
       $ sudo cpan -i Text::Template

# 以下是问题解决的办法, 其实是缺少了 IPC::Cmd 模块
[chenchen@grpc01 openssl-3.0.4]$ ./Configure --prefix=/usr/local/openssl-3.0.4 --libdir=lib --openssldir=/usr/local/ssl
Can't locate IPC/Cmd.pm in @INC (@INC contains: /home/chenchen/tmp/openssl-3.0.4/util/perl /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 . /home/chenchen/tmp/openssl-3.0.4/external/perl/Text-Template-1.56/lib) at /home/chenchen/tmp/openssl-3.0.4/util/perl/OpenSSL/config.pm line 18.
BEGIN failed--compilation aborted at /home/chenchen/tmp/openssl-3.0.4/util/perl/OpenSSL/config.pm line 18.
Compilation failed in require at ./Configure line 24.
BEGIN failed--compilation aborted at ./Configure line 24.
[chenchen@grpc01 openssl-3.0.4]$ vim /home/chenchen/tmp/openssl-3.0.4/util/perl/OpenSSL/config.pm
# 这里插播, 初次安装模块时, 会让选择全自动方式还是交互方式, 我选的是全自动方式
[chenchen@grpc01 openssl-3.0.4]$ sudo cpan -i IPC::Cmd
[chenchen@grpc01 openssl-3.0.4]$ ./Configure --prefix=/usr/local/openssl-3.0.4 --libdir=lib --openssldir=/usr/local/ssl
Configuring OpenSSL version 3.0.4 for target linux-x86_64
Using os-specific seed configuration
Created configdata.pm
Running configdata.pm
Created Makefile.in
Created Makefile
Created include/openssl/configuration.h

**********************************************************************
***                                                                ***
***   OpenSSL has been successfully configured                     ***
***                                                                ***
***   If you encounter a problem while building, please open an    ***
***   issue on GitHub <https://github.com/openssl/openssl/issues>  ***
***   and include the output from the following command:           ***
***                                                                ***
***       perl configdata.pm --dump                                ***
***                                                                ***
***   (If you are new to OpenSSL, you might want to consult the    ***
***   'Troubleshooting' section in the INSTALL.md file first)      ***
***                                                                ***
**********************************************************************
[chenchen@grpc01 openssl-3.0.4]$ make
[chenchen@grpc01 openssl-3.0.4]$ make test
[chenchen@grpc01 openssl-3.0.4]$ sudo make install			(sudo)
[chenchen@grpc01 openssl-3.0.4]$ make clean





```



```sh
网文: https://www.jianshu.com/p/092c060510e4?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation

升级openssl版本
1 简介
根据安全扫描结果，由于自带的opensl的版本存在漏洞，所以需要升级openssl版本。但是由于机器数量过多。并且有些机器可以连接外网，有些机器是存内网的状态，所以提供一些两种方法进行升级。

系统版本：centos7.2

2 方法一（rpm包安装）
1、去网站下载rpm的安装包。网站地址如下 https://pkgs.org/download/openssl
2、按照你的系统版本选择要下载的rpm包
OpenSSL 1.0.2k-16.el7.x86_64.rpm
3、下载相应的依赖包
因为openssl依赖openssl-devel和openssl-libs两个包，所以也要下载相应的包。
4、上传到服务器上，并且安装(root权限)
# rpm -ivh openssl-libs-1.0.2k-16.el7.x86_64.rpm --force --nodeps
# rpm -ivh openssl-devel-1.0.2k-16.el7.x86_64.rpm --force --nodeps
# rpm -ivh openssl-1.0.2k-16.el7.x86_64.rpm --force --nodeps
命令解释:
--nodeps就是安装时不检查依赖关系，比如你这个rpm需要A，但是你没装A，这样你的包就装不上，用了--nodeps你就能装上了。
--force就是强制安装，比如你装过这个rpm的版本1，如果你想装这个rpm的版本2，就需要用--force强制安装
4、验证
# openssl version
OpenSSL 1.0.2k-fips 26 Jan 2017

3 方法二（yum update）
此方法方便简单，但是无法升级到指定的版本。
1、查看openssl版本
# openssl version
OpenSSL 1.0.1e-fips 11 Feb 2013
2、把包信息下载到本地电脑缓存
# yum makecache
3、升级openssl
# yum update openssl
4、验证升级结果
# openssl version
OpenSSL 1.0.2k-fips 26 Jan 2017
```

































