# Jupyter

[toc]



## Overview

### install

```sh
# Install the Jupyter Notebook using:
(nano) [chenchen@dev3_10.211.21.18 src]$ pip install jupyter
```

> http://jupyter.org/install

```sh
# 升级 pip 自身
(nano) [chenchen@dev3_10.211.21.18 src]$ pip list -o
Package   Version Latest Type
--------- ------- ------ -----
dnspython 2.3.0   2.4.0  wheel
pip       23.1.2  23.2   wheel
pymongo   4.4.0   4.4.1  wheel
urllib3   2.0.3   2.0.4  wheel

[notice] A new release of pip is available: 23.1.2 -> 23.2
[notice] To update, run: pip install --upgrade pip
(nano) [chenchen@dev3_10.211.21.18 src]$ pip install -U pip
Requirement already satisfied: pip in /data1/www/htdocs/chenchen/python31011/venv/nano/lib/python3.10/site-packages (23.1.2)
Collecting pip
  Downloading pip-23.2-py3-none-any.whl (2.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 4.2 MB/s eta 0:00:00
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 23.1.2
    Uninstalling pip-23.1.2:
      Successfully uninstalled pip-23.1.2
Successfully installed pip-23.2
(nano) [chenchen@dev3_10.211.21.18 src]$ 
```

### jupyter command

#### Synopsis

```sh
jupyter <subcommand> [options]
```

#### Description

```sh
1. jupyter notebook, 启动 Jupyter 应用, jupyter 是主命令, 也是子命令 notebook 的命名空间.
2. jupyter-foo 命令文件可以在你的 PATH 环境变量中找到, 作为一个子命令 jupyter foo. 也就是 子命令的文件叫 jupyter-foo, 变成子命令后就叫 jupyter foo, 其中 jupyter 是主命令, foo 是 jupyter-foo 子命令文件的子命令名称. 

```

#### The **jupyter** Command

```sh
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter -h
usage: jupyter [-h] [--version] [--config-dir] [--data-dir] [--runtime-dir] [--paths] [--json] [--debug] [subcommand]

Jupyter: Interactive Computing

positional arguments:
  subcommand     the subcommand to launch

options:
  -h, --help     show this help message and exit
  --version      show the versions of core jupyter packages and exit
  --config-dir   show Jupyter config dir	# 配置目录
  --data-dir     show Jupyter data dir		# 数据目录
  --runtime-dir  show Jupyter runtime dir	# 运行时目录
  --paths        show all Jupyter paths. Add --json for machine-readable format.
  							 # 所有 Jupyter 与有关的路径. 加 --json 转成机器可读格式
  --json         output paths as machine-readable json
  --debug        output debug information about paths

Available subcommands: console dejavu events execute kernel kernelspec lab labextension labhub migrate nbconvert notebook qtconsole run server troubleshoot trust
(nano) [chenchen@dev3_10.211.21.18 src]$
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --paths
config:
    /usr/home/chenchen/.jupyter
    /data1/www/htdocs/chenchen/python31011/venv/nano/etc/jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
data:
    /usr/home/chenchen/.local/share/jupyter
    /data1/www/htdocs/chenchen/python31011/venv/nano/share/jupyter
    /usr/local/share/jupyter
    /usr/share/jupyter
runtime:
    /usr/home/chenchen/.local/share/jupyter/runtime
(nano) [chenchen@dev3_10.211.21.18 src]$
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --path --json | jq
{
  "runtime": [
    "/usr/home/chenchen/.local/share/jupyter/runtime"
  ],
  "config": [
    "/usr/home/chenchen/.jupyter",
    "/data1/www/htdocs/chenchen/python31011/venv/nano/etc/jupyter",
    "/usr/local/etc/jupyter",
    "/etc/jupyter"
  ],
  "data": [
    "/usr/home/chenchen/.local/share/jupyter",
    "/data1/www/htdocs/chenchen/python31011/venv/nano/share/jupyter",
    "/usr/local/share/jupyter",
    "/usr/share/jupyter"
  ]
}
(nano) [chenchen@dev3_10.211.21.18 src]$ 
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --path --debug
JUPYTER_PLATFORM_DIRS is set to a false value, or is not set, so we use hardcoded legacy paths for platform-specific directories
JUPYTER_PREFER_ENV_PATH is set to a false value, or JUPYTER_PREFER_ENV_PATH is not set and we did not detect a virtual environment, making the user-level path preferred over the environment-level path for data and config
JUPYTER_NO_CONFIG is not set, so we use the full path list for config
JUPYTER_CONFIG_PATH is not set, so we do not prepend anything to the config paths
JUPYTER_CONFIG_DIR is not set, so we use the default user-level config directory
Python's site.ENABLE_USER_SITE is not True, so we do not add the Python site user directory '/usr/home/chenchen/.local'
JUPYTER_PATH is not set, so we do not prepend anything to the data paths
JUPYTER_DATA_DIR is not set, so we use the default user-level data directory
JUPYTER_RUNTIME_DIR is not set, so we use the default runtime directory

config:
    /usr/home/chenchen/.jupyter
    /data1/www/htdocs/chenchen/python31011/venv/nano/etc/jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
data:
    /usr/home/chenchen/.local/share/jupyter
    /data1/www/htdocs/chenchen/python31011/venv/nano/share/jupyter
    /usr/local/share/jupyter
    /usr/share/jupyter
runtime:
    /usr/home/chenchen/.local/share/jupyter/runtime
(nano) [chenchen@dev3_10.211.21.18 src]$ 
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --version
Selected Jupyter core packages...
IPython          : 8.14.0
ipykernel        : 6.24.0
ipywidgets       : 8.0.7
jupyter_client   : 8.3.0
jupyter_core     : 5.3.1
jupyter_server   : 2.7.0
jupyterlab       : 4.0.3
nbclient         : 0.8.0
nbconvert        : 7.7.2
nbformat         : 5.9.1
notebook         : 7.0.0
qtconsole        : 5.4.3
traitlets        : 5.9.0
(nano) [chenchen@dev3_10.211.21.18 src]$ 
```

#### Common Directories and File Locations

```sh
Jupyter 分为三类文件:
1. configuration - config files, custom.js
2. data files - nbextensions, kernelspecs
3. runtime files - logs, pid files, connection files
```

##### Configuration files

```sh
# 递进关系: 默认位置, JUPYTER_CONFIG_DIR, JUPYTER_CONFIG_PATH
1. jupyter 配置文件默认位置: ~/.jupyter 目录
2. 除了 1. 默认位置之外, JUPYTER_CONFIG_DIR, 环境变量, jupyter 实际配置文件目录
3. 除了 JUPYTER_CONFIG_DIR 之外, 还可以通过 JUPYTER_CONFIG_PATH 指定要搜索的其他目录, JUPYTER_CONFIG_PATH, 环境变量, 为了配置搜索路径提供额外的扩展目录, 可以包含一系列目录, 用 "os.pathsep" 分隔(在Windows上;(分号), 在Unix上:(冒号))

# 注意⚠️
可以设置 JUPYTER_CONFIG_PATH 的一个示例是 notebook 或者 server extensions 安装在自定义前缀中.
由于 notebook 或者 server extensions 是通过配置文件自动启用的, 因此仅当自定义前缀的 etc/jupyter 目录添加到 Jupyter 配置搜索路径时，自动启用才有效.

# 搜索优先级
除了上面提到的用户配置目录之外, Jupyter 还有一个搜索路径, 其中包含将从中加载配置文件的其他位置. 
下表列出了要搜索的位置, 按优先顺序排列: (1)优先 -> ... -> (4)
JUPYTER_CONFIG_DIR (1)
JUPYTER_CONFIG_PATH (2)
{sys.prefix}/etc/jupyter/ (3)
/usr/local/etc/jupyter/ /etc/jupyter/ (4)

# 查看配置文件目录
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --paths

# 查看当前起效的配置文件目录
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --config-dir
/usr/home/chenchen/.jupyter
(nano) [chenchen@dev3_10.211.21.18 src]$ 
```

##### [Data files](https://docs.jupyter.org/en/latest/use/jupyter-directories.html#data-files)

```sh
# 搜索路径
Jupyter 使用搜索路径来查找可安装的数据文件, 例如 kernelspecs 和 notebook extensions. 
搜索资源时, 代码将从第一个目录开始搜索搜索路径, 直到找到资源所在的位置.
每个类别的文件都位于搜索路径的每个目录的子目录中. 例如, kernelspecs 位于 kernels 子目录中.

# JUPYTER_PATH
JUPYTER_PATH 环境变量, 给搜索路径提供扩展目录. 包含一系列目录, 用 "os.pathsep" 分隔(在Windows上;(分号), 在Unix上:(冒号)). JUPYTER_PATH 中给出的目录优先被搜索, 优先于其他位置. 这是与其他条目一起使用的, 而不是替换任何条目.

# 搜索优先级
JUPYTER_PATH > 
JUPYTER_DATA_DIR or (if not set) ~/.local/share/jupyter/ (respects $XDG_DATA_HOME) >
{sys.prefix}/share/jupyter/ >
/usr/local/share/jupyter /usr/share/jupyter
Jupyter 数据文件的配置目录, 其中包含非瞬态、非配置文件. 示例包括 kernelspecs, nbextensions, or voila templates.

# JUPYTER_DATA_DIR
JUPYTER_DATA_DIR 环境变量, 此环境变量设置为实际使用的目录(而不是默认目录)作为用户数据目录.

# 查看配置文件目录
如上所述, 要列出当前正在使用的配置目录, 您可以从命令行运行以下命令:
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --paths

# 查看当前起效的数据文件目录
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --data-dir
/usr/home/chenchen/.local/share/jupyter
(nano) [chenchen@dev3_10.211.21.18 src]$ 
```

##### [Runtime files](https://docs.jupyter.org/en/latest/use/jupyter-directories.html#id3)

```sh
# $XDG_RUNTIME_DIR/jupyter 为默认存储位置
像连接文件这样的东西, 只对特定进程的生存期有用, 有一个运行时目录.
在 Linux 和其他免费桌面平台上, 这些运行时文件默认存储在 $XDG_RUNTIME_DIR/jupyter 中. 
在其他平台上, 它是用户数据目录的 runtime/子目录(上表的第二行)

# JUPYTER_RUNTIME_DIR 优先级更高
环境变量也可用于设置运行时目录. 设置此项以覆盖 Jupyter 存储运行时文件的位置($XDG_RUNTIME_DIR/jupyter).

# 查看配置文件目录
如上所述, 要列出当前正在使用的配置目录, 您可以从命令行运行以下命令:
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --paths

# 查看当前起效的运行时目录
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --runtime-dir
/usr/home/chenchen/.local/share/jupyter/runtime
(nano) [chenchen@dev3_10.211.21.18 src]$ 
```

##### Summary

```sh
1. 汇总
JUPYTER_CONFIG_DIR 	- 配置文件位置
JUPYTER_CONFIG_PATH - 配置文件位置
JUPYTER_PATH 				- 数据文件目录位置
JUPYTER_DATA_DIR 		- 数据文件位置
JUPYTER_RUNTIME_DIR - 运行时文件位置

2. 优先级
配置: 	JUPYTER_CONFIG_DIR > JUPYTER_CONFIG_PATH
数据: 	JUPYTER_PATH > JUPYTER_DATA_DIR
运行时: JUPYTER_RUNTIME_DIR 最高
```

### Jupyter 的通用配置方法

##### Summary

```sh
- 通用 Jupyter 配置系统: Juypter 应用有一个通用配置系统和一个通用配置目录. 默认, 目录为 ~/.jupyter
- 内核配置目录: 如果内核使用配置文件, 那么通常会为每个内核分别用目录分割开. 例如, IPython 内核在 IPython 目录中查找文件, 而不是默认的 Jupyter 目录 ~/.jupyter
```

##### [The Python config file](https://docs.jupyter.org/en/latest/use/config.html#id2)

```sh
# 创建默认配置文件
jupyter {application} --generate-config
生成的文件会被命名为 jupyter_{application}_config.py

# 编辑配置文件
jupyter_{application}_config.py, 小心拼写. 不正确的名称将被忽略, 没有错误消息. (也就是配置文件有错误也不会报错)⚠️, 形如以下: 
c.NotebookApp.port = 8754

要添加到可能已经在其他地方定义的集合中, 您可以使用类似于列表、字典和集合中的方法: append、extend、prepend()(类似于 extend, 但在前面)、add 和 update(它们适用于字典和集合):
c.TemplateExporter.template_path.append('./templates')


```

##### [Command line options for configuration](https://docs.jupyter.org/en/latest/use/config.html#id3)

```sh
也可以使用以下语法从命令行设置每个可配置值并作为参数传递:
jupyter notebook --NotebookApp.port=8754

常用选项还将具有短别名和标志, 例如: --port 8754 或 --no-browser

要查看缩写选项, 请按如下方式传递 --help 或 --help-all:
jupyter {application} --help 			# 只是短选项 
jupyter {application} --help-all 	# 包括没有短名称的选项
命令行选项将覆盖配置文件中设置的选项.⚠️
```

### JupyterLab & Jupyter Notebook

```sh
# JupyterLab 文档
JupyterLab 是一个高度可扩展、功能丰富的笔记本创作应用程序和编辑环境, 并且是 Jupyter 项目的一部分，Jupyter 项目是一个大型伞式项目，其目标是为计算笔记本的交互式计算提供工具(和标准).
JupyterLab 是 Project Jupyter 下的其他笔记本创作应用程序的兄弟, 例如 Jupyter Notebook 和 Jupyter Desktop. 与 Jupyter Notebook 相比, JupyterLab 提供了更先进、功能丰富、可定制的体验.

Classic Notebook 会发生什么变化？
随着 JupyterLab 4 开发的继续, 以及 Notebook 7 的开发, Notebook 7 最终将取代经典的 Jupyter Notebook.
在整个过渡过程中, 经典 Notebook、Notebook 7 和 JupyterLab 将支持相同的笔记本文档格式.
从 JupyterLab 4 开始, Notebook Web 应用程序将不再与 JupyterLab 一起安装, 因为它不再是 JupyterLab 的依赖项.
要了解有关生态系统中经典 Jupyter Notebook 未来的更多信息，请访问 Jupyter Notebook 7 文档.
```

#### JupyterLab

```sh
# pip install 
pip install jupyterlab
https://jupyterlab.readthedocs.io/en/latest/getting_started/installation.html#installation-problems

pip install jupyterlab-language-pack-zh-CN
多语言本地化支持, 语言包仓库: https://github.com/jupyterlab/language-packs/

# Docker
https://jupyterlab.readthedocs.io/en/latest/getting_started/installation.html#docker

# Supported browsers
Firefox
Chrome
Safari

# 启动 JupyterLab
jupyter lab

# 通过浏览器访问
http(s)://<server:port>/<lab-location>/lab
/lab 是工作空间
JupyterLab 运行在 Jupyter Server 之上.

# 提示和技巧
如何每次都以干净的工作空间启动 JupyterLab?
将 "c.ServerApp.default_url = '/lab?reset'" 添加到您的 jupyter_server_config.py. 
有关详细信息，请参阅如何创建 jupyter_server_config.py 
(https://jupyter-server.readthedocs.io/en/latest/users/configuration.html)

```

### Configuring a Jupyter Server

```sh
Jupyter Server 默认在 Jupyter 路径下查找服务器配置文件 jupyter_server_config

# 查看 jupyter 路径
$ jupyter --paths
config:
    /Users/username/.jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
data:
    /Users/username/Library/Jupyter
    /usr/local/share/jupyter
    /usr/share/jupyter
runtime:
    /Users/username/Library/Jupyter/runtime
    
config 节点下的列表带有优先顺序. 
Jupyter Server 使用的是 IPython 的 traitlets 系统配置.

$ jupyter server --generate-config
在 .jupyter 目录下创建 jupyter_server_config.py 配置文件.

# python 格式:
jupyter_server_config.py 配置文件里的 traits 都有 c.ServerApp 前缀
c.ServerApp.port = 9999

# JSON 格式:
{
    "ServerApp": {
        "port": 9999
    }
}

# 命令行 格式:
$ jupyter server --ServerApp.port=9999
每个参数都有 --ServerApp 前缀

# 启动裸露的 Jupyter 服务器
大多数情况下, 您不需要直接启动 Jupyter 服务器. 
Jupyter Web应用程序(如 Jupyter Notebook，Jupyterlab，Voila等)带有自己的入口点, 可以自动启动服务器.
但是, 有时, 当您想要同时运行多个 Jupyter Web 应用程序时, 直接启动 Jupyter Server 会很有用.
$ jupyter server

# 管理多个扩展程序
参考: https://jupyter-server.readthedocs.io/en/latest/users/index.html
Jupyter Server 的主要优点之一是, 您可以在同一个 Tornado Web server 上运行多个 Jupyter 前端应用程序.
这是因为每个 Jupyter 前端应用程序现在都是服务器扩展.
在启用多个扩展的情况下运行 Jupyter Server 时, 每个扩展都会将自己的一组处理程序和静态资产追加到服务器.

一个 Server 对应多个 extension, Jupyter applications 就是扩展

# 列出所有 Jupyter Server extension
$ jupyter server extension list
# 开启/关闭一个扩展
$ jupyter server extension enable myextension
$ jupyter server extension disable myextension
# 从 entrypoint 运行一个扩展, Jupyter applications(i.e. Notebook, JupyterLab, Voila, etc.)也是扩展
$ jupyter notebook
# 启动一个带有多个扩展的 Server, 启动 Server 时, 其下的扩展也都启动了
$ jupyter server
# 在 Server 入口点, 手动启动 Server 同时指定一个扩展
$ jupyter server --ServerApp.jpserver_extensions="myextension=True"

# 配置扩展
配置 Server 扩展就是配置 applications.
有两个途径:
1. 通过参数在扩展的入口点传进去, 就像上面
$ jupyter server --ServerApp.jpserver_extensions="myextension=True"
2. 通过配置 Jupyter 配置文件的列表配置选项
# 从配置文件配置扩展
扩展配置文件名格式: jupyter_{扩展名}_config
扩展配置文件有两种书写格式: Python 语法 或者 JSON 文件
Python 语法中, 每条属性前都有 c. 前缀, 例如: c.NotebookApp.mathjax_enabled = False
Jupyter Server 会自动加载为每个扩展加载配置文件, 所以你应该给每个扩展创建独自的配置文件
# 从命令行配置扩展
在入口点, 通过命令行可以同时配置多条属性, 例如:
$ jupyter server --ServerApp.port=9999 --MyExtension1.trait=False --MyExtension2.trait=True
$ jupyter myextension --ServerApp.port=9999 --MyExtension1.trait=False --MyExtension2.trait=True
第一行是通过 Server 入口点配置, 第二行是通过扩展入口点配置, 效果一样, 该作用于 Server 的作用于 Server, 该具体作用于某一个扩展的作用于某一个扩展.

# 运行一个公开的 Jupyter Server
以下配置均在 jupyter_server_config.py 配置文件中:
# 安全相关
ServerApp.password, (设置密码)
$ jupyter server --generate-config, (生成配置文件和配置文件的目录, aaa/jupyter_server_config.py)
首次使用令牌登录时, 服务器应让您有机会从用户界面设置密码.
您将看到一份表格, 要求提供当前令牌以及新密码; 输入两者, 然后单击登录并设置新密码.
下次需要登录时, 您将能够使用新密码而不是登录令牌, 否则请按照从命令行设置密码的过程进行操作.
集成可以通过设置 --ServerApp.allow_password_change=False 来禁用在首次登录时更改密码的功能
您可以使用单个命令输入和存储服务器的密码. Jupyter 服务器密码将提示您输入密码, 并将散列密码记录在 jupyter_server_config.json 中.
# 重置密码
这可用于重置丢失的密码; 或者, 如果您认为您的凭据已泄露并希望更改密码. 
更改密码将使服务器重新启动后所有登录会话失效.
$ jupyter server password
Enter password:  ****
Verify password: ****
[JupyterPasswordApp] Wrote hashed password to /Users/you/.jupyter/jupyter_server_config.json
# 用自带工具生成密码的 hash 串
>>> from jupyter_server.auth import passwd
>>> passwd()
... Enter password:
... Verify password:
'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
passwd() 在没有参数的情况下调用时, 将提示您输入并验证密码, 例如在上面的代码片段中.
虽然该函数也可以将字符串作为参数传递, 例如: passwd('mypassword'), 但不要在 IPython 会话中将字符串作为参数传递, 因为它将保存在您的输入历史记录中.
# 将生成的密码 hash 串添加到 notebook 配置文件中
默认位置, ~/.jupyter/jupyter_server_config.py
c.ServerApp.password = u'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
⚠️自动密码设置会将哈希存储在 jupyter_server_config.json 中, 而此方法将哈希存储在 jupyter_server_config.py 中. .json 配置选项优先于.py 配置选项, 因此如果 Json 文件设置了密码, 手动密码可能不会生效. 
# 使用 SSL 加密通讯
$ jupyter server --certfile=mycert.pem --keyfile mykey.key
可以使用 openssl 生成自签名证书. 例如, 以下命令将创建一个有效期为 365 天的证书, 并将密钥和证书数据写入同一文件:
$ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem
启动 notebook server 时, 浏览器可能会警告您的自签名证书不安全或无法识别.
如果您希望拥有一个不会引发警告的完全合规的自签名证书, 则可以(但涉及)创建一个, 如本教程中所述.(https://arstechnica.com/information-technology/2009/12/how-to-get-set-with-a-secure-sertificate-for-free/)
或者, 您可以使用 "让我们加密"(https://letsencrypt.org/) 获取免费的SSL证书, 并按照使用 "让我们加密"(https://jupyter-server.readthedocs.io/en/latest/operators/public-server.html#using-lets-encrypt) 中的步骤设置公共服务器.
# 运行一个公共 notebook server
通过浏览器远程访问 notebook server
1. 生成服务器配置文件: $ jupyter server --generate-config
2. 位置在 ~/.jupyter/jupyter_server_config.py
3. 默认, 所有字段都是注释掉了. 
4. 最小配置, 去掉注释
# Set options for certfile, ip, password, and toggle off
# browser auto-opening
c.ServerApp.certfile = u'/absolute/path/to/your/certificate/mycert.pem'
c.ServerApp.keyfile = u'/absolute/path/to/your/certificate/mykey.key'
# Set ip to '*' to bind on all interfaces (ips) for the public server
c.ServerApp.ip = '*'
c.ServerApp.password = u'sha1:bcd259ccf...<your hashed password here>'
c.ServerApp.open_browser = False

# It is a good idea to set a known, fixed port for server access
c.ServerApp.port = 9999
5. 启动服务器, $ jupyter server 
# 自定义 URL 前缀
c.ServerApp.base_url = '/ipython/'
# Docker CMD
Using jupyter server as a Docker CMD results in kernels repeatedly crashing, likely due to a lack of PID reaping. To avoid this, use the tini init as your Dockerfile ENTRYPOINT:
# Add Tini. Tini operates as a process subreaper for jupyter. This prevents
# kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini
ENTRYPOINT ["/usr/bin/tini", "--"]

EXPOSE 8888
CMD ["jupyter", "server", "--port=8888", "--no-browser", "--ip=0.0.0.0"]
```

#### 实践 practice

```sh
# practice
# generate server config
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter --paths
config:
    /usr/home/chenchen/.jupyter
    /data1/www/htdocs/chenchen/python31011/venv/nano/etc/jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
data:
    /usr/home/chenchen/.local/share/jupyter
    /data1/www/htdocs/chenchen/python31011/venv/nano/share/jupyter
    /usr/local/share/jupyter
    /usr/share/jupyter
runtime:
    /usr/home/chenchen/.local/share/jupyter/runtime
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter server --generate-config
Writing default config to: /usr/home/chenchen/.jupyter/jupyter_server_config.py
(nano) [chenchen@dev3_10.211.21.18 src]$ l /usr/home/chenchen/.jupyter/jupyter_server_config.py
276806 -rw-rw-r-- 1 chenchen chenchen 67K Jul 21 19:19 /usr/home/chenchen/.jupyter/jupyter_server_config.py

# edit server config
$ vim /usr/home/chenchen/.jupyter/jupyter_server_config.py
c.ServerApp.ip = '*'
c.ServerApp.open_browser = False
c.ServerApp.password = 'argon2:$argon2id$v=19$m=10240,t=10,p=8$SmlMq1V0KGr0/ub8VqnupA$68co4Gf011X2mttKE/9ccLFfUTrYDdPV2g3rjtz60B8'
c.ServerApp.port = 8765
c.ServerApp.certfile = u'/usr/local/sinasrv5/etc/ssl/server.crt'
c.ServerApp.keyfile = u'/usr/local/sinasrv5/etc/ssl/server.key'
    ssl_certificate      ssl/server.crt;
    ssl_certificate_key  ssl/server.key;


# generate password
(nano) [chenchen@dev3_10.211.21.18 src]$ python
Python 3.10.11 (main, Jun 28 2023, 00:37:48) [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from jupyter_server.auth import passwd
>>> passwd()
Enter password: root007
Verify password: root007
'argon2:$argon2id$v=19$m=10240,t=10,p=8$SmlMq1V0KGr0/ub8VqnupA$68co4Gf011X2mttKE/9ccLFfUTrYDdPV2g3rjtz60B8'


# 启动 server
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter server
[I 2023-07-21 20:10:24.268 ServerApp] Package jupyter_lsp took 0.0196s to import
......

# 查看正在运行的 jupyer server 列表, 一个目录(::后面的部分)可以接受多个浏览器
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter server list
Currently running servers:
http://localhost:8765/ :: /data1/www/htdocs/chenchen/python31011/workspace/nano/src

# 停止服务, stop 后加端口号
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter server stop 8765
# 但没起作用, 最后还是 kill 
(nano) [chenchen@dev3_10.211.21.18 src]$ ps -ef f | grep jupyter
chenchen 25945 30619  0 13:55 pts/19   S+     0:00  |           \_ grep --color=auto jupyter
chenchen 11610     1  0 Jul22 ?        Sl     0:55 /data1/www/htdocs/chenchen/python31011/venv/nano/bin/python3 /data1/www/htdocs/chenchen/python31011/venv/nano/bin/jupyter-server
(nano) [chenchen@dev3_10.211.21.18 src]$ kill 11610
(nano) [chenchen@dev3_10.211.21.18 src]$ jupyter server list
Currently running servers:

# as a background service
$ nohup jupyter server 2>&1 &
[1] 11610
(nano) [chenchen@dev3_10.211.21.18 src]$ nohup: ignoring input and appending output to ‘nohup.out’

# nohup
只输出错误信息到日志文件
nohup ./program >/dev/null 2>log &
什么信息也不要
nohup ./program >/dev/null 2>&1 &



#####
# as a service config
---
# service name:     jupyter.service 
# path:             /lib/systemd/system/jupyter.service

# After Ubuntu 16.04, Systemd becomes the default.
# Ref: https://gist.github.com/whophil/5a2eab328d2f8c16bb31c9ceaf23164f

[Unit]
Description=Jupyter Notebook Server

[Service]
Type=simple
PIDFile=/run/jupyter.pid

EnvironmentFile=/home/fermi/.jupyter/env

# Jupyter Notebook: change PATHs as needed for your system
ExecStart=/usr/local/bin/jupyter-notebook --config=/home/fermi/.jupyter/jupyter_notebook_config.py

# In case you want to use jupyter-lab 
# ExecStart=/usr/local/bin/jupyter-lab --config=/home/fermi/.jupyter/jupyter_notebook_config.py

User=fermi
Group=fermi
WorkingDirectory=/home/fermi/users
Restart=always
RestartSec=10
#KillMode=mixed

[Install]
WantedBy=multi-user.target

---
# Enable the service
sudo systemctl enable jupyter.service
Created symlink from /etc/systemd/system/multi-user.target.wants/jupyter.service to /lib/systemd/system/jupyter.service.

# Reload and Restart Service 
sudo systemctl daemon-reload
sudo systemctl restart jupyter.service


# Now check status of the jupyter service daemon

sudo service jupyter status
```

```sh
# service name:     jupyter.service 
# path:             /lib/systemd/system/jupyter.service

# After Ubuntu 16.04, Systemd becomes the default.
# Ref: https://gist.github.com/whophil/5a2eab328d2f8c16bb31c9ceaf23164f

[Unit]
Description=Jupyter Notebook Server

[Service]
Type=simple
PIDFile=/run/jupyter.pid

EnvironmentFile=/home/fermi/.jupyter/env

# Jupyter Notebook: change PATHs as needed for your system
ExecStart=/usr/local/bin/jupyter-notebook --config=/home/fermi/.jupyter/jupyter_notebook_config.py

# In case you want to use jupyter-lab 
# ExecStart=/usr/local/bin/jupyter-lab --config=/home/fermi/.jupyter/jupyter_notebook_config.py

User=fermi
Group=fermi
WorkingDirectory=/home/fermi/users
Restart=always
RestartSec=10
#KillMode=mixed

[Install]
WantedBy=multi-user.target
```



```sh
[Unit]
Description=Jupyter-Notebook Daemon

[Service]
Type=simple
ExecStart=/bin/bash -c "/home/username/.local/share/virtualenvs/notebook/bin/jupyter-notebook --no-browser --notebook-dir=/home/username"
WorkingDirectory=/home/username
User=username
Group=username
PIDFile=/run/jupyter-notebook.pid
Restart=on-failure
RestartSec=60s

[Install]
WantedBy=multi-user.target

---
# add the service file
$ sudo cp jupyter-notebook.service /etc/systemd/system/
# let the daemon reload all the service files
$ sudo systemctl daemon-reload
# (optional) "enable" -> started when the computer boots
$ sudo systemctl enable jupyter-notebook.service
# start the service
$ sudo systemctl start jupyter-notebook.service

$ systemctl status jupyter-notebook
```

```sh
# service setting file
位置: /etc/systemd/system/
```





## FAQ







## See Also

http://jupyter.org/

