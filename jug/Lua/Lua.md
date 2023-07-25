# Lua

[toc]



## Overview

### Download

```sh
# download
[chenchen@dev3_10.211.21.18 htdocs]$ mkdir lua
[chenchen@dev3_10.211.21.18 htdocs]$ cd lua
[chenchen@dev3_10.211.21.18 lua]$ l
total 16K
23446426 drwxrwxr-x  2 chenchen chenchen 4.0K Jul 25 10:24 .
23330989 drwxr-xr-x 89 chenchen www       12K Jul 25 10:24 ..
[chenchen@dev3_10.211.21.18 lua]$ wget https://www.lua.org/ftp/lua-5.4.6.tar.gz
--2023-07-25 10:24:38--  https://www.lua.org/ftp/lua-5.4.6.tar.gz
Connecting to 127.0.0.1:7890... connected.
ERROR: cannot verify www.lua.org's certificate, issued by ‘/C=US/O=Let's Encrypt/CN=R3’:
  Issued certificate has expired.
To connect to www.lua.org insecurely, use `--no-check-certificate'.
[chenchen@dev3_10.211.21.18 lua]$ wget https://www.lua.org/ftp/lua-5.4.6.tar.gz --no-check-certificate
--2023-07-25 10:24:57--  https://www.lua.org/ftp/lua-5.4.6.tar.gz
Connecting to 127.0.0.1:7890... connected.
WARNING: cannot verify www.lua.org's certificate, issued by ‘/C=US/O=Let's Encrypt/CN=R3’:
  Issued certificate has expired.
Proxy request sent, awaiting response... 200 OK
Length: 363329 (355K) [application/gzip]
Saving to: ‘lua-5.4.6.tar.gz’

100%[==================================================================================================================================================================================>] 363,329      130KB/s   in 2.7s   

2023-07-25 10:25:01 (130 KB/s) - ‘lua-5.4.6.tar.gz’ saved [363329/363329]

# tar zxvf
[chenchen@dev3_10.211.21.18 lua]$ l
total 372K
23444302 -rw-rw-r--  1 chenchen chenchen 355K May  3 04:12 lua-5.4.6.tar.gz
23330989 drwxr-xr-x 89 chenchen www       12K Jul 25 10:24 ..
23446426 drwxrwxr-x  2 chenchen chenchen 4.0K Jul 25 10:24 .
[chenchen@dev3_10.211.21.18 lua]$ tar zxvf lua-5.4.6.tar.gz 
...
...
[chenchen@dev3_10.211.21.18 lua]$ l
total 376K
23444302 -rw-rw-r--  1 chenchen chenchen 355K May  3 04:12 lua-5.4.6.tar.gz
23446427 drwxr-xr-x  4 chenchen chenchen 4.0K May  3 04:12 lua-5.4.6
23330989 drwxr-xr-x 89 chenchen www       12K Jul 25 10:24 ..
23446426 drwxrwxr-x  3 chenchen chenchen 4.0K Jul 25 10:28 .

# sha256sum
[chenchen@dev3_10.211.21.18 lua]$ sha256sum
^C
[chenchen@dev3_10.211.21.18 lua]$ sha256sum --help
Usage: sha256sum [OPTION]... [FILE]...
Print or check SHA256 (256-bit) checksums.
With no FILE, or when FILE is -, read standard input.

  -b, --binary         read in binary mode
  -c, --check          read SHA256 sums from the FILEs and check them
      --tag            create a BSD-style checksum
  -t, --text           read in text mode (default)
  Note: There is no difference between binary and text mode option on GNU system.

The following four options are useful only when verifying checksums:
      --quiet          don't print OK for each successfully verified file
      --status         don't output anything, status code shows success
      --strict         exit non-zero for improperly formatted checksum lines
  -w, --warn           warn about improperly formatted checksum lines

      --help     display this help and exit
      --version  output version information and exit

The sums are computed as described in FIPS-180-2.  When checking, the input
should be a former output of this program.  The default mode is to print
a line with checksum, a character indicating input mode ('*' for binary,
space for text), and name for each FILE.

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
For complete documentation, run: info coreutils 'sha256sum invocation'
[chenchen@dev3_10.211.21.18 lua]$ sha256sum lua-5.4.6.tar.gz
7d5ea1b9cb6aa0b59ca3dde1c6adcb57ef83a1ba8e5432c0ecd06bf439b3ad88  lua-5.4.6.tar.gz
[chenchen@dev3_10.211.21.18 lua]$ 
```

### Building

```sh
# https://www.lua.org/faq.html#1.1
# https://www.lua.org/manual/5.4/readme.html

如果你没有时间或意愿自己编译Lua, 可以从 LuaBinaries 获取二进制文件. http://luabinaries.sourceforge.net/
如果您只想尝试 Lua, 请尝试现场演示. https://www.lua.org/demo.html

# 构建详细文档
# https://www.lua.org/manual/5.4/readme.html
1. 移动到 lua-5.4.6 目录根下, 这里有个 Makefile 文件, 这个文件控制构建和安装处理的配置.
2. 当执行 "make" 时, 这个 Makefile 会猜你的系统平台.
3. 如果猜失败了, 通过 "make help" 查看目前支持的平台列表. 列表如下: 
	 guess aix bsd c89 freebsd generic ios linux linux-readline macosx mingw posix solaris
	 如果你的平台在列表里, 可以执行 "make xxx", xxx是平台名字.
	 如果你的平台不在列表里, 按这个顺序, 尝试一个接近的平台名, posix, generic, c89
4. 编译只需要几分钟, 并在 src 目录中生成三个文件: lua(解释器), luac(编译器) 和 liblua.a(库).
5. 要检查 Lua 是否已正确构建, 请在构建 Lua 后进行 "make test". 这将运行解释器并打印其版本.

如果您运行的是 Linux, 请尝试 "make linux-readline" 来构建具有方便的行编辑和历史记录功能的交互式 Lua 解释器. 如果出现编译错误, 请确保已安装 readline 开发包(可能名为 libreadline-dev 或 readline-devel). 如果之后出现链接错误, 请尝试 "make linux-readline MYLIBS=-ltermcap".
```

```sh
# 查看当前 platform 信息, 我认为是 uname -a 的第一个字段 Linux

[chenchen@dev3_10.211.21.18 lua]$ uname -a
Linux dev5 3.10.0-514.el7.x86_64 #1 SMP Tue Nov 22 16:42:41 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

[chenchen@dev3_10.211.21.18 lua]$ lsb_release -a
-bash: lsb_release: command not found
[root@dev3_10.211.21.18 ~]# yum -y install redhat-lsb
```

### Install

```sh
构建 Lua 之后就可以安装了.
1. 安装在官方位置. 
"make install" 安装在官方位置. 官方位置定义在 Makefile 文件里. 有可能需要权限安装 "sudo make install".
通过一步完成构建和安装, "make all install" 或者 "make xxx install", xxx 是平台名字.

2. 自定义安装位置. 要在构建 Lua 后在本地安装 Lua, 执行 "make local". 这将创建一个目录来安装, 其中包括子目录 bin, include, lib, man, share, 如下所示:
这将创建一个包含子目录 bin、include、lib、man、share 和安装 Lua 的目录安装, 如下所示:
bin:
		lua luac
include:
		lua.h luaconf.h lualib.h lauxlib.h lua.hpp
lib:
		liblua.a
man/man1:
		lua.1 luac.1

安装到本地其他目录中, "make install INSTALL_TOP=xxx", xxx 是你选择的目录. 
安装从 src 和 doc 目录中开始, 因此如果 INSTALL_TOP 不是绝对路径, 请小心.

这些是开发所需的唯一目录. 如果你只想运行 Lua 程序, 你只需要 bin 和 man 中的文件. 包含和 lib 中的文件是将 Lua 嵌入 C 或 C++ 程序中所必需的.
```

### Customization 定制

```sh
# 定制
通过编辑文件可以自定义三种内容: 
- 在哪里以及如何安装 Lua — 编辑 Makefile。
- 如何构建 Lua — 编辑 src/Makefile。
- Lua 功能 — 编辑 src/luaconf.h.

您实际上不需要编辑 Makefile, 因为您可以在调用 "make" 时在命令行中设置相关变量. 不过, 最好编辑并保存生成文件以记录您所做的更改.

另一方面, 如果您需要自定义一些 Lua 功能, 则需要在构建和安装 Lua 之前编辑 src/luaconf.h. 编辑后的文件将是安装的文件, 并且将由您构建的任何 Lua 客户端使用, 以确保一致性. 专家可以通过编辑 Lua 源码进行进一步的定制.
```

### Building Lua on other systems

```sh
如果您没有使用通常的 Unix 工具, 那么构建 Lua 的说明取决于您使用的编译器. 您需要创建项目(或编译器使用的任何内容)来构建库、解释器和编译器, 如下所示:

library:
		lapi.c lcode.c lctype.c ldebug.c ldo.c ldump.c lfunc.c lgc.c llex.c lmem.c lobject.c 		
		lopcodes.c lparser.c lstate.c lstring.c ltable.c ltm.c lundump.c lvm.c lzio.c lauxlib.c 
		lbaselib.c lcorolib.c ldblib.c liolib.c lmathlib.c loadlib.c loslib.c lstrlib.c ltablib.c 
		lutf8lib.c linit.c
interpreter:
		library, lua.c
compiler:
		library, luac.c

要在您自己的程序中将 Lua 用作库, 您需要知道如何使用编译器创建和使用库. 
此外, 要为 Lua 动态加载 C 库, 您需要知道如何创建动态库, 并且需要确保这些动态库可以访问 Lua API 函数, 但不要将 Lua 库链接到每个动态库. 对于 Unix, 我们建议将 Lua 库静态链接到主机程序中, 并导出其符号以进行动态链接; src/Makefile 为 Lua 解释器执行此操作. 对于 Windows, 我们建议 Lua 库为 DLL. 在所有情况下, 编译器 luac 都应该静态链接. 

如上所述, 您可以在构建 Lua 之前编辑 src/luaconf.h 以自定义某些功能.
```

### Practical

```sh
# 准备
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ make help
make[1]: Entering directory `/data1/www/htdocs/chenchen/lua/lua-5.4.6/src'
Do 'make PLATFORM' where PLATFORM is one of these:
   guess aix bsd c89 freebsd generic ios linux linux-readline macosx mingw posix solaris
See doc/readme.html for complete instructions.
make[1]: Leaving directory `/data1/www/htdocs/chenchen/lua/lua-5.4.6/src'
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ 

# 编辑 lua-5.4.6/Makefile 文件
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ vim Makefile
# 这里修改一行配置, 指定 lua 安装(install)的目录位置.
# INSTALL_TOP= /usr/local
INSTALL_TOP= /usr/home/chenchen/htdocs/lua

# 构建, 通过指定 linux-readline 来构建. 
# 如果是 Linux 平台, 当官方建议用 linux-readline 来尝试构建
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ make linux-readline
......

# 准备安装, 为安装提前创建目录
# lua-5.4.6/ 目录下4个文件, doc, README, Makefile, src
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ l
total 24K
23446428 drwxr-xr-x 2 chenchen chenchen 4.0K May  3 04:09 doc
23444377 -rw-r--r-- 1 chenchen chenchen  151 May  3 04:12 README
23446426 drwxrwxr-x 3 chenchen chenchen 4.0K Jul 25 10:28 ..
23444380 -rw-r--r-- 1 chenchen chenchen 3.2K Jul 25 12:45 Makefile
23446429 drwxr-xr-x 2 chenchen chenchen 4.0K Jul 25 12:46 src
23446427 drwxr-xr-x 4 chenchen chenchen 4.0K Jul 25 12:52 .
# 创建 install 目录, 为最终安装而提前创建的目录, 其中包含
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ make local
make install INSTALL_TOP=../install
make[1]: Entering directory `/data1/www/htdocs/chenchen/lua/lua-5.4.6'
cd src && mkdir -p ../install/bin ../install/include ../install/lib ../install/man/man1 ../install/share/lua/5.4 ../install/lib/lua/5.4
cd src && install -p -m 0755 lua luac ../install/bin
cd src && install -p -m 0644 lua.h luaconf.h lualib.h lauxlib.h lua.hpp ../install/include
cd src && install -p -m 0644 liblua.a ../install/lib
cd doc && install -p -m 0644 lua.1 luac.1 ../install/man/man1
make[1]: Leaving directory `/data1/www/htdocs/chenchen/lua/lua-5.4.6'
# 多了一个 install 目录
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ l
total 28K
23446428 drwxr-xr-x 2 chenchen chenchen 4.0K May  3 04:09 doc
23444377 -rw-r--r-- 1 chenchen chenchen  151 May  3 04:12 README
23446426 drwxrwxr-x 3 chenchen chenchen 4.0K Jul 25 10:28 ..
23444380 -rw-r--r-- 1 chenchen chenchen 3.2K Jul 25 12:45 Makefile
23446429 drwxr-xr-x 2 chenchen chenchen 4.0K Jul 25 12:46 src
23446430 drwxrwxr-x 7 chenchen chenchen 4.0K Jul 25 12:55 install
23446427 drwxr-xr-x 5 chenchen chenchen 4.0K Jul 25 12:55 .
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ l ..
total 376K
23444302 -rw-rw-r--  1 chenchen chenchen 355K May  3 04:12 lua-5.4.6.tar.gz
23330989 drwxr-xr-x 89 chenchen www       12K Jul 25 10:24 ..
23446426 drwxrwxr-x  3 chenchen chenchen 4.0K Jul 25 10:28 .
23446427 drwxr-xr-x  5 chenchen chenchen 4.0K Jul 25 12:55 lua-5.4.6
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ 
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ tree install/
install/
├── bin
│   ├── lua
│   └── luac
├── include
│   ├── lauxlib.h
│   ├── luaconf.h
│   ├── lua.h
│   ├── lua.hpp
│   └── lualib.h
├── lib
│   ├── liblua.a
│   └── lua
│       └── 5.4
├── man
│   └── man1
│       ├── lua.1
│       └── luac.1
└── share
    └── lua
        └── 5.4

# 安装
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ make install
cd src && mkdir -p /usr/home/chenchen/htdocs/lua/bin /usr/home/chenchen/htdocs/lua/include /usr/home/chenchen/htdocs/lua/lib /usr/home/chenchen/htdocs/lua/man/man1 /usr/home/chenchen/htdocs/lua/share/lua/5.4 /usr/home/chenchen/htdocs/lua/lib/lua/5.4
cd src && install -p -m 0755 lua luac /usr/home/chenchen/htdocs/lua/bin
cd src && install -p -m 0644 lua.h luaconf.h lualib.h lauxlib.h lua.hpp /usr/home/chenchen/htdocs/lua/include
cd src && install -p -m 0644 liblua.a /usr/home/chenchen/htdocs/lua/lib
cd doc && install -p -m 0644 lua.1 luac.1 /usr/home/chenchen/htdocs/lua/man/man1
# lua-5.4.6/ 目录下无变化, 还是之前多了一个 install 目录
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ l
total 28K
23446428 drwxr-xr-x 2 chenchen chenchen 4.0K May  3 04:09 doc
23444377 -rw-r--r-- 1 chenchen chenchen  151 May  3 04:12 README
23444380 -rw-r--r-- 1 chenchen chenchen 3.2K Jul 25 12:45 Makefile
23446429 drwxr-xr-x 2 chenchen chenchen 4.0K Jul 25 12:46 src
23446430 drwxrwxr-x 7 chenchen chenchen 4.0K Jul 25 12:55 install
23446427 drwxr-xr-x 5 chenchen chenchen 4.0K Jul 25 12:55 .
23446426 drwxrwxr-x 8 chenchen chenchen 4.0K Jul 25 13:00 ..
# 在 Makefile 中 INSTALL_TOP= /usr/home/chenchen/htdocs/lua 指定的位置多了 5 个目录: 
# 分别为, bin, share, lib, include, man 目录, 没有文件
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ l ..
total 396K
23444302 -rw-rw-r--  1 chenchen chenchen 355K May  3 04:12 lua-5.4.6.tar.gz
23330989 drwxr-xr-x 89 chenchen www       12K Jul 25 10:24 ..
23446427 drwxr-xr-x  5 chenchen chenchen 4.0K Jul 25 12:55 lua-5.4.6
23446446 drwxrwxr-x  3 chenchen chenchen 4.0K Jul 25 13:00 share
23446444 drwxrwxr-x  3 chenchen chenchen 4.0K Jul 25 13:00 man
23446426 drwxrwxr-x  8 chenchen chenchen 4.0K Jul 25 13:00 .
23446441 drwxrwxr-x  2 chenchen chenchen 4.0K Jul 25 13:00 bin
23446442 drwxrwxr-x  2 chenchen chenchen 4.0K Jul 25 13:00 include
23446443 drwxrwxr-x  3 chenchen chenchen 4.0K Jul 25 13:00 lib
[chenchen@dev3_10.211.21.18 lua-5.4.6]$ 

# $ lua --help
[chenchen@dev3_10.211.21.18 bin]$ lua --help
usage: lua [options] [script [args]].
Available options are:
  -e stat  execute string 'stat'
  -l name  require library 'name'
  -i       enter interactive mode after executing 'script'
  -v       show version information
  --       stop handling options
  -        execute stdin and stop handling options
  
# $ lua -v
[chenchen@dev3_10.211.21.18 bin]$ lua -v
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
[chenchen@dev3_10.211.21.18 bin]$ pwd
/usr/home/chenchen/htdocs/lua/bin

# 检查
# 为了防止安装到系统环境上 (应该找不到)
[chenchen@dev3_10.211.21.18 bin]$ l /bin | grep lua
[chenchen@dev3_10.211.21.18 bin]$ l /sbin | grep lua
[chenchen@dev3_10.211.21.18 bin]$ l /usr/local/bin | grep lua
[chenchen@dev3_10.211.21.18 bin]$ l /usr/local/sbin | grep lua

# 更新自己环境变量 (为了便于使用)
[chenchen@dev3_10.211.21.18 bin]$ vim ~/.bash_profile 
# lua
export PATH=/usr/home/chenchen/htdocs/lua/bin:$PATH
[chenchen@dev3_10.211.21.18 bin]$ lua -v
Lua 5.4.6  Copyright (C) 1994-2023 Lua.org, PUC-Rio

```











## FAQ







## See Also

https://www.lua.org/manual/5.4/readme.html

https://www.lua.org/download.html

https://www.lua.org/faq.html#1.1

https://luarocks.org/

https://github.com/LewisJEllis/awesome-lua

https://luabinaries.sourceforge.net/



