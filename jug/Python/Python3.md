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
或者(采用)
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
$ sudo yum -y install libffi libffi-devel			(_ctypes)

$ sudo yum -y install bzip2 bzip2-devel			(_bz2)
$ sudo yum -y install readline readline-devel			(readline)
$ sudo yum -y install tcl tcl-devel			(_tkinter)
$ sudo yum -y install tk tk-devel			(_tkinter)
$ sudo yum -y install tkinter			(_tkinter)
$ sudo yum -y install python3-tkinter			(_tkinter)
$ sudo yum -y install libX11 libX11-devel
$ sudo yum -y install sqlite sqlite-devel			(_sqlite3)
$ sudo yum -y install xz lzma xz-devel			(_lzma)
$ sudo yum -y install uuid uuid-devel			(_uuid)
#$ sudo yum -y install libuuid libuuid-devel  (libxxx 命名的一般是 Ubantu系统)
$ sudo yum -y install zlib zlib-devel			(zlib)
$ sudo yum -y install ncurses ncurses-devel			(_curses, _curses_panel)
$ sudo yum -y install gdbm gdbm-devel			(_dbm, _gdbm)

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
./configure --prefix=$HOME/tmp/python311 --enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/usr/local/openssl111m (成功, --with-openssl=OpenSSL本体根目录)

./configure --prefix=$HOME/program/python3 --exec-prefix=$HOME/bin --enable-shared --enable-optimizations --enable-big-digits  --with-system-ffi
```



### make, make install

```shell
make distclean 	# 清除 configure
make clean
./configure ...
make
make test
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
$ sudo vim /etc/ld.so.conf.d/python39.conf		# 增加 3.9 的配置
vim 编辑添加一行 /home/chenchen/tmp/python37/lib
/home/chenchen/tmp/python39/lib		# 增加 3.9 的配置
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
[chenchen@grpc01 ~]$ python3.6 -m venv --help
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
$ python3 -m pip install --upgrade pip==20.0.2
$ python3 -m pip install -U pip						  # pip 自身更新
$ python3 -m pip install 包名                # 安装
$ python3 -m pip install --user 包名         # 只为当前用户安装
$ python3 -m pip list                       # 列出所有已经安装过的包名
$ python3 -m pip list -o										# 在安装的库中, 看哪些需要版本升级
$ python3 -m pip show 包名                   # 列出安装过的包名的详细信息
$ python3 -m pip show -f 包名                # 列出安装过的包名的详细信息和文件位置
$ python3 -m pip search 包名                 # 从远程PyPI里找包
$ pip uninstall 包名 												 # 卸载包名
$ pip install --upgrade keras==2.1.0 				# 升级到指定版本
$ pip install -U keras==2.1.0 							# 升级到指定版本
$ pip install keras==2.1.0 									# 安装指定版本
# $ python -m pip install 等于 $ pip install, 即 $ python -m pip 等于 $ pip
$ pip install "package>=0.2,<0.3"						# 版本至少为0.2且小于0.3 (必须双引号)
$ pip install -U "gast<=0.4.0,>=0.2.1"
$ pip check 																# 检查所有安装包之间的兼容性
$ pip check 包名 		 												 # 检查指定包与其他包之间的兼容性

# 
$ pip list --outdate		# 列出可升级的包
$ pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U    # 升级所有可升级的包
$ pip freeze --local | grep -v '^-e' | cut -d = -f 1  | xargs -n1 pip install -U    # 升级所有可升级的包

# 安装 pytorch, https://pytorch.org/get-started/locally/
$ pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cpu
或者
(grpc02) [chenchen@grpc01 bin]$ python -m pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu

# pip 回退版本实例
$ pip install -U "gast<=0.4.0,>=0.2.1"
$ pip show gast
$ pip list -o		# 检查回退

(grpc02) [chenchen@grpc01 bin]$ pip install -U "gast<=0.4.0,>=0.2.1"
Requirement already satisfied: gast<=0.4.0,>=0.2.1 in /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages (0.4.0)
(grpc02) [chenchen@grpc01 bin]$ pip show gast
Name: gast
Version: 0.4.0
Summary: Python AST that abstracts the underlying Python version
Home-page: https://github.com/serge-sans-paille/gast/
Author: serge-sans-paille
Author-email: serge.guelton@telecom-bretagne.eu
License: BSD 3-Clause
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: tensorflow
(grpc02) [chenchen@grpc01 bin]$ pip install -U "numpy<1.24,>=1.22"
Requirement already satisfied: numpy<1.24,>=1.22 in /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages (1.23.5)
(grpc02) [chenchen@grpc01 bin]$ pip show numpy
Name: numpy
Version: 1.23.5
Summary: NumPy is the fundamental package for array computing with Python.
Home-page: https://www.numpy.org
Author: Travis E. Oliphant et al.
Author-email: 
License: BSD
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: h5py, jax, ml-dtypes, opt-einsum, scipy, tensorboard, tensorflow, torchvision
(grpc02) [chenchen@grpc01 bin]$ 
(grpc02) [chenchen@grpc01 bin]$ pip list -o
Package     Version Latest Type
----------- ------- ------ -----
gast        0.4.0   0.5.4  wheel
numpy       1.23.5  1.24.3 wheel
setuptools  58.1.0  67.8.0 wheel
tensorboard 2.12.3  2.13.0 wheel
urllib3     1.26.15 2.0.2  wheel
wheel       0.38.4  0.40.0 wheel
wrapt       1.14.1  1.15.0 wheel
```

```shell
# pip 包兼容性处理办法
$ pip check 				# 检查所有安装包之间的兼容性
$ pip check 包名 		 # 检查指定包与其他包之间的兼容性

(grpc02) [chenchen@grpc01 bin]$ pip check
tensorflow 2.12.0 has requirement gast<=0.4.0,>=0.2.1, but you have gast 0.5.4.
(grpc02) [chenchen@grpc01 bin]$ pip check gast
tensorflow 2.12.0 has requirement gast<=0.4.0,>=0.2.1, but you have gast 0.5.4.


# 出现兼容性后, 根据提示降级(也就是升级到低版本)
$ pip check
$ pip show gast
$ pip install -U "gast<=0.4.0,>=0.2.1"
$ pip show gast
$ pip check

(grpc02) [chenchen@grpc01 bin]$ pip show gast
Name: gast
Version: 0.5.4
Summary: Python AST that abstracts the underlying Python version
Home-page: https://github.com/serge-sans-paille/gast/
Author: serge-sans-paille
Author-email: serge.guelton@telecom-bretagne.eu
License: BSD 3-Clause
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: tensorflow
(grpc02) [chenchen@grpc01 bin]$ pip install -U "gast<=0.4.0,>=0.2.1"
Collecting gast<=0.4.0,>=0.2.1
  Using cached gast-0.4.0-py3-none-any.whl (9.8 kB)
Installing collected packages: gast
  Attempting uninstall: gast
    Found existing installation: gast 0.5.4
    Uninstalling gast-0.5.4:
      Successfully uninstalled gast-0.5.4
Successfully installed gast-0.4.0
(grpc02) [chenchen@grpc01 bin]$ pip show gast
Name: gast
Version: 0.4.0
Summary: Python AST that abstracts the underlying Python version
Home-page: https://github.com/serge-sans-paille/gast/
Author: serge-sans-paille
Author-email: serge.guelton@telecom-bretagne.eu
License: BSD 3-Clause
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: tensorflow
(grpc02) [chenchen@grpc01 bin]$ pip check
No broken requirements found.

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

### pip 清理安装包后的缓存 ~/.cache/pip

```python
# 上述命令可以清空 pip 缓存, 包括 pip 下载的所有包, 解压后的内容以及源代码. 
# 此外, pip 还会删除所有的安装历史和日志文件.

(nano) [chenchen@dev3_10.211.21.18 ~]$ pip cache purge
Files removed: 290
(nano) [chenchen@dev3_10.211.21.18 ~]$ du -sh .[!.]* * | sort -hr
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
detect_modules() 在 setup.py 中找到的以下模块是由 Makefile 构建的, 由安装程序文件配置：
The following modules found by detect_modules() in setup.py, have been
built by the Makefile instead, as configured by the Setup files:
_abc                  pwd                   time 

原因:
我理解不是错误, 是提示说明. 

解决:
不用管
```

```shell
问题:
ldconfig: Can't create temporary cache file /etc/ld.so.cache~: Permission denied
[chenchen@grpc01 ld.so.conf.d]$ openssl
openssl: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory
```

```shell
问题:
The necessary bits to build these optional modules were not found:
_tkinter                                                       
To find the necessary bits, look in setup.py in detect_modules() for the module's name.

原因:
tkinter 简介
tkinter 是 python 的一个模块, 封装了 Tcl/Tk 接口, Tcl/Tk 是跨平台脚本图形界面接口. tk 的底层 C 语言接口在动态链接库_tkinter 中.
估计很多图线需要此库支持.

setup.py 源码, 没找到这2个文件所以报错:
    def detect_tkinter(self):
        self.addext(Extension('_tkinter', ['_tkinter.c', 'tkappinit.c']))

解决: 
先不安装 python 3.11 了, 先用 python 3.9 吧
从 python3.10 开始, tkinter 需要8.6 版本, 而 contos7 的 yum 能安装到的最新版本是 8.5. 因此不能用python 3.10, 只能到 python 3.9.

依据参考文档: 
https://docs.python.org/3.9/library/tkinter.html python3.9
https://docs.python.org/3.10/library/tkinter.html python3.10
https://docs.python.org/3.11/library/tkinter.html python3.11
TK 参考:
https://tkdocs.com/tutorial/install.html
https://www.tcl.tk/doc/howto/compile.html
https://ksvi.mff.cuni.cz/~dingle/2021-2/prog_1/tkinter_reference.html#introduction|outline
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

```shell
问题:
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
grpcio-tools 1.51.1 requires protobuf<5.0dev,>=4.21.6, but you have protobuf 3.19.6 which is incompatible.

原因:
是在 安装 tensorflow 时报错
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip install tensorflow

解决:
需要将 protobuf 包的版本控制在 <5.0dev, >=4.21.6
先看看现在是什么版本. pip show protobuf
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip show protobuf
Name: protobuf
Version: 3.19.6
Summary: Protocol Buffers
Home-page: https://developers.google.com/protocol-buffers/
Author: 
Author-email: 
License: 3-Clause BSD License
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: grpcio-tools, tensorboard, tensorflow

包参考: https://pypi.org/project/protobuf/#history, 从这里找特定包的特定版本. 好搜完之后点进去
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

### Python-3.11.1 源码安装过程

```shell
1. 下载, 通过 clash 翻墙工具下载, 参考: Clash.md
[chenchen@grpc01 ~]$ wget 'https://www.python.org/ftp/python/3.11.1/Python-3.11.1.tgz'
--2023-01-19 06:57:28--  https://www.python.org/ftp/python/3.11.1/Python-3.11.1.tgz
Connecting to 127.0.0.1:7890... connected.
Proxy request sent, awaiting response... 200 OK
Length: 26394378 (25M) [application/octet-stream]
Saving to: ‘Python-3.11.1.tgz’

100%[========================================================================================================================================================================================>] 26,394,378  28.6KB/s   in 19m 24s

2023-01-19 07:16:53 (22.2 KB/s) - ‘Python-3.11.1.tgz’ saved [26394378/26394378]

[chenchen@grpc01 ~]$ 

2. md5sum 校验, https://www.python.org/downloads/release/python-3111/
[chenchen@grpc01 tmp]$ md5sum Python-3.11.1.tgz 
5c986b2865979b393aa50a31c65b64e8  Python-3.11.1.tgz
[chenchen@grpc01 tmp]$ 

3. 解压缩
[chenchen@grpc01 tmp]$ tar zxvf Python-3.11.1.tgz -C ./
134289413 drwxr-xr-x. 16 chenchen chenchen 4.0K Dec  7 03:20 Python-3.11.1
  2453768 -rw-rw-r--.  1 chenchen chenchen  26M Dec  7 03:26 Python-3.11.1.tgz

4. 检查/查询已安装软件和依赖
上面有需要事先安装的软件库和依赖库
[chenchen@grpc01 tmp]$ yum list installed
[chenchen@grpc01 tmp]$ rpm -qa

5. ./configure
[chenchen@grpc01 tmp]$ mkdir python311
[chenchen@grpc01 tmp]$ cd Python-3.11.1
./configure --prefix=$HOME/tmp/python311 --enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/usr/local/openssl111m (成功, --with-openssl=OpenSSL本体根目录, 执行大约1分钟)
./configure --prefix=/local/usr/local/python3 --enable-shared --with-ensurepip=install --enable-optimizations
./configure --prefix=/usr        \
            --enable-shared      \
            --with-system-expat  \
            --with-system-ffi    \
            --enable-optimizations &&
            
yum update
yum install openssl-devel bzip2-devel libffi-devel

6. make, make install
make distclean 	# 清除 configure
make clean 			# 同上
./configure ...
make
make -j4
make test
make install
(VirtualBox 上需要30分钟左右)
# make -j 带一个参数, 可以把项目在进行并行编译, 比如在一台双核的机器上, 完全可以用 make -j4, 让 make 最多允许4个编译命令同时执行, 这样可以更有效的利用 CPU 资源. 但并行的任务不宜太多, 一般是以CPU的核心数目的两倍为宜.

7. 创建软连接在 PATH 中, 方便使用
[chenchen@grpc01 bin]$ pwd
/usr/bin
[chenchen@grpc01 bin]$ sudo ln -s ~/tmp/python39/bin/python3.9 python3.9

8. 查看版本和模块
[chenchen@grpc01 bin]$ python3.9 -V
Python 3.9.10
[chenchen@grpc01 bin]$ python3.6 -V
Python 3.6.8
[chenchen@grpc01 ld.so.conf.d]$ python3.7 -m pydoc modules
[chenchen@grpc01 ld.so.conf.d]$ python3.9 -m pydoc modules
[chenchen@grpc01 bin]$ python3.7 -m pydoc modules | wc -l
/home/chenchen/tmp/python37/lib/python3.7/site-packages/_distutils_hack/__init__.py:30: UserWarning: Setuptools is replacing distutils.
  warnings.warn("Setuptools is replacing distutils.")
88
[chenchen@grpc01 ld.so.conf.d]$ python3.9 -m pydoc modules | wc -l
/home/chenchen/tmp/python39/lib/python3.9/site-packages/_distutils_hack/__init__.py:30: UserWarning: Setuptools is replacing distutils.
  warnings.warn("Setuptools is replacing distutils.")
86
[chenchen@grpc01 bin]$ python3 -m pydoc modules | wc -l
80

[chenchen@grpc01 ~]$ python3
Python 3.6.8 (default, Nov 16 2020, 16:55:22) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.modules.keys()
dict_keys(['builtins', 'sys', '_frozen_importlib', '_imp', '_warnings', '_thread', '_weakref', '_frozen_importlib_external', '_io', 'marshal', 'posix', 'zipimport', 'encodings', 'codecs', '_codecs', 'encodings.aliases', 'encodings.utf_8', '_signal', '__main__', 'encodings.latin_1', 'io', 'abc', '_weakrefset', 'site', 'os', 'errno', 'stat', '_stat', 'posixpath', 'genericpath', 'os.path', '_collections_abc', '_sitebuiltins', 'sysconfig', '_sysconfigdata_m_linux_x86_64-linux-gnu', 'readline', 'atexit', 'rlcompleter'])
>>> len(sys.modules)
38
[chenchen@grpc01 ~]$ python3.7
Python 3.7.12 (default, Mar 12 2022, 16:23:39) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.modules.keys()
dict_keys(['sys', 'builtins', '_frozen_importlib', '_imp', '_thread', '_warnings', '_weakref', 'zipimport', '_frozen_importlib_external', '_io', 'marshal', 'posix', 'encodings', 'codecs', '_codecs', 'encodings.aliases', 'encodings.utf_8', '_signal', '__main__', 'encodings.latin_1', 'io', 'abc', '_abc', 'site', 'os', 'stat', '_stat', '_collections_abc', 'posixpath', 'genericpath', 'os.path', '_sitebuiltins', '_bootlocale', '_locale', '_distutils_hack', 'readline', 'atexit', 'rlcompleter'])
>>> len(sys.modules)
38
[chenchen@grpc01 ~]$ python3.9
Python 3.9.10 (main, Mar 12 2022, 17:09:05) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.modules.keys()
dict_keys(['sys', 'builtins', '_frozen_importlib', '_imp', '_thread', '_warnings', '_weakref', '_io', 'marshal', 'posix', '_frozen_importlib_external', 'time', 'zipimport', '_codecs', 'codecs', 'encodings.aliases', 'encodings', 'encodings.utf_8', '_signal', 'encodings.latin_1', '_abc', 'abc', 'io', '__main__', '_stat', 'stat', '_collections_abc', 'genericpath', 'posixpath', 'os.path', 'os', '_sitebuiltins', '_locale', '_bootlocale', '_distutils_hack', 'site', 'readline', 'atexit', 'rlcompleter'])
>>> len(sys.modules)
39

9. 虚拟环境
# 创建虚拟环境
[chenchen@grpc01 python39]$ python3.9 -m venv ./grpc02
# 激活, 本质是执行 activate 脚本, 启动了一个 shell 环境. 一旦激活进入虚拟环境, 那么连 python 解释器也应该用虚拟环境下的 ./bin/python3.9
[chenchen@grpc01 grpc02]$ . ./bin/activate
# 撤销, 是新 shell 环境下的方法, 退出这个新 shell 环境
[chenchen@grpc01 grpc02]$ deactivate
# pip, 查看 pip 是否已经安装, 有没有 pip
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip -V
pip 22.3.1 from /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/pip (python 3.9)
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip --version
pip 22.3.1 from /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/pip (python 3.9)
# pip, 查看已经安装过的模块, list
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip list
Package    Version
---------- -------
pip        21.2.4
setuptools 58.1.0
WARNING: You are using pip version 21.2.4; however, version 22.3.1 is available.
You should consider upgrading via the '/home/chenchen/tmp/venv/python39/grpc02/bin/python3.9 -m pip install --upgrade pip' command.
(grpc02) [chenchen@grpc01 grpc02]$
# pip, 升级 pip 本身, -U 同 --upgrade
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip install --upgrade pip
Requirement already satisfied: pip in ./lib/python3.9/site-packages (21.2.4)
Collecting pip
  Downloading pip-22.3.1-py3-none-any.whl (2.1 MB)
     |████████████████████████████████| 2.1 MB 237 kB/s 
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 21.2.4
    Uninstalling pip-21.2.4:
      Successfully uninstalled pip-21.2.4
Successfully installed pip-22.3.1
# pip, 再次查看 pip 模块, 升级提示消失
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip list
Package    Version
---------- -------
pip        22.3.1
setuptools 58.1.0
# pip, 查看安装过的包名的详细信息
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip show pip
Name: pip
Version: 22.3.1
Summary: The PyPA recommended tool for installing Python packages.
Home-page: https://pip.pypa.io/
Author: The pip developers
Author-email: distutils-sig@python.org
License: MIT
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: 
# pip, 从远程PyPI里找包, 不再支持 pip search, 而是通过 浏览器在 https://pypi.org/search/ 页面上搜索.
# 包参考: https://pypi.org/project/protobuf/#history, 从这里找特定包的特定版本. 好搜完之后点进去
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip search pip  # 已经废弃, 不能使用了
# pip inspect, 试验性的
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip inspect ./
WARNING: pip inspect is currently an experimental command. The output format may change in a future release without prior warning.
{
  "version": "0",
  "pip_version": "22.3.1",
  "installed": [
# pip, 安装 gRPC
# pip 安装 gRPC 工具, 工具包括 protoc(protocol buffer 编译器) 和 插件(从 .proto 服务定义文件 生成 server 和 client 代码)
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip install grpcio
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip install grpcio-tools
# pip, 安装 tensorflow 有错误
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip install tensorflow
# pip, 升级 tensorflow
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip install -U tensorflow
# pip, 二次安装 tensorflow, 无错误
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip install tensorflow
# 这种报错仅仅是提示, 已经自动修复版本范围了, 如果紧接着二次执行 pip install tensorflow 的话就不报错了.
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
错误：pip 的依赖项解析器当前未考虑已安装的所有软件包。此行为是以下依赖项冲突的根源。 grpcio-tools 1.51.1 需要 protobuf<5.0dev，>=4.21.6，但你有 protobuf 3.19.6 不兼容。
grpcio-tools 1.51.1 requires protobuf<5.0dev,>=4.21.6, but you have protobuf 3.19.6 which is incompatible.
# pip, 查看 protobuf 包的详细信息
(grpc02) [chenchen@grpc01 grpc02]$ ./bin/python3.9 -m pip show protobuf
Name: protobuf
Version: 3.19.6
Summary: Protocol Buffers
Home-page: https://developers.google.com/protocol-buffers/
Author: 
Author-email: 
License: 3-Clause BSD License
Location: /home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages
Requires: 
Required-by: grpcio-tools, tensorboard, tensorflow
# venv, 虚拟环境目录结构
bin 虚拟环境下的 python 解释器和模块的二进制命令.
lib 解释器版本, 比如, python3.9, 然后是 site-packages(固定的), 然后是各种包和模块目录和文件.
lib64 lib 的软连接, 同上
include 空目录
pyvenv.cfg 虚拟环境配置文件, 虚拟环境的主环境命令python解释器位置和python解释器版本, 虚拟环境目录位置.
```

```shell
urllib, urllib2, urllib3
https://blog.csdn.net/jiduochou963/article/details/87564467
https://developer.aliyun.com/article/796828
https://zhuanlan.zhihu.com/p/92847111
```

### 选项

```sh

./configure --prefix=$HOME/tmp/python311 --enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/usr/local/openssl111m (成功, --with-openssl=OpenSSL本体根目录, 执行大约1分钟)

--enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/usr/local/openssl111m

# --enable-shared
即是说，在大多数 Unix 系统上（除了 Mac OS X 之外），共享库的路径不是绝对路径。因此，如果我们在非标准位置安装 Python，为了不和相同版本的系统 Python 产生干扰，我们需要配置非标准位置安装的 Python 共享库的路径，或者通过设置运行时的环境变量，如 LD_LIBRARY_PATH。 为了避免这个问题，我们最好避免使用 `--enable-shared`。
From Python Issue27685, Issue 27685: altinstall with --enable-shared showing incorrect behaviour, https://link.zhihu.com/?target=https%3A//bugs.python.org/issue27685

如果你要编译一个库的源代码，可以把它编译链接成静态库，也可以把它编译链接成动态库。
如果你想编译链接成动态库，就用 --enable-shared 参数；如果你想编译链接成静态库，就用 --enable-static 参数。

--enable-shared disable/enable building shared python library
关闭/开启 将要构建 共享 python 库

# --with-system-ffi
--with-system-ffi build _ctypes module using an installed ffi library
构建 _ctypes 模块时使用的一个已经安装过的 ffi 库

# --with-openssl=/usr/local/openssl111m
--with-openssl=DIR root of the OpenSSL directory
OpenSSL的根目录

# ./configure
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ pwd
/usr/home/chenchen/htdocs/python3916/Python-3.9.16
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ ./configure --prefix=$HOME/htdocs/python3916 --enable-shared --enable-big-digits  --with-system-ffi --with-openssl=/etc/pki/tls
```

### OpenSSL

```sh
https://pkgs.org/download/openssl
openssl-1.0.2k-24.el7_9.x86_64	(openssl 软件本身)
openssl-libs-1.0.2k-24.el7_9.x86_64	(依赖包)
openssl-devel-1.0.2k-24.el7_9.x86_64	(依赖包)
因为 openssl 依赖 openssl-devel 和 openssl-libs 两个包, 所以也要下载相应的包。

# 验证
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ openssl version
OpenSSL 1.0.2k-fips  26 Jan 2017

# 查看当前 openssl 位置
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ which openssl
/bin/openssl
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ whereis openssl
openssl: /usr/bin/openssl /usr/lib64/openssl /usr/include/openssl /usr/share/man/man1/openssl.1ssl.gz

[chenchen@dev3_10.211.21.18 Python-3.9.16]$ l /bin/openssl
137750 -rwxr-xr-x 1 root root 543K Jan 18  2022 /bin/openssl
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ l /usr/bin/openssl
137750 -rwxr-xr-x 1 root root 543K Jan 18  2022 /usr/bin/openssl
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ l /usr/lib64/openssl
total 76K
662286 drwxr-xr-x.  3 root root 4.0K Jan 18  2022 .
662287 drwxr-xr-x.  2 root root 4.0K Jun 15  2022 engines
656641 dr-xr-xr-x. 90 root root  64K Jul 28  2022 ..
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ l /usr/include/openssl
total 1.9M
184770 -rw-r--r--   1 root root 144K Jan 18  2022 ssl.h
...

[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep openssl
openssl-libs-1.0.2k-24.el7_9.x86_64
openssl098e-0.9.8e-29.el7.centos.3.x86_64
openssl-1.0.2k-24.el7_9.x86_64
openssl-devel-1.0.2k-24.el7_9.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -ql openssl-1.0.2k-24.el7_9.x86_64
/etc/pki/CA
/usr/bin/openssl
...

# 涉及的目录
openssl:
/etc/pik/
/usr/bin/openssl
/usr/share/

openssl-libs:
/etc/pki/
/usr/lib64/
/usr/share/

openssl-devel:
/usr/include/openssl/ 头文件
/usr/lib64/ 库
/usr/share/ 共享文档

/bin - Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令, 普通用户可以使用的命令的存放目录
/usr - unix shared resources(共享资源)的缩写, 用户的很多应用程序和文件都放在这个目录下, 类似于 windows 下的 program files 目录
/usr/bin - 系统用户使用的应用程序
/usr/share - 存放共享文件的目录, 在此目录下不同的子目录中保存了同一个操作系统在不同构架下工作时特定应用程序的共享数据(例如程序文档信息)
/usr/include - C程序语言编译使用的头文件. linux下开发和编译应用程序所需要的头文件一般都存放在这里，通过头文件来使用某些库函数. 默认来说这个路径被添加到了环境变量中, 这样编译开发程序的时候编译器会自动搜索这个路径, 从中找到你的程序中可能包含的头文件.
/usr/local - 安装本地程序的一般默认路径. 当我们下载一个程序源代码, 编译并且安装的时候, 如果不特别指定安装的程序路径, 那么默认会将程序相关的文件安装到这个目录的对应目录下. 例如, 安装的程序可执行文件被安装(安装实质就是复制到了/usr/local/bin下面, 此程序(可执行文件所需要依赖的库文件被安装到了 /usr/local/lib 目录下, 被安装的软件如果是某个开发库(例如Qt, Gtk等那么相应的头文件可能就被安装到了 /usr/local/include 中等等. 也就是说, 这个目录存放的内容, 一般都是我们后来自己安装的软件的默认路径, 如果择了这个默认路径作为软件的安装路径, 被安装的软件的所文件都限制在这个目录中, 其中的子目录就相应于根目录的子目录.
/etc - Etcetera(等等)的缩写, 这个目录用来存放所有的系统管理所需要的配置文件和子目录
/lib - 存放基本代码库(比如c++库), 其作用类似于 Windows 里的DLL文件. 几乎所有的应用程序都需要用到这些共享库.

# /bin,/sbin与/usr/bin,/usr/sbin:
/bin - 一般存放对于用户和系统来说“必须”的程序（二进制文件）。
/sbin - 一般存放用于系统管理的“必需”的程序（二进制文件），一般普通用户不会使用，根用户使用。
/usr/bin - 一般存放的只是对用户和系统来说“不是必需的”程序（二进制文件）。
/usr/sbin - 一般存放用于系统管理的系统管理的不是必需的程序（二进制文件）。

# /lib与/usr/lib:
/lib 和 /usr/lib 的区别类似 /bin, /sbin 与 /usr/bin, /usr/sbin
/lib - 一般存放对于用户和系统来说“必须”的库（二进制文件）
/usr/lib - 一般存放的只是对用户和系统来说“不是必需的”库（二进制文件）


# 对比
[chenchen@dev3_10.211.21.18 openssl]$ ldd /usr/bin/openssl
[chenchen@dev3_10.211.21.18 openssl]$ ldd /bin/openssl
对比返回的结果, 结果是只有.so的地址不同, 其他均相同
```

###  通过 openssl 命令本身获取信息

```sh
openssl 命令 选项
[chenchen@dev3_10.211.21.18 openssl]$ openssl version
OpenSSL 1.0.2k-fips  26 Jan 2017
[chenchen@dev3_10.211.21.18 openssl]$ openssl version -
usage:version -[avbofpd]
[chenchen@dev3_10.211.21.18 openssl]$ openssl version -a
OpenSSL 1.0.2k-fips  26 Jan 2017
built on: reproducible build, date unspecified
platform: linux-x86_64
options:  bn(64,64) md2(int) rc4(16x,int) des(idx,cisc,16,int) idea(int) blowfish(idx) 
compiler: gcc -I. -I.. -I../include  -fPIC -DOPENSSL_PIC -DZLIB -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DKRB5_MIT -m64 -DL_ENDIAN -Wall -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches   -m64 -mtune=generic -Wa,--noexecstack -DPURIFY -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_MONT5 -DOPENSSL_BN_ASM_GF2m -DRC4_ASM -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DAES_ASM -DVPAES_ASM -DBSAES_ASM -DWHIRLPOOL_ASM -DGHASH_ASM -DECP_NISTZ256_ASM
OPENSSLDIR: "/etc/pki/tls"
engines:  dynamic 

OPENSSLDIR - openssl 的安装目录
通过 openssl version -a 返回的 OPENSSLDIR 获取 openssl 安装目录
```



### Python-3.9.16 源码安装过程

```sh
[chenchen@dev3_10.211.21.18 python3916]$ curl www.google.com -I
HTTP/1.1 200 OK
Transfer-Encoding: chunked
...
# https://www.python.org/downloads/release/python-3916/ 下载页面
[chenchen@dev3_10.211.21.18 python3916]$ wget https://www.python.org/ftp/python/3.9.16/Python-3.9.16.tgz
--2023-06-21 12:20:36--  https://www.python.org/ftp/python/3.9.16/Python-3.9.16.tgz
Connecting to 127.0.0.1:7890... connected.
Proxy request sent, awaiting response... 200 OK
Length: 26333525 (25M) [application/octet-stream]
Saving to: ‘Python-3.9.16.tgz’

100%[==================================================================================================================================================================================>] 26,333,525  5.86MB/s   in 4.8s   

2023-06-21 12:20:43 (5.22 MB/s) - ‘Python-3.9.16.tgz’ saved [26333525/26333525]

# 解压缩
[chenchen@dev3_10.211.21.18 python3916]$ l
total 26M
23377848 -rw-rw-r--  1 chenchen chenchen  26M Dec  7  2022 Python-3.9.16.tgz
23330989 drwxr-xr-x 81 chenchen www       12K Jun 21 11:31 ..
23396831 drwxrwxr-x  2 chenchen chenchen 4.0K Jun 21 12:20 .
[chenchen@dev3_10.211.21.18 python3916]$ md5sum Python-3.9.16.tgz
38c99c7313f416dcf3238f5cf444c6c2  Python-3.9.16.tgz
[chenchen@dev3_10.211.21.18 python3916]$ tar zxvf Python-3.9.16.tgz -C ./

# ./configure
使用 --with-openssl=/usr/ 之后 openssl 成功, 这里描述了解决和找寻的过程: 
./configure --prefix=/usr/home/chenchen/htdocs/python3916 --enable-shared --enable-big-digits --with-system-ffi --with-openssl=/usr/
参考, https://stackoverflow.com/questions/17915098/openssl-ssl-h-no-such-file-or-directory-during-installation-of-git

checking for openssl/ssl.h in /usr/... yes
checking whether compiling and linking against OpenSSL works... yes
checking for X509_VERIFY_PARAM_set1_host in libssl... yes
checking for --with-ssl-default-suites... python

# 编译
make -j4

Python build finished successfully!
The necessary bits to build these optional modules were not found:
_bz2                  _dbm                  _gdbm              
_lzma                 _sqlite3              _tkinter           
_uuid                 readline                                 
To find the necessary bits, look in setup.py in detect_modules() for the module's name.


The following modules found by detect_modules() in setup.py, have been
built by the Makefile instead, as configured by the Setup files:
_abc                  atexit                pwd                
time                                                           


Failed to build these modules:
_ctypes    


running build_scripts
creating build/scripts-3.9
copying and adjusting /data1/www/htdocs/chenchen/python3916/Python-3.9.16/Tools/scripts/pydoc3 -> build/scripts-3.9
copying and adjusting /data1/www/htdocs/chenchen/python3916/Python-3.9.16/Tools/scripts/idle3 -> build/scripts-3.9
copying and adjusting /data1/www/htdocs/chenchen/python3916/Python-3.9.16/Tools/scripts/2to3 -> build/scripts-3.9
changing mode of build/scripts-3.9/pydoc3 from 664 to 775
changing mode of build/scripts-3.9/idle3 from 664 to 775
changing mode of build/scripts-3.9/2to3 from 664 to 775
renaming build/scripts-3.9/pydoc3 to build/scripts-3.9/pydoc3.9
renaming build/scripts-3.9/idle3 to build/scripts-3.9/idle3.9
renaming build/scripts-3.9/2to3 to build/scripts-3.9/2to3-3.9

# 逐一消除编译后的模块不支持的情况
_dbm 和 _gdbm 这两个是由一个软件包完成的
# 先查一下本地是否安装过 gdbm, 一查果然安装过了, 既然安装过了为什么不行呢? 仔细一看是仅安装了 gdbm, 但没有安装 gdbm-devel, 于是开始安装 gdbm-devel. 安装时 yum 提示 critical, 估计是没有代理, 开代理后安装成功.
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep gdbm
gdbm-1.10-8.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -ql gdbm-1.10-8.el7.x86_64
/usr/bin/testgdbm
/usr/lib64/libgdbm.so.4
...
$ sudo yum -y install gdbm gdbm-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install gdbm-devel
CRITICAL:yum.cli:Config error: Error accessing file for config http://repos.sina.cn/conf/yumconf.php
[chenchen@dev3_10.211.21.18 clash]$ pwd
/usr/home/chenchen/htdocs/clash
[chenchen@dev3_10.211.21.18 clash]$ sudo ./clash2 -d ./
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install gdbm-devel
Loaded plugins: fastestmirror, langpacks
...
Installed:
  gdbm-devel.x86_64 0:1.10-8.el7                                                               
Complete!

_bz2
$ sudo yum -y install bzip2 bzip2-devel
# 只安装了 bzip2, 没有安装 bzip2-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep bzip2
bzip2-libs-1.0.6-13.el7.x86_64
bzip2-1.0.6-13.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install bzip2-devel

_tkinter 和 python3-tkinter 与 (tk, tcl) 有关, 也要装 tk, tcl
$ sudo yum -y install tkinter
# 完全没有安装 tkinter
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep tkinter
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install tkinter
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make -j4    # 还有报错, _thinter, 应该安装 python3-tkinter
# 安装 python3-tkinter
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep python3-tkinter
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install python3-tkinter
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make -j4    # 还有报错, _thinter 和 _uuid
# 安装 tcl-devel 和 tk-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep tcl
targetcli-2.1.fb41-3.el7.noarch
tcl-8.5.13-8.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep tk
tkinter-2.7.5-90.el7.x86_64
at-spi2-atk-2.14.1-1.el7.x86_64
gtk2-2.24.28-8.el7.x86_64
python3-tkinter-3.6.8-18.el7.x86_64
gtk3-3.14.13-20.el7.x86_64
tk-8.5.13-6.el7.x86_64
atk-2.14.0-1.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install tcl-devel tk-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make -j4    # _thinter 报错没有了, 仅剩 _uuid

_sqlite3
$ sudo yum -y install sqlite sqlite-devel
# 只安装了 sqlite, 没有安装 sqlite-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep sqlite
sqlite-3.7.17-8.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep sqlite-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install sqlite-devel

readline
$ sudo yum -y install readline readline-devel
# 只安装了 readline, 没有安装 readline-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep readline
readline-6.2-9.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep readline-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install readline-devel

_lzma
$ sudo yum -y install xz lzma xz-devel
# 只安装了 xz 和 lzma, 没有安装 xz-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep lzma
pyliblzma-0.5.3-11.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep xz
xz-5.2.2-1.el7.x86_64
xz-libs-5.2.2-1.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep xz-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install xz-devel

_uuid
$ sudo yum -y install uuid uuid-devel
# 只安装了 libuuid(和uuid无关), 没有安装 uuid 和 uuid-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep uuid
libuuid-2.23.2-33.el7.x86_64 ()
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep uuid-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install uuid-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep uuid # 检查一下再 
uuid-1.6.2-26.el7.x86_64
libuuid-2.23.2-33.el7.x86_64
uuid-devel-1.6.2-26.el7.x86_64
# 试着安装了 libffi-devel, 但问题没有解决, 还有错误, 当时解决了 _ctypes 问题!
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep libffi
libffi-3.0.13-18.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install libffi-devel
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make -j4  # 问题依旧, 但解决了 _ctypes
# 尝试安装 lzma
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo yum -y install lzma
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ rpm -qa | grep lzma
xz-lzma-compat-5.2.2-1.el7.x86_64
pyliblzma-0.5.3-11.el7.x86_64
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make -j4  # 问题依旧
# 尝试 make clean 后重新 ./config, 和 make -j4 之后问题解决.
# 也就是当遇到安装了包和make之后问题没有解决时, 需要重新 make clean, ./config, make -j4

# 经过以上的处理, 目前仅剩一下问题
# 这个问题我理解不需要处理, 仅仅是个说明
The following modules found by detect_modules() in setup.py, have been
built by the Makefile instead, as configured by the Setup files:
_abc                  atexit                pwd                
time    

_ctypes (与 libffi libffi-devel 有依赖关系)
# 通过 libffi-devel 安装, 解决了
$ sudo yum -y install libffi libffi-devel

# 测试
# 测试有错误, 还没有找到原因
make test
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make test
== Tests result: FAILURE ==

409 tests OK.

3 tests failed:
    test_urllib test_urllib2 test_urllib2net

13 tests skipped:
    test_devpoll test_ioctl test_kqueue test_msilib test_ossaudiodev
    test_startfile test_tix test_tk test_ttk_guionly test_winconsoleio
    test_winreg test_winsound test_zipfile64
0:06:13 load avg: 4.61
0:06:13 load avg: 4.61 Re-running failed tests in verbose mode
0:06:13 load avg: 4.61 Re-running test_urllib in verbose mode (matching: test_url_host_with_control_char_rejected, test_url_host_with_newline_header_injection_rejected)
test_url_host_with_control_char_rejected (test.test_urllib.urlopen_HttpTests) ... FAIL
test_url_host_with_newline_header_injection_rejected (test.test_urllib.urlopen_HttpTests) ... FAIL

======================================================================
FAIL: test_url_host_with_control_char_rejected (test.test_urllib.urlopen_HttpTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib.py", line 437, in test_url_host_with_control_char_rejected
    urlopen(f"http:{schemeless_url}")
AssertionError: InvalidURL not raised

======================================================================
FAIL: test_url_host_with_newline_header_injection_rejected (test.test_urllib.urlopen_HttpTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib.py", line 452, in test_url_host_with_newline_header_injection_rejected
    urlopen(f"http:{schemeless_url}")
AssertionError: InvalidURL not raised

----------------------------------------------------------------------
Ran 2 tests in 0.005s

FAILED (failures=2)
test test_urllib failed
0:06:13 load avg: 4.61 Re-running test_urllib2net in verbose mode (matching: test_redirect_url_withfrag, test_urlwithfrag)
test_redirect_url_withfrag (test.test_urllib2net.OtherNetworkTests) ... ERROR
test_urlwithfrag (test.test_urllib2net.OtherNetworkTests) ... ERROR

======================================================================
ERROR: test_redirect_url_withfrag (test.test_urllib2net.OtherNetworkTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2net.py", line 173, in test_redirect_url_withfrag
    res = urllib.request.urlopen(req)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 214, in urlopen
    return opener.open(url, data, timeout)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 523, in open
    response = meth(req, response)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 632, in http_response
    response = self.parent.error(
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 555, in error
    result = self._call_chain(*args)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 494, in _call_chain
    result = func(*args)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 747, in http_error_302
    return self.parent.open(new, timeout=req.timeout)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 523, in open
    response = meth(req, response)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 632, in http_response
    response = self.parent.error(
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 561, in error
    return self._call_chain(*args)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 494, in _call_chain
    result = func(*args)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 641, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found

======================================================================
ERROR: test_urlwithfrag (test.test_urllib2net.OtherNetworkTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2net.py", line 165, in test_urlwithfrag
    res = urllib.request.urlopen(req)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 214, in urlopen
    return opener.open(url, data, timeout)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 523, in open
    response = meth(req, response)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 632, in http_response
    response = self.parent.error(
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 561, in error
    return self._call_chain(*args)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 494, in _call_chain
    result = func(*args)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 641, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found

----------------------------------------------------------------------
Ran 2 tests in 2.843s

FAILED (errors=2)
Warning -- urllib.requests._opener was modified by test_urllib2net
  Before: None
  After:  <urllib.request.OpenerDirector object at 0x7f914ae87ac0> 
test test_urllib2net failed
0:06:16 load avg: 4.24 Re-running test_urllib2 in verbose mode (matching: test_redirect_encoding, test_redirect_encoding, test_redirect_encoding, test_redirect_encoding, test_redirect_no_path)
test_redirect_encoding (test.test_urllib2.HandlerTests) ... test_redirect_no_path (test.test_urllib2.HandlerTests) ... FAIL

======================================================================
FAIL: test_redirect_encoding (test.test_urllib2.HandlerTests) [b'/p\xc3\xa5-dansk/']
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2.py", line 1359, in test_redirect_encoding
    self.assertTrue(request.startswith(expected), repr(request))
AssertionError: False is not true : b'GET http://example.com/p%C3%A5-dansk/ HTTP/1.1\r\nAccept-Encoding: identity\r\nHost: example.com\r\nUser-Agent: Python-urllib/3.9\r\nConnection: close\r\n\r\n'

======================================================================
FAIL: test_redirect_encoding (test.test_urllib2.HandlerTests) [b'/spaced%20path/']
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2.py", line 1359, in test_redirect_encoding
    self.assertTrue(request.startswith(expected), repr(request))
AssertionError: False is not true : b'GET http://example.com/spaced%20path/ HTTP/1.1\r\nAccept-Encoding: identity\r\nHost: example.com\r\nUser-Agent: Python-urllib/3.9\r\nConnection: close\r\n\r\n'

======================================================================
FAIL: test_redirect_encoding (test.test_urllib2.HandlerTests) [b'/spaced path/']
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2.py", line 1359, in test_redirect_encoding
    self.assertTrue(request.startswith(expected), repr(request))
AssertionError: False is not true : b'GET http://example.com/spaced%20path/ HTTP/1.1\r\nAccept-Encoding: identity\r\nHost: example.com\r\nUser-Agent: Python-urllib/3.9\r\nConnection: close\r\n\r\n'

======================================================================
FAIL: test_redirect_encoding (test.test_urllib2.HandlerTests) [b'/?p\xc3\xa5-dansk']
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2.py", line 1359, in test_redirect_encoding
    self.assertTrue(request.startswith(expected), repr(request))
AssertionError: False is not true : b'GET http://example.com/?p%C3%A5-dansk HTTP/1.1\r\nAccept-Encoding: identity\r\nHost: example.com\r\nUser-Agent: Python-urllib/3.9\r\nConnection: close\r\n\r\n'

======================================================================
FAIL: test_redirect_no_path (test.test_urllib2.HandlerTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2.py", line 1323, in test_redirect_no_path
    fp = urllib.request.urlopen("http://python.org/path")
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 214, in urlopen
    return opener.open(url, data, timeout)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 517, in open
    response = self._open(req, data)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 534, in _open
    result = self._call_chain(self.handle_open, protocol, protocol +
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 494, in _call_chain
    result = func(*args)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 1375, in http_open
    return self.do_open(http.client.HTTPConnection, req)
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/urllib/request.py", line 1346, in do_open
    h.request(req.get_method(), req.selector, req.data, headers,
  File "/data1/www/htdocs/chenchen/python3916/Python-3.9.16/Lib/test/test_urllib2.py", line 1318, in request
    self.assertEqual(url, next(urls))
AssertionError: 'http://python.org/path' != '/path'
- http://python.org/path
+ /path


----------------------------------------------------------------------
Ran 2 tests in 0.014s

FAILED (failures=5)
test test_urllib2 failed
3 tests failed again:
    test_urllib test_urllib2 test_urllib2net

== Tests result: FAILURE then FAILURE ==

409 tests OK.

3 tests failed:
    test_urllib test_urllib2 test_urllib2net

13 tests skipped:
    test_devpoll test_ioctl test_kqueue test_msilib test_ossaudiodev
    test_startfile test_tix test_tk test_ttk_guionly test_winconsoleio
    test_winreg test_winsound test_zipfile64

3 re-run tests:
    test_urllib test_urllib2 test_urllib2net

Total duration: 6 min 16 sec
Tests result: FAILURE then FAILURE
make: *** [test] Error 2
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ 

# 安装
make install
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make install
Installing collected packages: setuptools, pip
  WARNING: The scripts pip3 and pip3.9 are installed in '/usr/home/chenchen/htdocs/python3916/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed pip-22.0.4 setuptools-58.1.0

# 修改 ~/.bashrc, PATH
# python python3916
export PATH=/data1/www/htdocs/chenchen/python3916/bin:$PATH

# 测试
[chenchen@dev3_10.211.21.18 bin]$ ./python3 -v
./python3: error while loading shared libraries: libpython3.9.so.1.0: cannot open shared object file: No such file or directory

# 虚拟环境
[chenchen@dev3_10.211.21.18 python3916]$ python3 -m venv /data1/www/htdocs/chenchen/python3916/venv/ai			(创建虚拟环境)
[chenchen@dev3_10.211.21.18 python3916]$ . /data1/www/htdocs/chenchen/python3916/venv/ai/bin/activate			(激活虚拟环境)
(venv) [chenchen@dev3_10.211.21.18 python3916]$ deactivate			(退出当前虚拟环境)

(venv) [chenchen@dev3_10.211.21.18 python3916]$ python3 -V
Python 3.9.16
(venv) [chenchen@dev3_10.211.21.18 python3916]$ python3 -m pip list			(列出所有pip包)
Package    Version
---------- -------
pip        22.0.4
setuptools 58.1.0
WARNING: You are using pip version 22.0.4; however, version 23.1.2 is available.
You should consider upgrading via the '/data1/www/htdocs/chenchen/python3916/venv/bin/python3 -m pip install --upgrade pip' command.


[chenchen@dev3_10.211.21.18 venv]$ python3 -m venv --help			(列出虚拟模块帮助)
[chenchen@dev3_10.211.21.18 python3916]$ python3 -m venv --clear /data1/www/htdocs/chenchen/python3916/venv			(删除指定的虚拟环境目录)

[chenchen@dev3_10.211.21.18 python3916]$ python3 -m venv /data1/www/htdocs/chenchen/python3916/venv/ai			(创建虚拟环境, 并指定目录)
[chenchen@dev3_10.211.21.18 python3916]$ . /data1/www/htdocs/chenchen/python3916/venv/ai/bin/activate			(激活指定的虚拟环境)
(ai) [chenchen@dev3_10.211.21.18 ai]$ python3 -m pip list			(列出所有pip包)
Package    Version
---------- -------
pip        22.0.4
setuptools 58.1.0
WARNING: You are using pip version 22.0.4; however, version 23.1.2 is available.
You should consider upgrading via the '/data1/www/htdocs/chenchen/python3916/venv/ai/bin/python3 -m pip install --upgrade pip' command.
(ai) [chenchen@dev3_10.211.21.18 ai]$ python3 -m pip install --upgrade pip			(升级pip自身)
Requirement already satisfied: pip in ./lib/python3.9/site-packages (22.0.4)
Collecting pip
  Downloading pip-23.1.2-py3-none-any.whl (2.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 3.6 MB/s eta 0:00:00
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 22.0.4
    Uninstalling pip-22.0.4:
      Successfully uninstalled pip-22.0.4
Successfully installed pip-23.1.2
(ai) [chenchen@dev3_10.211.21.18 ai]$ python3 -m pip list			(列出所有pip包)
Package    Version
---------- -------
pip        23.1.2
setuptools 58.1.0
(ai) [chenchen@dev3_10.211.21.18 ai]$ python3 -m pip --version			(列出pip包版本)
pip 23.1.2 from /data1/www/htdocs/chenchen/python3916/venv/ai/lib/python3.9/site-packages/pip (python 3.9)
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 --version			(列出pip包版本)
pip 23.1.2 from /data1/www/htdocs/chenchen/python3916/venv/ai/lib/python3.9/site-packages/pip (python 3.9)
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 -h			(列出pip包帮助)
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 -V			(列出pip包版本)
pip 23.1.2 from /data1/www/htdocs/chenchen/python3916/venv/ai/lib/python3.9/site-packages/pip (python 3.9)
(ai) [chenchen@dev3_10.211.21.18 ai]$ 
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cpu			(安装pytorch)
Looking in indexes: https://pypi.org/simple, https://download.pytorch.org/whl/cpu
Collecting torch
...
Installing collected packages: mpmath, urllib3, typing-extensions, sympy, pillow, numpy, networkx, MarkupSafe, idna, filelock, charset-normalizer, certifi, requests, jinja2, torch, torchvision, torchaudio
Successfully installed MarkupSafe-2.1.3 certifi-2023.5.7 charset-normalizer-3.1.0 filelock-3.12.2 idna-3.4 jinja2-3.1.2 mpmath-1.3.0 networkx-3.1 numpy-1.25.0 pillow-9.5.0 requests-2.31.0 sympy-1.12 torch-2.0.1+cpu torchaudio-2.0.2+cpu torchvision-0.15.2+cpu typing-extensions-4.6.3 urllib3-2.0.3
(ai) [chenchen@dev3_10.211.21.18 ai]$ 
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 list -h			(pip list 帮助)
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 list --help
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 list         (列出已安装的软件包, 包括可编辑的软件包)
Package            Version
------------------ ----------
certifi            2023.5.7
charset-normalizer 3.1.0
filelock           3.12.2
idna               3.4
Jinja2             3.1.2
MarkupSafe         2.1.3
mpmath             1.3.0
networkx           3.1
numpy              1.25.0
Pillow             9.5.0
pip                23.1.2
requests           2.31.0
setuptools         58.1.0
sympy              1.12
torch              2.0.1+cpu
torchaudio         2.0.2+cpu
torchvision        0.15.2+cpu
typing_extensions  4.6.3
urllib3            2.0.3
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 list -o      (列出过时的软件包)
Package    Version Latest Type
---------- ------- ------ -----
setuptools 58.1.0  68.0.0 wheel
(ai) [chenchen@dev3_10.211.21.18 ai]$ 
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 install -U setuptools			(升级 setuptools)
...
Successfully installed setuptools-68.0.0
(ai) [chenchen@dev3_10.211.21.18 ai]$ pip3 list -o			(列出过时的软件包)

# 问题:
还是 OpenSSL 的问题, 按以下方式编译的 python3 并不支持 urllib3 v2.0, 使用 urllib3 需要 OpenSSL 1.1.1+ 具体解决参看本页 FAQ
./configure --prefix=/data1/www/htdocs/chenchen/python3916 --enable-shared --enable-big-digits --with-system-ffi --with-openssl=/usr/

重新配合和编译, --with-openssl=安装目录树, 参看 OpenSSL.md 文档
./configure --prefix=/data1/www/htdocs/chenchen/python3916 --enable-shared --enable-big-digits --with-system-ffi --with-openssl=/data1/www/htdocs/chenchen/openssl111u

./configure --prefix=/data1/www/htdocs/chenchen/python3916 --enable-shared --enable-big-digits --with-system-ffi --with-openssl=/data1/www/htdocs/chenchen/openssl311				(采用)

./configure --prefix=/data1/www/htdocs/chenchen/python3916 --enable-shared --enable-big-digits --with-system-ffi --with-openssl=/data1/www/htdocs/chenchen/openssl311 --with-openssl-rpath=auto				(实验, 无效果)

[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make -j4
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ make test
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ sudo make install
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ python3 -V
[chenchen@dev3_10.211.21.18 Python-3.9.16]$ . ~/.bashrc

```

### Python3.10.11 源码安装过程

```sh
[chenchen@dev3_10.211.21.18 python31011]$ wget https://www.python.org/ftp/python/3.10.11/Python-3.10.11.tgz
[chenchen@dev3_10.211.21.18 python31011]$ md5sum Python-3.10.11.tgz
7e25e2f158b1259e271a45a249cb24bb  Python-3.10.11.tgz
[chenchen@dev3_10.211.21.18 python31011]$ tar zxvf Python-3.10.11.tgz -C ./

./configure --prefix=/data1/www/htdocs/chenchen/python31011 --enable-shared --enable-big-digits --with-system-ffi --with-openssl=/data1/www/htdocs/chenchen/openssl311

[chenchen@dev3_10.211.21.18 Python-3.10.11]$ make -j4
[chenchen@dev3_10.211.21.18 Python-3.10.11]$ make test
[chenchen@dev3_10.211.21.18 Python-3.10.11]$ vim ~/.bashrc
# python python31011
export PATH=/data1/www/htdocs/chenchen/python31011/bin:$PATH

[chenchen@dev3_10.211.21.18 Python-3.10.11]$ python3 -V
python3: error while loading shared libraries: libpython3.10.so.1.0: cannot open shared object file: No such file or directory
[chenchen@dev3_10.211.21.18 Python-3.10.11]$ cd /etc/ld.so.conf.d/
[chenchen@dev3_10.211.21.18 ld.so.conf.d]$ sudo cp python3916-htdocs-chenchen.conf python31011-htdocs-chenchen.conf
[chenchen@dev3_10.211.21.18 ld.so.conf.d]$ sudo vim python31011-htdocs-chenchen.conf
/data1/www/htdocs/chenchen/python31011/lib
[chenchen@dev3_10.211.21.18 bin]$ sudo ldconfig
[chenchen@dev3_10.211.21.18 bin]$ ldconfig -p
[chenchen@dev3_10.211.21.18 bin]$ ldd python3
        linux-vdso.so.1 =>  (0x00007ffc8a1b6000)
        libpython3.10.so.1.0 => /data1/www/htdocs/chenchen/python31011/lib/libpython3.10.so.1.0 (0x00007fed98e6c000)
[chenchen@dev3_10.211.21.18 python31011]$ python3 -V
Python 3.10.11
[chenchen@dev3_10.211.21.18 python31011]$ python3 -m venv /data1/www/htdocs/chenchen/python31011/venv/ai
[chenchen@dev3_10.211.21.18 python31011]$ . /data1/www/htdocs/chenchen/python31011/venv/ai/bin/activate
(ai) [chenchen@dev3_10.211.21.18 python31011]$ pip list -o
(ai) [chenchen@dev3_10.211.21.18 python31011]$ pip install -U pip
(ai) [chenchen@dev3_10.211.21.18 python31011]$ pip install -U setuptools
(ai) [chenchen@dev3_10.211.21.18 python31011]$ pip list -o

```

### urllib, urllib2, urllib3

```sh
# 1. urllib, urllib2
在 Python2 中主要为 urllib 和 urllib2, 在 Python3 中整合成了 urllib, 为官方内置模块, 不用安装, 可以直接使用, 和一般的导包不同, 必须按照指定方法导包.

urllib 是一个收集了多个(4个)用到 URL 的模块包:
urllib.request - 打开和读取 URL
urllib.error - 包含 urllib.request 抛出的异常
urllib.parse - 用于解析 URL
urllib.robotparser - 用于解析 robots.txt 文件
参考: https://zhuanlan.zhihu.com/p/543133759

https 用到证书使用 Python3 内置 ssl 模块, 和 urllib 一样不需要安装, 仅需要 import 导入, 并且增加一行设置证书: 
ssl._create_default_https_context = ssl._create_unverified_context			# https 证书相关

# 2. urllib3
urllib3 是独立的第三方包, 需要 pip install urllib3 来特别安装.
https 用到证书也需要安装 certifi 模块, pip install certifi
```

### urllib, ssl

```python
#! /usr/bin/env python
# -*- coding:UTF-8 -*-

import urllib.request
import ssl		# https 证书相关

ssl._create_default_https_context = ssl._create_unverified_context			# https 证书相关
response = urllib.request.urlopen('https://www.google.com')
# response = urllib.request.urlopen('https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1')
print(response.status)
print(response.getheaders())

返回:
(ai) [chenchen@dev3_10.211.21.18 aiworkspacepython31011]$ ./uull.py 
200
[('Date', 'Wed, 28 Jun 2023 04:53:35 GMT'), ('Expires', '-1'), ('Cache-Control', 'private, max-age=0'), ('Content-Type', 'text/html; charset=Big5'), ('Content-Security-Policy-Report-Only', "object-src 'none';base-uri 'self';script-src 'nonce-2bSUqXrt9s4NmVsGW8z9CQ' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp"), ('P3P', 'CP="This is not a P3P policy! See g.co/p3phelp for more info."'), ('Server', 'gws'), ('X-XSS-Protection', '0'), ('X-Frame-Options', 'SAMEORIGIN'), ('Set-Cookie', '1P_JAR=2023-06-28-04; expires=Fri, 28-Jul-2023 04:53:35 GMT; path=/; domain=.google.com.hk; Secure'), ('Set-Cookie', 'AEC=AUEFqZfawef03ihejR2o0vy6lI89Q9ggKEYg-o5yRrzXqDAWH7HuT-X1HQ; expires=Mon, 25-Dec-2023 04:53:35 GMT; path=/; domain=.google.com.hk; Secure; HttpOnly; SameSite=lax'), ('Set-Cookie', 'NID=511=eloluMnzGDg65zmKh488BMGX2C0Fs2z41IF5C43wQLSEGfZE-BRfIjKvnAiV3Qj25Ld8mYBceeQNOjWACe3qKsiW1uzgqddotjbxDoghzhctHsxWWeCIeAtqQ40FCzOV8hXec6BhOPyqaQI3R-6A1WN4u1jL_ZJtWt5qBN2w50U; expires=Thu, 28-Dec-2023 04:53:35 GMT; path=/; domain=.google.com.hk; HttpOnly'), ('Alt-Svc', 'h3=":443"; ma=2592000,h3-29=":443"; ma=2592000'), ('Accept-Ranges', 'none'), ('Vary', 'Accept-Encoding'), ('Connection', 'close'), ('Transfer-Encoding', 'chunked')]

```

### urllib3, ssl

要记住的一件事是，urllib3的HTTPConnectionPool旨在成为特定主机的“连接池”，而不是有状态客户端。在这种情况下，将 cookie 的跟踪保持在实际池之外是有意义的。

```python
#! /usr/bin/env python
# -*- coding:UTF-8 -*-

# https://urllib3.readthedocs.io/en/latest/user-guide.html

import ssl
import certifi
import urllib3
import json
import time
import random


def main():
    print(certifi.where())
    cdt()

def req():
    url = 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1'
    resp = urllib3.request("GET", "https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1")
    resp.status
    200
    resp.data
    b"User-agent: *\nDisallow: /deny\n"

def cdt():
    # Creating a PoolManager instance for sending requests.
    # 两种解决证书方式, 1. CERT_NONE; 2. CERT_REQUIRED; 方式2更好(采用), 方式1是忽略证书
    # http = urllib3.PoolManager(cert_reqs = "CERT_NONE")               # 方式1, 会报 warnings
   
    http = urllib3.PoolManager(                                                                 # 方式2
        cert_reqs="CERT_REQUIRED",
        ca_certs=certifi.where()
    )

    # Sending a GET request and getting back response as HTTPResponse object.
    url = 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1'
    resp = http.request('GET', url, timeout=urllib3.Timeout(connect = 1.0, read = 2.0))
    # resp = http.request('GET', url, context=ssl.create_default_context(cafile=certifi.where()))
    # resp = http.request('GET', url, verify=False)
    # resp = http.request('GET', url, verify='/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem')

    # Print the returned data.
    print(resp.data)
    # b"User-agent: *\nDisallow: /deny\n"

if __name__ == '__main__':
    main()
```

有关链接池说明: https://stackabuse.com/guide-to-sending-http-requests-in-python-with-urllib3/

### pkg-config, LD_LIBRARY_PATH

```sh
pkg-config 是一个 Linux 下的命令, 用于获得某一个库/模块的所有编译相关的信息, 使用这个工具, 我们可以很方便地编译一个项目.

参考: https://www.cnblogs.com/yxyx213/articles/2451073.html

编译和连接
        一般来说，如果库的头文件不在 /usr/include 目录中，那么在编译的时候需要用 -I 参数指定其路径。由于同一个库在不同系统上可能位于不同的目录下，用户安装库的时候也可以将库安装在不同的目录下，所以即使使用同一个库，由于库的路径的 不同，造成了用 -I 参数指定的头文件的路径也可能不同，其结果就是造成了编译命令界面的不统一。如果使用 -L 参数，也会造成连接界面的不统一。编译和连接界面不统一会为库的使用带来麻烦。
       为了解决编译和连接界面不统一的问题，人们找到了一些解决办法。其基本思想就是：事先把库的位置信息等保存起来，需要的时候再通过特定的工具将其中有用的 信息提取出来供编译和连接使用。这样，就可以做到编译和连接界面的一致性。其中，目前最为常用的库信息提取工具就是下面介绍的 pkg-config。
pkg-config 是通过库提供的一个 .pc 文件获得库的各种必要信息的，包括版本信息、编译和连接需要的参数等。这些信息可以通过 pkg-config 提供的参数单独提取出来直接供编译器和连接器使用.

运行时
        库文件在连接（静态库和共享库）和运行（仅限于使用共享库的程序）时被使用，其搜索路径是在系统中进行设置的。一般 Linux 系统把 /lib 和 /usr/lib 两个目录作为默认的库搜索路径，所以使用这两个目录中的库时不需要进行设置搜索路径即可直接使用。对于处于默认库搜索路径之外的库，需要将库的位置添加到 库的搜索路径之中。设置库文件的搜索路径有下列两种方式，可任选其一使用：

在环境变量 LD_LIBRARY_PATH 中指明库的搜索路径。在 /etc/ld.so.conf 文件中添加库的搜索路径。         将自己可能存放库文件的路径都加入到/etc/ld.so.conf中是明智的选择 ^_^
```



### FAQ

```sh
问题:
(ai) [chenchen@dev3_10.211.21.18 ai]$ ./test.py 
Traceback (most recent call last):
  File "/data1/www/htdocs/chenchen/python3916/workspace/ai/./test.py", line 8, in <module>
    import urllib3
  File "/data1/www/htdocs/chenchen/python3916/venv/ai/lib/python3.9/site-packages/urllib3/__init__.py", line 41, in <module>
    raise ImportError(
ImportError: urllib3 v2.0 only supports OpenSSL 1.1.1+, currently the 'ssl' module is compiled with 'OpenSSL 1.0.2k-fips  26 Jan 2017'. See: https://github.com/urllib3/urllib3/issues/2168

make test 时错误:
0:00:24 load avg: 4.02 [ 72/425/1] test_urllib2net failed (2 errors)
test test_urllib2net failed -- multiple errors occurred; run in verbose mode for details

0:01:04 load avg: 4.34 [161/425/2] test_urllib failed (2 failures)
test test_urllib failed -- multiple errors occurred; run in verbose mode for details

0:04:48 load avg: 7.99 [372/425/3] test_urllib2 failed (5 failures) -- running: test_concurrent_futures (40.1 sec), test_subprocess (31.0 sec)
test test_urllib2 failed -- multiple errors occurred; run in verbose mode for details


原因:
10.211.21.18 系统上 OpenSSL 版本是 1.0.2k-fips, 太低, urllib3 v2.0 需要 OpenSSL 1.1.1+ 以上

解决:
在 /data1/www/htdocs/chenchen/ 目录下安装独立高版本 OpenSSL, 并且要保证和系统 OpenSSL 不冲突, 不影响现有系统环境.



```



### See Also

https://www.python.org/downloads/release/python-3111/	下载

https://www.python.org/downloads/source/

https://devguide.python.org/setup/#linux

https://vault.centos.org/7.7.1908/os/x86_64/Packages/

https://www.tensorflow.org/install

https://blog.csdn.net/sweetfather/article/details/79625482

https://zhuanlan.zhihu.com/p/578577985
