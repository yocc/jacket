# Python3

---



### 目录

[TOC]



### 写在前面

```shell
写在前面: 为了减少源码安装的异常情况, 必要采取两步, 1. 检验源码签名, SIG或者MD5; 2. 编译前先清理一下, make distclean;
1. 获取源码, tar zxvf 源码.tgz -C ./
验证下载文件完整性
$ gpg --verify Python-3.8.5.tgz.asc
$ gpg --search-keys E3FF2839C048B25C084DEBE9B26995E310250568
$ gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E3FF2839C048B25C084DEBE9B26995E310250568
$ gpg --verify Python-3.8.5.tgz.asc
或者
[chenchen@localhost tmp]$ md5sum Python-3.8.5.tgz
e2f52bcf531c8cc94732c0b6ff933ff0  Python-3.8.5.tgz
```



### 下载 Python3 源码

```shell
$ sudo yum -y install wget
$ wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
```



### 解压缩

```shell
$ tar zxvf 源码.tgz -C ./

[chenchen@grpc01 tmp]$ tar zxvf Python-3.10.2.tgz -C ./
Python-3.10.2/
Python-3.10.2/Mac/
Python-3.10.2/Mac/README.rst
...
```



### 基本工具, ssl 模块, ctypes 模块, 其他各种模块

``` shell
$ sudo yum -y install gcc make automake autoconf libtool
$ sudo yum -y install openssl openssl-devel
$ sudo yum -y install libffi libffi-devel

$ sudo yum -y install bzip2 bzip2-devel
$ sudo yum -y install readline readline-devel
$ sudo yum -y install tk tk-devel
$ sudo yum -y install sqlite sqlite-devel
$ sudo yum -y install xz lzma xz-devel
$ sudo yum -y install uuid uuid-devel
#$ sudo yum -y install libuuid libuuid-devel  (libxxx 命名的一般是 Ubantu系统)
$ sudo yum -y install zlib zlib-devel
$ sudo yum -y install ncurses ncurses-devel
$ sudo yum -y install gdbm gdbm-devel

# 别用, 安装了好几十个
# $ sudo yum -y install yum-utils
# $ sudo yum -y groupinstall development
# db4-devel libpcap-devel expat-devel

# 官网建议安装前戏, 参考: https://devguide.python.org/setup/#linux
$ sudo yum install yum-utils			# 工具
$ sudo yum-builddep python3				# 为python3配置一个必须的构建环境和安装所需的依赖

# ln -s
$ ln -s 原文件 软连接名

```



### ./configure

```shell
./configure # 不默认安装 ./configure --prefix=/home/chenchen/program/python3 --exec-prefix=/home/chenchen/bin --enable-shared --enable-optimizations --enable-big-digits        # prefix 程序本身的位置; exec-prefix 执行文件的位置

./configure --prefix=$HOME/program/python3 --enable-shared --enable-optimizations --enable-big-digits  --with-system-ffi  (使用1)

./configure --prefix=$HOME/program/python3 --enable-shared --enable-big-digits  --with-system-ffi  (使用2, 参考 FAQ)
./configure --prefix=$HOME/tmp/python37 --enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/usr/local/openssl111m (成功, --with-openssl=OpenSSL本体根目录)
./configure --prefix=$HOME/tmp/python39 --enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/usr/local/openssl111m (成功, --with-openssl=OpenSSL本体根目录)

./configure --prefix=$HOME/program/python3 --exec-prefix=$HOME/bin --enable-shared --enable-optimizations --enable-big-digits  --with-system-ffi
```



### make, make install

```shell
make distclean 	# 清除 configure
./configure ...
make clean
make                    
make install
(VirtualBox 上需要30分钟左右)
```



### 配置环境变量

```shell
[chenchen@grpc01 lib]$ pwd
/home/chenchen/tmp/python37/lib
[chenchen@grpc01 lib]$ l
total 14M
377487472 -r-xr-xr-x.  1 chenchen chenchen  14M Mar 12 16:55 libpython3.7m.so.1.0
377487474 -r-xr-xr-x.  1 chenchen chenchen 7.5K Mar 12 16:55 libpython3.so
377487473 lrwxrwxrwx.  1 chenchen chenchen   20 Mar 12 16:55 libpython3.7m.so -> libpython3.7m.so.1.0
377487471 drwxr-xr-x.  4 chenchen chenchen  113 Mar 12 16:56 .
381681757 drwxr-xr-x. 35 chenchen chenchen 8.0K Mar 12 16:56 python3.7
369127301 drwxr-xr-x.  6 chenchen chenchen   56 Mar 12 16:56 ..
 96471001 drwxr-xr-x.  2 chenchen chenchen   67 Mar 12 16:56 pkgconfig
直接在 安装的本体根目录/bin下执行 ./python3 --version, 会提示共享库没有的错误, 需要将本体lib加入共享库搜索目录.
$ ~/tmp/python37/bin/python3 --version
.......
$ sudo vim /etc/ld.so.conf.d/python37.conf
vim 编辑添加一行 /home/chenchen/tmp/python37/lib
$ sudo ldconfig -vvv
 


```



### 检查

```shell
$ python --version
$ python2 --version
$ python2.7 --version
Python 2.7.5

$ whereis python						# 查看当前系统 python 可执行文件的位置
$ which python
```



### Creating Virtual Environments

```shell
Python “虚拟环境” 允许将 Python 软件包安装在特定应用程序的隔离位置, 而不是全局安装.
如果要安全安装全局命令行工具, 请参阅安装独立命令行工具. 
https://packaging.python.org/guides/installing-stand-alone-command-line-tools/

假设您有一个需要 LibFoo 版本1的应用程序, 但是 另一个应用程序需要版本2.
如果您将所有内容都安装在 /usr/lib/python3.6/site-packages (或平台的标准位置)中, 那么很容易在无意中升级不应升级的应用程序.

或更笼统地说, 如果您要安装应用程序并保留原样该怎么办? 如果应用程序正常运行, 则其库或这些库的版本中的任何更改都可能破坏该应用程序. 另外, 如果您无法将软件包安装到全局 site-packages 目录中, 该怎么办? 例如, 在共享主机上.
在所有这些情况下, 虚拟环境都可以为您提供帮助. 它们具有自己的安装目录, 并且不与其他虚拟环境共享库.

当前, 有两种用于创建 Python 虚拟环境的常用工具: 
1. venv 在 Python3.3 和更高版本中默认可用, 并在Python3.4 和更高版本中将 pip 和 setuptools 安装到创建的虚拟环境中.
Unix/macOS:
$ python3 -m venv <DIR>
$ source <DIR>/bin/activate				// 在虚拟环境目录DIR中/bin/activate
$ deactivate											// 退出虚拟环境

例如: 
[chenchen@localhost bin]$ . ./activate
(xxx.01.py3.9.5) [chenchen@localhost bin]$ python3
Python 3.9.5 (default, May  7 2021, 10:23:11) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/home/chenchen/program/python3.9.5/lib/python39.zip', '/home/chenchen/program/python3.9.5/lib/python3.9', '/home/chenchen/program/python3.9.5/lib/python3.9/lib-dynload', '/home/chenchen/program/venv/xxx.01.py3.9.5/lib/python3.9/site-packages']
>>> 
(xxx.01.py3.9.5) [chenchen@localhost bin]$ deactivate
[chenchen@localhost bin]$ 
[chenchen@localhost bin]$ python3
Python 3.9.5 (default, May  7 2021, 10:23:11) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/home/chenchen/program/python3.9.5/lib/python39.zip', '/home/chenchen/program/python3.9.5/lib/python3.9', '/home/chenchen/program/python3.9.5/lib/python3.9/lib-dynload', '/home/chenchen/program/python3.9.5/lib/python3.9/site-packages']
>>> 
[chenchen@localhost bin]$ 
其实 venv 虚拟环境的激活是修改了系统环境变量的优先级, 通过 env shell指令是可以观察到的.

2. virtualenv 需要单独安装, 但是支持 Python 2.7+ 和 Python 3.3+, 并且 pip, setuptools 和 wheel 默认情况下始终安装到创建的虚拟环境中(无论 Python 版本如何)
Unix/macOS:
$ python3 -m venv <DIR>
$ source <DIR>/bin/activate				// 同上
$ deactivate											// 退出虚拟环境
在 Unix shell 下使用 source 可以确保将虚拟环境的变量设置在当前shell中, 而不是在子进程中(子进程随后会消失, 没有任何作用)

直接管理多个虚拟环境可能变得很乏味, 因此依赖性管理指南引入了更高级别的工具 Pipenv, 该工具自动为您处理的每个项目和应用程序管理一个单独的虚拟环境. 
```

```shell
[chenchen@localhost venv]$ ~/tmp/python3/bin/python3 -m venv ./grpc01
[chenchen@localhost venv]$ l
total 0
16777807 drwxrwxr-x. 7 chenchen chenchen 183 Mar 11 01:07 ..
 1642888 drwxrwxr-x. 3 chenchen chenchen  20 Mar 11 01:08 .
19577901 drwxrwxr-x. 5 chenchen chenchen  74 Mar 11 01:08 grpc01
[chenchen@localhost venv]$ cd grpc01/
[chenchen@localhost grpc01]$ l
total 4.0K
35025244 drwxrwxr-x. 2 chenchen chenchen   6 Mar 11 01:08 include
 1642888 drwxrwxr-x. 3 chenchen chenchen  20 Mar 11 01:08 ..
19577904 -rw-rw-r--. 1 chenchen chenchen  92 Mar 11 01:08 pyvenv.cfg
19577903 lrwxrwxrwx. 1 chenchen chenchen   3 Mar 11 01:08 lib64 -> lib
52437434 drwxrwxr-x. 3 chenchen chenchen  23 Mar 11 01:08 lib
19577901 drwxrwxr-x. 5 chenchen chenchen  74 Mar 11 01:08 .
35025245 drwxrwxr-x. 2 chenchen chenchen 173 Mar 11 01:08 bin
[chenchen@localhost grpc01]$ cd bin/
[chenchen@localhost bin]$ l
total 32K
35025246 lrwxrwxrwx. 1 chenchen chenchen   38 Mar 11 01:08 python3 -> /home/chenchen/tmp/python3/bin/python3
35025247 lrwxrwxrwx. 1 chenchen chenchen    7 Mar 11 01:08 python -> python3
19577901 drwxrwxr-x. 5 chenchen chenchen   74 Mar 11 01:08 ..
34954311 -rwxrwxr-x. 1 chenchen chenchen  256 Mar 11 01:08 easy_install-3.7
34954310 -rwxrwxr-x. 1 chenchen chenchen  256 Mar 11 01:08 easy_install
35021277 -rwxrwxr-x. 1 chenchen chenchen  247 Mar 11 01:08 pip3.7
35021276 -rwxrwxr-x. 1 chenchen chenchen  247 Mar 11 01:08 pip3
35021275 -rwxrwxr-x. 1 chenchen chenchen  247 Mar 11 01:08 pip
35021280 -rw-r--r--. 1 chenchen chenchen 2.4K Mar 11 01:08 activate.fish
35021279 -rw-r--r--. 1 chenchen chenchen 1.3K Mar 11 01:08 activate.csh
35021278 -rw-r--r--. 1 chenchen chenchen 2.2K Mar 11 01:08 activate
35025245 drwxrwxr-x. 2 chenchen chenchen  173 Mar 11 01:08 .
[chenchen@localhost bin]$ ./activate
-bash: ./activate: Permission denied
[chenchen@localhost bin]$ . ./activate
(grpc01) [chenchen@localhost bin]$ pwd
/home/chenchen/tmp/venv/grpc01/bin
(grpc01) [chenchen@localhost bin]$ ./python3 -m pip list
Package    Version
---------- -------
pip        20.1.1
setuptools 47.1.0
WARNING: You are using pip version 20.1.1; however, version 22.0.4 is available.
You should consider upgrading via the '/home/chenchen/tmp/venv/grpc01/bin/python3 -m pip install --upgrade pip' command.
(grpc01) [chenchen@localhost bin]$ ./python3 -m pip install --upgrade pip
Collecting pip
  Using cached pip-22.0.4-py3-none-any.whl (2.1 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 20.1.1
    Uninstalling pip-20.1.1:
      Successfully uninstalled pip-20.1.1
Successfully installed pip-22.0.4
(grpc01) [chenchen@localhost bin]$ /home/chenchen/tmp/venv/grpc01/bin/python3 -m pip list
Package    Version
---------- -------
pip        22.0.4
setuptools 47.1.0
(grpc01) [chenchen@localhost bin]$ 
```



### pip

```shell
pip (package installer for Python) 是推荐的安装程序, 现代版本的Python附带的安装软件包最流行工具. 
它提供了从 PyPI (Python Package Index) 和其他 Python 软件包索引中查找, 下载 和 安装软件包的基本核心功能, 并且可以通过其命令行界面(CLI)集成到各种开发工作流程中.
pip 文档: https://pip.pypa.io/en/stable/

最常见用法是使用 pip 从 Python Package Index (PyPI)中进行安装.
PyPI 是 Python 社区的默认 Package Index. 它对所有Python开发人员开放以使用和分发其发行版.

一般而言, 需求说明符由项目名称和可选的版本说明符组成。 PEP 440包含当前支持的说明符的完整说明。以下是一些示例。
需求说明
pip用于从“程序包索引”安装程序包的格式。有关格式的EBNF图，请参阅setuptools文档中的pkg_resources.Requirement条目。例如，“ foo> = 1.3”是需求说明符，其中“ foo”是项目名称，而“> = 1.3”部分是版本说明符

$ python3 -m pip install "SomeProject"					// 安装最后版本
$ python3 -m pip install "SomeProject==1.4"			// 安装特定1.4版
$ python3 -m pip install "SomeProject>=1,<2"		// 安装大于等于1小于2的版本
$ python3 -m pip install "SomeProject~=1.4.2"		// 安装“兼容”版本, 意思是安装任何是==1.4.*版的均可, 大于1.4.2版也可以
```



```shell
### 首先安装 pip
# 下列情况表示已经自带了 pip:
# 从 python.org 下载的 Python2 >= 2.7.9 或者 Python3 >= 3.4, 或者 已经在 virtualenv 或者 venv 创建的虚拟环境下工作时, 表示已经在安装了 pip. 此时仅需要确认升级 pip 即可.

[chenchen@dev3_10.211.21.18 ~]$ python -m pip --version				# 查看 pip 是否已经安装, 安装了返回版本
pip 20.0.2 from /usr/lib/python2.7/site-packages/pip-20.0.2-py2.7.egg/pip (python 2.7)

# 如果是通过 Linux Package Managers 软件包管理器来安装的 Python 的话, 那么您在通过源码安装 Python 的同时已经安装了 pip. 
# 通常 pip 开发者需要对此了解更多细节. pip 开发人员无法控制Linux发行版处理pip安装的方式, 因此通常无法为相关问题提供解决方案. # 但这里 https://pip.pypa.io/en/stable/installing/ 为 pip 开发者提供了一些 Linux 开发规范的信息供参考.

# 大于等于 3.4 的 Python 可以使用内置的 esurepip 模块自引导pip. 
# 有关更多详细信息, 请参考标准库文档. 确保在安装 pip 后升级pip.
# 如果您的 Python 报告在 Debian 和 派生系统 (例如Ubuntu) 上没有名为 esurepip 的模块, 请参见使用 Linux 软件包管理器部分.

# 使用 get-pip.py 安装
# 警告: 如果您使用的是由操作系统或其他程序包管理器管理的 Python 安装, 请务必谨慎. get-pip.py 与这些工具不协调, 并且可能使您的系统处于不一致状态.
# 要手动安装 pip, 请通过以下链接安全地1下载 get-pip.py: https://bootstrap.pypa.io/get-pip.py, 
# 或者, 使用 curl: curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
# 然后在下载 get-pip.py 的文件夹中运行以下命令:
# python get-pip.py
# 如果尚未安装, 则 get-pip.py 还将安装 setuptools (https://packaging.python.org/key_projects/#setuptools)和 wheel (https://packaging.python.org/key_projects/#wheel). 必须安装 setuptools 才能安装源码发行版. 尽管都不需要安装预建 wheels (https://packaging.python.org/glossary/#term-Wheel), 但是两者都是必须的在构建 Wheel Cache (https://pip.pypa.io/en/stable/cli/pip_install/#wheel-cache)（提高安装速度）时.
# 注意: 在与 pip 相同的 python 版本上支持 get-pip.py 脚本. 对于现在不受支持的 Python2.6, 可以在此处: https://bootstrap.pypa.io/2.6/get-pip.py 使用备用脚本.

# get-pip.py 选项
python get-pip.py --no-setuptools				# 不安装 setuptools
python get-pip.py --no-wheel						# 不安装 wheel
python get-pip.py --no-index --find-links=/local/copies				# 不从网络安装, 而从本地拷贝直接安装
python get-pip.py --user								# pip 默认将 Python 包安装到系统目录 (例如: /usr/local/lib/python3.4). 这需要 root 访问权限. –-user 会在您的主目录中生成 pip 安装包, 而不需要任何特殊权限. 利用 --user 参数, 即 pip install --user package_name 这样会将 Python 程序包安装到 $HOME/.local 路径下, 其中包含三个字文件夹: bin, lib 和 share. 然后修改 .bash_profile 文件使得 $HOME/.local/bin 目录下的程序加入到环境变量中 export PATH=$HOME/.local/bin:$PATH
python get-pip.py --proxy="http://[user:passwd@]proxy.server:port"			# 支持代理安装
python get-pip.py pip==9.0.2 wheel==0.30.0 setuptools==28.8.0						# 指定各个版本
python -m pip install -U pip						# 更新

# pip 可与 CPython 版本 3.6、3.7、3.8、3.9 以及 PyPy 一起使用. 这意味着 pip 可以在每个次要版本的最新补丁程序版本上使用. 尽力而为方法支持以前的修补程序版本.
```



### pip, 安装包

```shell
/home/chenchen/program/python3/lib/python3.8/site-packages/包名
python3本体安装目录: $HOME/program/python3
python3本体安装目录/lib/python版本/site-packages/包名

$ python3 -m pip --version									# 查看 pip 是否已经安装, 安装了返回版本
$ python3 -m pip install --upgrade pip      # 升级自身
$ python3 -m pip install -U pip						  # pip 自身更新
$ python3 -m pip install 包名                # 安装
$ python3 -m pip install --user 包名         # 只为当前用户安装
$ python3 -m pip list                       # 列出所有已经安装过的包名
$ python3 -m pip show 包名                   # 列出安装过的包名的详细信息
$ python3 -m pip search 包名                 # 从远程PyPI里找包
```

```shell
[chenchen@localhost python3]$ ./bin/python3 -m pip list
Package    Version
---------- -------
pip        20.1.1
setuptools 47.1.0
WARNING: You are using pip version 20.1.1; however, version 22.0.4 is available.
You should consider upgrading via the '/home/chenchen/tmp/python3/bin/python3 -m pip install --upgrade pip' command.
[chenchen@localhost python3]$ /home/chenchen/tmp/python3/bin/python3 -m pip install --upgrade pip
Collecting pip
  Downloading pip-22.0.4-py3-none-any.whl (2.1 MB)
     |████████████████████████████████| 2.1 MB 84 kB/s 
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 20.1.1
    Uninstalling pip-20.1.1:
      Successfully uninstalled pip-20.1.1
Successfully installed pip-22.0.4
[chenchen@localhost python3]$ 

# pip 已经是最新版本了, 但 setuptools 并不是, 通过 -U 升级
[chenchen@grpc01 python39]$ ./bin/python3 -m pip install --upgrade pip
Requirement already satisfied: pip in ./lib/python3.9/site-packages (22.0.4)
[chenchen@grpc01 python39]$ ./bin/python3 -m pip install -U setuptools
Requirement already satisfied: setuptools in ./lib/python3.9/site-packages (58.1.0)
Collecting setuptools
  Downloading setuptools-60.9.3-py3-none-any.whl (1.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.1/1.1 MB 1.5 MB/s eta 0:00:00
Installing collected packages: setuptools
  Attempting uninstall: setuptools
    Found existing installation: setuptools 58.1.0
    Uninstalling setuptools-58.1.0:
      Successfully uninstalled setuptools-58.1.0
Successfully installed setuptools-60.9.3
[chenchen@grpc01 python39]$ ./bin/python3 -m pip list
Package    Version
---------- -------
pip        22.0.4
setuptools 60.9.3

# 同上
[chenchen@grpc01 python37]$ ./bin/python3 -m pip list
Package    Version
---------- -------
pip        22.0.4
setuptools 47.1.0
[chenchen@grpc01 python37]$ ./bin/python3 -m pip install -U setuptools
Requirement already satisfied: setuptools in ./lib/python3.7/site-packages (47.1.0)
Collecting setuptools
  Using cached setuptools-60.9.3-py3-none-any.whl (1.1 MB)
Installing collected packages: setuptools
  Attempting uninstall: setuptools
    Found existing installation: setuptools 47.1.0
    Uninstalling setuptools-47.1.0:
      Successfully uninstalled setuptools-47.1.0
Successfully installed setuptools-60.9.3
[chenchen@grpc01 python37]$ ./bin/python3 -m pip list
Package    Version
---------- -------
pip        22.0.4
setuptools 60.9.3
[chenchen@grpc01 python37]$ 
```



### 安装 gRPC

```shell
$ python -m pip install grpcio                  # 安装 gRPC
$ python -m pip install grpcio-tools            # 安装 gRPC 工具, 工具包括 protoc(protocol buffer 编译器) 和 插件(从 .proto 服务定义文件 生成 server 和 client 代码)
```

```shell
(grpc01) [chenchen@localhost bin]$ ./python3 -m pip install grpcio
Collecting grpcio
  Downloading grpcio-1.44.0-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (4.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.3/4.3 MB 156.5 kB/s eta 0:00:00
Collecting six>=1.5.2
  Downloading six-1.16.0-py2.py3-none-any.whl (11 kB)
Installing collected packages: six, grpcio
Successfully installed grpcio-1.44.0 six-1.16.0
(grpc01) [chenchen@localhost bin]$ 
(grpc01) [chenchen@localhost bin]$ ./python3 -m pip install grpcio-tools
Collecting grpcio-tools
  Downloading grpcio_tools-1.44.0-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.4/2.4 MB 216.9 kB/s eta 0:00:00
Requirement already satisfied: grpcio>=1.44.0 in /home/chenchen/tmp/venv/grpc01/lib/python3.7/site-packages (from grpcio-tools) (1.44.0)
Collecting protobuf<4.0dev,>=3.5.0.post1
  Downloading protobuf-3.19.4-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.1/1.1 MB 9.9 MB/s eta 0:00:00
Requirement already satisfied: setuptools in /home/chenchen/tmp/venv/grpc01/lib/python3.7/site-packages (from grpcio-tools) (47.1.0)
Requirement already satisfied: six>=1.5.2 in /home/chenchen/tmp/venv/grpc01/lib/python3.7/site-packages (from grpcio>=1.44.0->grpcio-tools) (1.16.0)
Installing collected packages: protobuf, grpcio-tools
Successfully installed grpcio-tools-1.44.0 protobuf-3.19.4
(grpc01) [chenchen@localhost bin]$ 
```

### 安装 TensorFlow

```shell
$ python -m pip install tensorflow                  # 安装 tensorflow
```



### 目录说明

```shell
/home/chenchen/program/python3.9.5/																			  # python 本体安装目录
/home/chenchen/program/python3.9.5/lib/python3.9(版本号)										# 标准库目录, 其中含有 “site-packages” 目录(第三方软件包安装目录); 如果是 venv 虚拟环境下, 标准库目录下只有 “site-packages” 目录, 无标准库内容(显然共用虚拟环境的“父”环境标准库)
/home/chenchen/program/venv/xxx.01.py3.9.5/lib/python3.9/site-packages		# 第3方软件包安装目录(此例为虚拟环境下的情况)
```



### 关键术语

```shell
. 从 Python3.4 开始, pip 是首选项的安装程序, 默认包含在 Python 二进制安装器中.
. virtual environment 是个半隔离的 Python 环境, 在这个环境下允许安装软件包以供特定应用程序使用, 而不是在系统范围内安装软件包.
. venv 是用于创建虚拟环境的标准工具, 从 Python3.3 起成为 Python 的一部分. 从 Python3.4 开始, 默认情况下是将 pip 安装到所有被创建的虚拟环境中.
. virtualenv 是 venv 的第三方替代产品(也是其前身), 它允许在 Python3.4 之前的环境上使用虚拟环境, 这些版本要么根本不提供 venv, 要么无法在创建的虚拟环境里自动安装 pip.
. PyPI (Python Packaging Index) 是个开源代码的公共存储库.
. pip (package installer for Python) 是推荐的安装程序, 现代版本的Python附带的安装软件包最流行工具.
. Python Packaging Authority 是由开发人员和文档作者组成的小组, 负责标准包装工具以及相关的元数据和文件格式标准的维护和开发. 他们在 GitHub 和 Bitucket 上维护各种工具, 文档和问题跟踪器.
. distutils 是最初于 1998 年添加到 Python 标准库中的原始构建和分发系统. distutils 的直接使用正在逐步淘汰, 他仍然位当前的包装和发行奠定了基础架构, 它不仅是标准库的一部分, 而且其名称还可以通过其他方式(例如用于协调 Python 打包标准开发的邮件列表名称).
. CPython 是特指 C 语言实现的 Python, 就是原汁原味的 Python. 当我们从Python官方网站下载并安装好Python 3.5后, 我们就直接获得了一个官方版本的解释器: CPython. 这个解释器是用 C 语言开发的, 所以叫 CPython. 在命令行下运行 python 就是启动 CPython 解释器. 之所以使用 CPython 这个词, 是因为 Python 还有一些其它的实现, 比如 Jython, 就是 Java 版的 Python, 还有烧脑的 PyPy, 使用 Python 再把 Python 实现了一遍.
. IPython, IPython 是基于 CPython 之上的一个交互式解释器, 也就是说, IPython只是在交互方式上有所增强, 但是执行Python代码的功能和CPython是完全一样的. 好比很多国产浏览器虽然外观不同, 但内核其实都是调用了IE. CPython 用 >>> 作为提示符, 而 IPython 用In [序号]: 作为提示符.
. PyPy, PyPy 是另一个 Python 解释器, 它的目标是执行速度. PyPy 采用 JIT 技术, 对 Python 代码进行动态编译(注意不是解释), 所以可以显著提高 Python 代码的执行速度. 绝大部分 Python 代码都可以在 PyPy 下运行, 但是 PyPy 和 CPython 有一些是不同的, 这就导致相同的 Python 代码在两种解释器下执行可能会有不同的结果. 如果你的代码要放到 PyPy 下执行, 就需要了解 PyPy 和 CPython 的不同点.
. Jython, Jython 是运行在 Java 平台上的 Python 解释器, 可以直接把 Python 代码编译成 Java 字节码执行.
. IronPython, IronPython 和 Jython 类似, 只不过 IronPython 是运行在微软 .Net 平台上的 Python 解释器, 可以直接把 Python 代码编译成 .Net 的字节码.


在版本 3.5 中进行了更改: 现在建议使用 venv 创建虚拟环境.
参考: Python Packaging User Guide: Creating and using virtual environments.
https://packaging.python.org/tutorials/installing-packages/#creating-virtual-environments
```



### python 问题

> 参考 FAQ



### FAQ

```shell
问题:
./python3: error while loading shared libraries: libpython3.8.so.1.0: cannot open shared object file: No such file or directory
原因:
是 CentOS 系统默认加载 /usr/lib, /lib 下面库文件, python 默认安装到非此类文件夹. 不过可以通过添加库配置信息
解决:
方法1: (采用的, 行之有效的)
cd  /etc/ld.so.conf.d
->sudo vim python3.8.conf
->编辑 添加库文件路径 /home/chenchen/program/ptyon3/lib
->退出保存
->运行 sudo ldconfig -vvv
方法2:
设置环境变量：(试验了, 不起作用)
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/python3/bin
注：其中/usr/local/python3/为python3安装的目录
方法3: (最不可取, 没有实验, 以前貌似都是这么干)
root@localhost lib]# cd /usr/local/bin/python3/lib
[root@localhost lib]# cp libpython3.7m.so.1.0 /usr/lib64
```

```shell
问题:
$ python3 -m pip install grpcio 时,
ERROR: Could not install packages due to an EnvironmentError: HTTPSConnectionPool(host='files.pythonhosted.org', port=443): Max retries exceeded with url: /packages/2a/7a/a544b27a47d9b28601c3053dba0955c94d50bc1e3d3359a86c38a3dd9094/grpcio-1.30.0-cp38-cp38-manylinux2010_x86_64.whl (Caused by NewConnectionError('<pip._vendor.urllib3.connection.VerifiedHTTPSConnection object at 0x7f1da68dcac0>: Failed to establish a new connection: [Errno 101] Network is unreachable'))
原因:
Failed to establish a new connection: [Errno 101] Network is unreachable
网络环境不佳
方法:
$ python3 -m pip install grpcio -i https://pypi.doubanio.com/simple
$ python3 -m pip install grpcio-tools -i https://pypi.tuna.tsinghua.edu.cn/simple
配置国内镜像提速, PyPi镜像地址有多种配置方式:
* 系统全局配置 - /etc/pip.conf
* 当前用户配置 - $HOME/.pip/pip.conf
* 虚拟环境配置 - $VIRTUAL_ENV/pip.conf (基本用不到)
* 临时指定镜像 - pip install -i 镜像地址 包名 (采用)
以上几种方式中, 配置文件内容一样, 如：
[global]
index-url=http://pypi.douban.com/simple
其他镜像:
* http://pypi.douban.com/simple
* https://pypi.tuna.tsinghua.edu.cn/simple
* http://mirrors.aliyun.com/pypi/simple
```

```shell
一些问题和解决办法:
问题:
u'[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:765)'),))
原因:
把报错的域名添加到信任列表中去，可以添加多个受信任的域名.
方法:
$ python3 -m pip install --trusted-host=pypi.python.org --trusted-host=pypi.org --trusted-host files.pythonhosted.org grpcio
$ python3 -m pip install --trusted-host files.pythonhosted.org grpcio

问题:
Could not install packages due to an EnvironmentError: [Errno 13] Permission denied:
原因:
权限问题
python3 要往常规的 '本体目录/lib/python版本/site-packages/' 安装包之外的目录安装, 所以提示错误.
另外安装好了以后的包也无法直接使用, 因为不在 PATH 环境变量里, 也许根据实际情况手动添加 PATH.
方法:
$ python3 -m pip install --user grpcio            # 使用 --user 参数
```

```shell
问题:
gcc -pthread -shared     -Wl,--no-as-needed -o libpython3.so -Wl,-hlibpython3.so libpython3.9.so
gcc -pthread     -Xlinker -export-dynamic -o python Programs/python.o -L. -lpython3.9 -lcrypt -lpthread -ldl  -lutil -lm   -lm 
LD_LIBRARY_PATH=/home/chenchen/tmp/Python-3.9.5 ./python -E -S -m sysconfig --generate-posix-vars ;\
if test $? -ne 0 ; then \
       echo "generate-posix-vars failed" ; \
       rm -f ./pybuilddir.txt ; \
       exit 1 ; \
fi
Could not import runpy module
Traceback (most recent call last):
  File "/home/chenchen/tmp/Python-3.9.5/Lib/runpy.py", line 15, in <module>
    import importlib.util
  File "/home/chenchen/tmp/Python-3.9.5/Lib/importlib/util.py", line 2, in <module>
    from . import abc
  File "/home/chenchen/tmp/Python-3.9.5/Lib/importlib/abc.py", line 17, in <module>
    from typing import Protocol, runtime_checkable
  File "/home/chenchen/tmp/Python-3.9.5/Lib/typing.py", line 21, in <module>
    import collections
SystemError: <built-in function compile> returned NULL without setting an error
generate-posix-vars failed
make[1]: *** [pybuilddir.txt] Error 1
make[1]: Leaving directory `/home/chenchen/tmp/Python-3.9.5'
make: *** [profile-opt] Error 2
[chenchen@localhost Python-3.9.5]$
原因:
. 在低版本的 gcc 版本中带有 --enable-optimizations 参数时会出现上面问题
. gcc 8.1.0 修复此问题
方法:
1. 升级 gcc 至 8.1.0 【不推荐】
2. ./configure 参数中去掉 --enable-optimizations
```

```shell
问题1:
Failed to build these modules:
_hashlib              _ssl 

问题2:
Following modules built successfully but were removed because they could not be imported:
_hashlib              _ssl                                     

问题3:
Could not build the ssl module!
Python requires a OpenSSL 1.1.1 or newer

问题4:
Following modules built successfully but were removed because they could not be imported:
_hashlib              _ssl                                     


Could not build the ssl module!
Python requires an OpenSSL 1.0.2 or 1.1 compatible libssl with X509_VERIFY_PARAM_set1_host().
LibreSSL 2.6.4 and earlier do not provide the necessary APIs, https://github.com/libressl-portable/portable/issues/381

问题5:
*** WARNING: renaming "_ssl" since importing it failed: build/lib.linux-x86_64-3.8/_ssl.cpython-38-x86_64-linux-gnu.so: undefined symbol: OPENSSL_sk_num
*** WARNING: renaming "_hashlib" since importing it failed: build/lib.linux-x86_64-3.8/_hashlib.cpython-38-x86_64-linux-gnu.so: undefined symbol: EVP_blake2b512

原因:
再次印证, 是 openssl 没有安装对, 或者文件有损坏. 细节文件不清, 但为了保险, 需要重新安装 openssl.
openssl 是系统自带, 应该叫做 openssl-libs, 和 openssl, openssl-devel 还不是一回事儿

# openssl-devel-1.0.2k-24.el7_9.x86_64
# openssl-1.0.2k-24.el7_9.x86_64
# openssl-libs-1.0.2k-24.el7_9.x86_64

# rpm -e openssl-devel-1.0.2k-24.el7_9.x86_64
# rpm -e openssl-1.0.2k-24.el7_9.x86_64
# rpm -e openssl-libs-1.0.2k-24.el7_9.x86_64 --nodeps --force

# sudo yum -y install openssl-libs
# sudo yum -y reinstall openssl-libs
# sudo yum -y install openssl openssl-devel
# sudo yum -y reinstall openssl openssl-devel

解决:
openssl 一共三个: openssl-devel, openssl, openssl-libs
其中 openssl-libs 是系统自带的, 无法 rpm -e xxx 卸载, 只能通过 sudo yum -y reinstall openssl-libs 来重装, 重装时一定看着 /usr/lib64 里面的文件是不是更新了, 一遍不行就两遍, 或者先卸载了 openssl 和 openssl-devel 再重装, 然后再安装 openssl 和 openssl-devel

sudo yum -y reinstall openssl-libs
sudo yum -y reinstall openssl openssl-devel

接下来就是正常安装了:
make distclean
./configure --prefix=$HOME/tmp/python3 --enable-shared --enable-big-digits  --with-system-ffi
make -j16
make install

相关:
. 动态库查看, /etc/ld.so.conf.d/ 这下面是手动添加动态库的配置, 系统默认 openssl 是不需要这个配置. 但手动安装 openssl其他版本, 或者同系统多版本共存是需要手动配置. 在配置之后还需要刷新缓存, sudo ldconfig -vvv
. 查看 openssl 命令所在的位置, $ whereis openssl
. 查看系统环境默认使用的 openssl 是那个版本, $ openssl version
. 查看 openssl 使用的对应动态库, $ ldd openssl, 必须到 openssl 二进制命令下的目录里执行
. 查看系统都安装了那些 openssl 软件包, $ rpm -qa | grep ssl
. 卸载指定全称的软件包, rpm -e openssl-devel-1.0.2k-24.el7_9.x86_64
. 如果卸载时出现依赖而禁止卸载时, 可以‘强制’或者‘禁止依赖性’, rpm -e openssl-libs-1.0.2k-24.el7_9.x86_64 --nodeps --force
. openssl-libs 包不能直接卸载, 卸载不掉, 只能 yum 重装, $ sudo yum -y reinstall openssl-libs, 重装时要观察动态库文件是否更新, 如果一遍不行就重装两遍.
. 为了保险起见, 注意重装包的顺序, 先是 openssl-libs, 然后才是 openssl 和 openssl-devel, $ sudo yum -y reinstall openssl openssl-devel
```

```shell
问题:
The following modules found by detect_modules() in setup.py, have been
built by the Makefile instead, as configured by the Setup files:
_abc                  pwd                   time 

原因:

解决:

```

```shell
问题:
ldconfig: Can't create temporary cache file /etc/ld.so.cache~: Permission denied
[chenchen@grpc01 ld.so.conf.d]$ openssl
openssl: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory
```

```shell

```

```shell
问题:
Installing collected packages: setuptools, pip
  WARNING: The script easy_install-3.7 is installed in '/home/chenchen/tmp/python3/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
  WARNING: The scripts pip3 and pip3.7 are installed in '/home/chenchen/tmp/python3/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed pip-20.1.1 setuptools-47.1.0

还没写:
原因:

解决:


```

```shell
问题:
WARNING: Value for scheme.headers does not match. Please report this to <https://github.com/pypa/pip/issues/10151>
distutils: /home/chenchen/tmp/python39/include/python3.9/UNKNOWN
sysconfig: /home/chenchen/tmp/Python-3.9.10/Include/UNKNOWN
WARNING: Additional context:
user = False
home = None
root = '/'
prefix = None
Looking in links: /tmp/tmp9c1aoon0
Processing /tmp/tmp9c1aoon0/setuptools-58.1.0-py3-none-any.whl
Processing /tmp/tmp9c1aoon0/pip-21.2.4-py3-none-any.whl
Installing collected packages: setuptools, pip
  WARNING: Value for scheme.headers does not match. Please report this to <https://github.com/pypa/pip/issues/10151>
  distutils: /home/chenchen/tmp/python39/include/python3.9/setuptools
  sysconfig: /home/chenchen/tmp/Python-3.9.10/Include/setuptools
  WARNING: Value for scheme.headers does not match. Please report this to <https://github.com/pypa/pip/issues/10151>
  distutils: /home/chenchen/tmp/python39/include/python3.9/pip
  sysconfig: /home/chenchen/tmp/Python-3.9.10/Include/pip
  WARNING: The scripts pip3 and pip3.9 are installed in '/home/chenchen/tmp/python39/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed pip-21.2.4 setuptools-58.1.0

还没写:
原因:

解决:

```



### Python 3 on CentOS 8.x

```shell
Python 3 on CentOS 8.x
Python 3 can be installed on CentOS 8.x using DNF package manager. Use the below command to start Python 3 install:

$ sudo dnf install python3
When prompted, please confirm by pressing ‘y’:

Is this ok [y/N]: y
Log snippet of Python 3 install on CentOS 8 is shown below:

$ sudo dnf install python3
Last metadata expiration check: 6:25:17 ago on Friday 11 December 2020 12:44:46 PM IST.
Package python36-3.6.8-2.module_el8.1.0+245+c39af44f.x86_64 is already installed.
Dependencies resolved.
==========================================================================================================================================================================
Package Architecture Version Repository Size
==========================================================================================================================================================================
Upgrading:
python36 x86_64 3.6.8-2.module_el8.3.0+562+e162826a AppStream 19 k

Transaction Summary
==========================================================================================================================================================================
Upgrade 1 Package

Total download size: 19 k
Is this ok [y/N]: y
Downloading Packages:
python36-3.6.8-2.module_el8.3.0+562+e162826a.x86_64.rpm 5.6 kB/s | 19 kB 00:03
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total 4.6 kB/s | 19 kB 00:04
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
Preparing : 1/1
Upgrading : python36-3.6.8-2.module_el8.3.0+562+e162826a.x86_64 1/2
Running scriptlet: python36-3.6.8-2.module_el8.3.0+562+e162826a.x86_64 1/2
Cleanup : python36-3.6.8-2.module_el8.1.0+245+c39af44f.x86_64 2/2
Running scriptlet: python36-3.6.8-2.module_el8.1.0+245+c39af44f.x86_64 2/2
Verifying : python36-3.6.8-2.module_el8.3.0+562+e162826a.x86_64 1/2
Verifying : python36-3.6.8-2.module_el8.1.0+245+c39af44f.x86_64 2/2
Installed products updated.

Upgraded:
python36-3.6.8-2.module_el8.3.0+562+e162826a.x86_64

Complete!
$

```





### See Also

https://www.python.org/downloads/source/

https://devguide.python.org/setup/#linux

https://vault.centos.org/7.7.1908/os/x86_64/Packages/

https://www.tensorflow.org/install
