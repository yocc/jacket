# Python3

### 下载 Python3 源码

```
$ wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
```

### 解压缩

```
$ tar zxvf 源码.tgz -C ./
```

### 基本工具, ssl 模块, ctypes 模块

```
$ sudo yum -y install gcc make automake autoconf libtool
$ sudo yum -y install openssl openssl-devel
$ sudo yum -y install libffi libffi-devel
```

### ./configure

```
./configure # 不默认安装 ./configure --prefix=/home/chenchen/program/python3 --exec-prefix=/home/chenchen/bin --enable-shared --enable-optimizations --enable-big-digits        # prefix 程序本身的位置; exec-prefix 执行文件的位置

./configure --prefix=$HOME/program/python3 --enable-shared --enable-optimizations --enable-big-digits  --with-system-ffi  (使用)

./configure --prefix=$HOME/program/python3 --exec-prefix=$HOME/bin --enable-shared --enable-optimizations --enable-big-digits  --with-system-ffi
```

### make, make install

```
make                    # make distclean 清除 configure
make install
(VirtualBox 上需要30分钟左右)
```

### python 问题

> 参考 FAQ

### pip, 安装包

```
/home/chenchen/program/python3/lib/python3.8/site-packages/包名
python3本体安装目录: $HOME/program/python3
python3本体安装目录/lib/python版本/site-packages/包名

$ python3 -m pip install --upgrade pip      # 升级自身
$ python3 -m pip install 包名                # 安装
$ python3 -m pip install --user 包名         # 只为当前用户安装
$ python3 -m pip list                       # 列出所有已经安装过的包名
$ python3 -m pip show 包名                   # 列出安装过的包名的详细信息
$ python3 -m pip search 包名                 # 从远程PyPI里找包
```

### 安装 gRPC

```
$ python -m pip install grpcio                  # 安装 gRPC
$ python -m pip install grpcio-tools            # 安装 gRPC 工具, 工具包括 protoc(protocol buffer 编译器) 和 插件(从 .proto 服务定义文件 生成 server 和 client 代码)
```

### FAQ

```
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
->运行 sudo ldconfig
方法2:
设置环境变量：(试验了, 不起作用)
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/python3/bin
注：其中/usr/local/python3/为python3安装的目录
方法3: (最不可取, 没有实验, 以前貌似都是这么干)
root@localhost lib]# cd /usr/local/bin/python3/lib
[root@localhost lib]# cp libpython3.7m.so.1.0 /usr/lib64
```

```
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

```
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

### See Also

https://www.python.org/downloads/source/
