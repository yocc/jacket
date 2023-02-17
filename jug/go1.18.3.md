# go1.18.3

----

[toc]



## Overview

 GOROOT/bin/go 是一个管理 Go 源码的工具



## Linux install, 二进制安装

1. 通过删除 /usr/local/go 文件夹(如果存在)来删除任何以前的 Go 安装, 然后将刚刚下载的存档解压缩到 /usr/local, 在 /usr/local/go 中创建一个新的 Go 树:
	```shell
	$ rm -rf /usr/local/go &amp;&amp; tar -C /usr/local -xzvf go1.18.3.linux-amd64.tar.gz
	```
	(您可能需要以 root 身份或通过 sudo 运行该命令)
	
	> 注意: tar -C /usr/local/go -xzvf go1.18.3.linux-amd64.tar.gz, 这是错误的. 
	>
	> ​		 安装的目标目录结构是 /usr/local/go/bin, 而不是 /usr/local/go/go/bin
	
2. 将 /usr/local/go/bin 添加到 PATH 环境变量中.
    您可以通过将以下行添加到您的 $HOME/.bashrc 或 /etc/bashrc  (用于系统范围的安装) 来做到这一点:

  ```shell
  # ~/.bashrc 中增加 go 本体命令的配置
  # export PATH=$PATH:/usr/local/go/bin
  export PATH=$PATH:/home/chenchen/tmp/go1183/go/bin
  ```
  ```shell
  source $HOME/.bashrc
  ```

  > PATH 环境变量里前面的优先执行, 优先级更高.

3. 通过打开命令提示符并键入以下命令来验证您是否已安装 Go:
	```shell
	$ go version
	```
4. 确认该命令打印已安装的 Go 版本.



## go 配置

```shell
# ~/.bashrc 中增加新的环境变量: GOPATH, GOROOT, GOBIN

# go1183
export GOPATH=/home/chenchen/tmp/go1183/gopath
export GOROOT=/home/chenchen/tmp/go1183/go
export GOBIN=/home/chenchen/tmp/go1183/gobin

# ~/.bashrc 中增加 upx 配置
# upx
export PATH=$PATH:/home/chenchen/tmp/go1183/upx396/upx-3.96-amd64_linux
```

## Go 配置

```shell
另一个角度说明:

一. 配置文件位置:
		~/.bashrc

二. 配置内容:
1. go 本体的工具二进制文件路径 (必须)
# export PATH=$PATH:/usr/local/go/bin											# 生产环境
export PATH=$PATH:/home/chenchen/tmp/go1183/go/bin				# 开发环境, 多版本环境

2. 不同版本的一些配置
# go1183
export GOPATH=/home/chenchen/tmp/go1183/gopath						# 工作目录, workspace, 必须, (包含, bin, src, pkg 目录)
export GOROOT=/home/chenchen/tmp/go1183/go								# go 本体安装位置, go 本体的根目录, 可选, (与 PATH 相关)
export GOBIN=/home/chenchen/tmp/go1183/gobin							# 源码编译后生成二进制文件的位置, 可选, (与 GOPATH/bin 相关)

3. ~/.config/go/env, 运行时配置
参见下文.

⚠️ GOROOT/bin 和 GOBIN, 是两个不同含义, GOROOT/bin 本体二进制命令; GOBIN 是源码编译发布后的二进制命令; 二者含义不同, 但有可能指向同一个目录, 也就是将这些二进制文件放一起(并不推荐, 至少会重名)
```



## UPX, 编译生成的文件 体积过大问题

```shell
# build|install -ldflags="-w, 去掉DWARF调试信息, 不能gdb调试", "-s, 去掉符号信息"
[chenchen@localhost hello]$ go install -ldflags='-w'
[chenchen@localhost hello]$ l ../../../gobin
total 3.0M
 2614449 -rwxrwxr-x. 1 chenchen chenchen 1.3M Oct 12 04:22 hello
 # upx
 [chenchen@localhost hello]$ upx -9 hello		// 是否存在 -s 参数
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
   1307619 ->    509756   38.98%   linux/amd64   hello                         

Packed 1 file.
```



## UPX binary install for Linux

```shell
# UPX 是打包程序(packer), 自身可执行.
# UPX 是一款免费、可移植、可扩展、高性能的可执行, 打包程序, 适用于多种可执行格式.

# 以下过程从创建 /home/chenchen/tmp/go1183/upx396 目录开始:
[chenchen@grpc01 go1183]$ pwd
/home/chenchen/tmp/go1183
[chenchen@grpc01 go1183]$ l
total 4.0K
134246039 drwxr-xr-x. 10 chenchen chenchen  257 Jun  2 00:52 go
   595690 drwxrwxr-x. 33 chenchen chenchen 4.0K Jul  7 03:51 ..
100724614 drwxrwxr-x.  3 chenchen chenchen   51 Jul  7 03:57 tmp
423656465 drwxrwxr-x.  2 chenchen chenchen    6 Jul  8 13:17 gopath
427876412 drwxrwxr-x.  2 chenchen chenchen    6 Jul  8 13:17 gobin
 96607976 drwxrwxr-x.  6 chenchen chenchen   54 Jul  8 13:17 .

[chenchen@grpc01 go1183]$ mkdir upx396
[chenchen@grpc01 go1183]$ l
total 4.0K
134246039 drwxr-xr-x. 10 chenchen chenchen  257 Jun  2 00:52 go
   595690 drwxrwxr-x. 33 chenchen chenchen 4.0K Jul  7 03:51 ..
100724614 drwxrwxr-x.  3 chenchen chenchen   51 Jul  7 03:57 tmp
423656465 drwxrwxr-x.  2 chenchen chenchen    6 Jul  8 13:17 gopath
427876412 drwxrwxr-x.  2 chenchen chenchen    6 Jul  8 13:17 gobin
432015596 drwxrwxr-x.  2 chenchen chenchen    6 Jul  8 14:10 upx396
 96607976 drwxrwxr-x.  7 chenchen chenchen   68 Jul  8 14:10 .

[chenchen@grpc01 go1183]$ cd upx396
[chenchen@grpc01 upx396]$ l
total 0
 96607976 drwxrwxr-x. 7 chenchen chenchen 68 Jul  8 14:10 ..
432015596 drwxrwxr-x. 2 chenchen chenchen  6 Jul  8 14:10 .
# 下载 Intel 64位处理器 Linux 版, 如果 wget 失败, 请多实验几次.
[chenchen@grpc01 upx396]$ wget https://github.com/upx/upx/releases/download/v3.96/upx-3.96-amd64_linux.tar.xz
--2022-07-08 14:11:04--  https://github.com/upx/upx/releases/download/v3.96/upx-3.96-amd64_linux.tar.xz
Connecting to 127.0.0.1:7890... connected.
... 
2022-07-08 14:11:08 (297 KB/s) - ‘upx-3.96-amd64_linux.tar.xz’ saved [462784/462784]

[chenchen@grpc01 upx396]$ l
total 452K
432015597 -rw-rw-r--. 1 chenchen chenchen 452K Dec  8  2021 upx-3.96-amd64_linux.tar.xz
 96607976 drwxrwxr-x. 7 chenchen chenchen   68 Jul  8 14:10 ..
432015596 drwxrwxr-x. 2 chenchen chenchen   41 Jul  8 14:11 .

# 解缩 .xz 压缩文件
[chenchen@grpc01 upx396]$ xz -d upx-3.96-amd64_linux.tar.xz
[chenchen@grpc01 upx396]$ l
total 600K
432015598 -rw-rw-r--. 1 chenchen chenchen 600K Dec  8  2021 upx-3.96-amd64_linux.tar
 96607976 drwxrwxr-x. 7 chenchen chenchen   68 Jul  8 14:10 ..
432015596 drwxrwxr-x. 2 chenchen chenchen   38 Jul  8 14:12 .

# 解包 .tar 打包文件
[chenchen@grpc01 upx396]$ tar xvf upx-3.96-amd64_linux.tar
upx-3.96-amd64_linux/
upx-3.96-amd64_linux/BUGS
upx-3.96-amd64_linux/COPYING
upx-3.96-amd64_linux/LICENSE
upx-3.96-amd64_linux/NEWS
upx-3.96-amd64_linux/README
upx-3.96-amd64_linux/README.1ST
upx-3.96-amd64_linux/THANKS
upx-3.96-amd64_linux/upx
upx-3.96-amd64_linux/upx.1
upx-3.96-amd64_linux/upx.doc
upx-3.96-amd64_linux/upx.html
[chenchen@grpc01 upx396]$ l
total 600K
436228024 drwxr-xr-x. 2 chenchen chenchen  161 Jan 24  2020 upx-3.96-amd64_linux
432015598 -rw-rw-r--. 1 chenchen chenchen 600K Dec  8  2021 upx-3.96-amd64_linux.tar
 96607976 drwxrwxr-x. 7 chenchen chenchen   68 Jul  8 14:10 ..
432015596 drwxrwxr-x. 3 chenchen chenchen   66 Jul  8 14:12 .

[chenchen@grpc01 upx396]$ cd upx-3.96-amd64_linux
[chenchen@grpc01 upx-3.96-amd64_linux]$ l
total 616K
436228035 -rw-r--r--. 1 chenchen chenchen  39K Jan 24  2020 upx.html
436228034 -rw-r--r--. 1 chenchen chenchen  37K Jan 24  2020 upx.doc
436228033 -rw-r--r--. 1 chenchen chenchen  43K Jan 24  2020 upx.1
436228032 -rwxr-xr-x. 1 chenchen chenchen 419K Jan 24  2020 upx		 	# 二进制可执行文件
436228031 -rw-r--r--. 1 chenchen chenchen 2.2K Jan 24  2020 THANKS
436228030 -rw-r--r--. 1 chenchen chenchen  800 Jan 24  2020 README.1ST
436228029 -rw-r--r--. 1 chenchen chenchen 4.5K Jan 24  2020 README
436228028 -rw-r--r--. 1 chenchen chenchen  23K Jan 24  2020 NEWS
436228027 -rw-r--r--. 1 chenchen chenchen 5.4K Jan 24  2020 LICENSE
436228026 -rw-r--r--. 1 chenchen chenchen  18K Jan 24  2020 COPYING
436228025 -rw-r--r--. 1 chenchen chenchen 1.8K Jan 24  2020 BUGS
436228024 drwxr-xr-x. 2 chenchen chenchen  161 Jan 24  2020 .
432015596 drwxrwxr-x. 3 chenchen chenchen   66 Jul  8 14:12 ..
[chenchen@grpc01 upx-3.96-amd64_linux]$ pwd
/home/chenchen/tmp/go1183/upx396/upx-3.96-amd64_linux								# 为了便于使用, 需配置进 ~/.bashrc 中
[chenchen@grpc01 upx-3.96-amd64_linux]$
```



## 配置文件总结

### ~/.config/go/env, 运行时的配置

```shell
# ~/.config/go/env, 此文件为 go 运行时才起效果的临时时配置文件, 按照最小原则, 应该优先使用在这个配置文件里配置, 当不能满足需要时, 才会用到 ~/.bashrc 文件.

#GO111MODULE=on
GOPROXY=https://goproxy.cn,direct
#GOROOT=/home/chenchen/tmp/go1172/go
#GOBIN=/home/chenchen/tmp/go1172/gobin

⚠️ 经过反复使用验证, 进需要 GOPROXY 一行配置, 其余都要放入 ~/.bashrc 中
```



### ~/.bashrc 文件

```shell
# 最终 go 相关的 ~/.bashrc 配置如下, 包括 golang, upx, vim, grpc 等等

# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
    . /etc/bashrc
fi

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
 export LS_OPTIONS='--color=auto'
 eval "`dircolors`"
 alias ls='ls $LS_OPTIONS'
 alias ll='ls $LS_OPTIONS -l'
#alias l='ls $LS_OPTIONS -lA'
 alias l='ls $LS_OPTIONS -alrthi'
 alias lm='ls $LS_OPTIONS -alrthi | more'
 export TERM='xterm-256color'
 # export TERM='screen-256color'

HISTSIZE=45000
ISTFILESIZE=45000

# http://10.210.97.118/proxy.pac
# 10.255.0.186:3128
# 10.81.254.21:3128
#export http_proxy="http://10.81.254.21:3128"
#export https_proxy="http://10.81.254.21:3128"
#export http_proxy="http://10.255.0.186:3128"
#export https_proxy="http://10.255.0.186:3128"

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
# [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

# nvm 加速
# NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node

# vim8
### export PATH=/usr/local/vim8/bin:$PATH
# vim9
export PATH=/usr/local/vim9/bin:$PATH

# go1172
### export PATH=$PATH:/home/chenchen/tmp/go1172/go/bin
### export GOPATH=/home/chenchen/tmp/go1172/gopath
### export GOROOT=/home/chenchen/tmp/go1172/go
### export GOBIN=/home/chenchen/tmp/go1172/gobin

# go1183
export PATH=$PATH:/home/chenchen/tmp/go1183/go/bin
export GOPATH=/home/chenchen/tmp/go1183/gopath
export GOROOT=/home/chenchen/tmp/go1183/go
export GOBIN=/home/chenchen/tmp/go1183/gobin

# upx
### export PATH=$PATH:/home/chenchen/tmp/upx-3.96-amd64_linux
export PATH=$PATH:/home/chenchen/tmp/go1183/upx396/upx-3.96-amd64_linux

# protoc
export PATH="$PATH:/home/chenchen/tmp/protoc-3.19.4/bin"

# Go plugins
# export PATH="$PATH:$GOBIN"
export PATH="$PATH:$(go env GOBIN)"

# ES
#export ES_HOME="$HOME/elasticsearch-7.14.0"
export ES_HOME="/usr/local/elasticsearch-7.14.0"
#export ES_JAVA_HOME="$ES_HOME/jdk"
#export ES_JAVA_HOME="./jdk"

# OpenSSL
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/openssl111m/lib  #.so file path

# Clash
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890

# replication
export PATH="$(echo "$PATH" | /bin/awk 'BEGIN{RS=":";}{sub(sprintf("%c$",10),"");if(A[$0]){}else{A[$0]=1;printf(((NR==1)?"":":")$0)}}')";
```



### 环境变量说明

```shell
GOROOT, 配置在 ~/.bashrc 中, 可选项; go 软件/工具本体安装的根目录位置.

GOPATH, 配置在 ~/.bashrc 中, 必选项; 配置后必须检查 `$ go env GOPATH` 的返回将和系统环境变量一致了, 这点必须检查清楚;
GOPATH, 可以设置多个, 其中, 第一个将会是默认的包目录, 使用 go get 下载的包都会在第一个 path 中的 src 目录下, 使用 go install 时, 在哪个 GOPATH 中找到了这个包, 就会在哪个 GOPATH 下的 bin 目录生成可执行文件.

GOBIN, 配置在 ~/.bashrc 中, 可选项; `$ go install` 的目录, 开发出的二进制文件生成位置.

GOPROXY, 配置在 ~/.config/go/env 中, 可选项; 用于 go 运行时需要的资源代理配置, 提高访问速度或者满足科学上网.

PATH, 配置在 ~/.bashrc 中, 必选项; 用于简化便捷二进制工具命令使用, 主要是 `GOROOT/bin/go` 工具 和 `GOBIN` 生成后的工具 和 `UPX` 工具.
```



## 实验

```shell
    1. 在 GOPATH/src 目录的 hello 目录下创建 hello.go 文件, 即, GOPATH/src/hello/hello.go
        package main
        import "fmt"
        func main() {
            fmt.Printf("hello, world\n")
        }
    2. 构建,
        $ cd GOPATH/src/hello
        $ go build hello.go
    3. 运行,
        $ ./hello
        hello, world
    4. 安装到 bin 目录, 或者删除
        $ go install        // 安装到了 workspace 的 bin 目录里
        $ go clean -i       // 删除上一步的安装
```

## packages, module

## 依赖, 扩展包的安装

```shell
# 有两种方式:
1. 直接在代码中 import 包, 条件是 go本体版本要大于 Go 1.11+
import "google.golang.org/grpc"
这样在代码被执行时(go build, run, test)再自动下载获取必要的依赖包文件.

2. 通过 got get 事先下载到本体指定目录下保存, 待到编译时直接参与使用.
$ go get -u google.golang.org/grpc

参考: https://github.com/grpc/grpc-go#installation
     https://github.com/golang/go/wiki/Modules  中国翻墙说明
```



## 杂项

```shell
# 标准库位置:
$GOROOT/pkg/$GOOS_$GOARCH/
go本体\pkg\linux_amd64

# build:
usage: go build [-o output] [build flags] [packages]
[chenchen@localhost e04_2]$ go build -o hh7 -ldflags="-s -w" hello_world.go 
[chenchen@localhost e04_2]$ go build -o count_characters -ldflags "-s -w" count_characters.go

# install
usage: go install [build flags] [packages]
[chenchen@grpc01 hello]$ go install -ldflags "-s -w" hello.go
```



## 卸载

```shell
    1. 直接删除 go 本体安装的根目录 (GOROOT)
    2. 直接删除 ~/.config/go/env 和 ~/.bashrc 中的环境变量, GOROOT, GOPATH, GOBIN, GOPROXY, PATH的一部分 等等.
```



## go mod

### Overview

```shell
# 我的理解
module模块目录/package包目录/*.go文件

import 导入是 module模块 目录
import "module模块/package包"

module目录/package目录/xx.go文件, 中有 package main 这行语句, 并且有 main(), 此为程序|模块入口
module目录/package01目录/yy.go文件, 中有 package zz 这行语句
```



```shell
# 程序/模块
程序|模块module, 是由多个包pkg组成, 包又分为自身和导入, 程序|模块module 通过 import 将一堆包pkg组织到一个程序|模块module下面.
一个包pkg是由多个 .go 源文件组成. 一个 .go 文件仅能属于一个包pkg, 因为在 .go 文件的第一行指明了这个文件属于哪个包pkg.

# package
import 导入的其实是目录, 并不是 pkg包 的名字, 只是通常恰好一致, 其实是可以随便定义目录名.
例如：package big ，我们 import “math/big” ，其实是在 src 中的 src/math 目录。在代码中使用 big.Int 时，big 指的才是 Go 文件中定义的 package 名字。
如果包名不是以 ./, 如 "fmt" 或者 "container/list", 则 Go 会在全局文件进行查找; 如果包名以 ./ 开头, 则 Go 会在相对目录中查找.
1. 相对路径
`import   "./module"   		// 当前文件同一目录的 module 目录, 此方式没什么用容易出错, 不建议用`
2. 绝对路径
`import  "LearnGo/init"  	// 加载 GOPATH/src/LearnGo/init 模块, 一般建议这样使用"`

# init()
特殊函数, pkg包 中含有 init() 就必须优先执行.
程序|模块module 开始执行并完成初始化后, 第一个调用的函数是 main.main() (程序入口), 但如果有 init() 函数那么就会更优先执行这个函数.
init() 和 main() 一样, 既没有参数, 也没有返回类型.

# package main & main()
包名必须小写.
每个 Go 程序|模块, 都必须仅包含一个名为 main 的包. 
package main 包是特殊的包, 表示一个可以独立执行的程序|模块.
package main 包目录里包含多个 .go 文件, 但只能有一个 main() 方法, main() 方法是 程序|模块 入口.
main 包不能被 import.

# .go 文件
每个 .go 文件第一行必须声明本 .go 文件所属的 包pkg. 一个 .go 文件仅能属于一个包pkg.
非 main 包源文件编译后产生 .a 文件, 而不是可执行文件.

# module & package
多个包组成一个模块
模块是相关的 Go包的集合. 模块是源代码交换和版本控制的单元. 
```



```shell
# Go 程序|模块module 的执行顺序:
入口(main 包) ... -> ... main()
. 程序的初始化和执行都起始于 main 包
. 如果 main 包还导入了其它的包, 那么就会在编译时将它们依次导入(一个包会被多个包同时导入, 那么它只会被导入一次)
. 当一个包被导入时, 如果该包还导入了其它的包, 那么会先将其它包导入进来
	.. 然后再对这些包中的包级常量和变量进行初始化
	.. 接着执行 init() (如果有的话), 依次类推
. 待所有被导入的包都加载完毕了,
	.. 就会开始对 main 包中的包级常量和变量进行初始化,
	.. 然后执行 main 包中的 init() (如果存在的话),
. 最后执行 main()
```



```shell
# init()
init() 用于包 (package) 的初始化
. init 函数是用于程序执行前做包的初始化的函数, 比如初始化包里的变量等
. 每个 pkg包 可以拥有多个 init()
. 包的每个 .go 源文件也可以拥有多个 init()
. 同一个包中多个 init() 的执行顺序 Go 语言没有明确的定义 (说明)
. 不同包的 init() 按照包导入的依赖关系决定该初始化函数的执行顺序
. init() 不能被其他函数调用, 而是在 main 函数执行之前, 自动被调用
```



```shell
# go mod 是什么?
Go mod 提供对模块操作的访问. 

# 目的
为了解决在代码中出现的非内置包, 如下, `"github.com/buger/jsonparser"` 就是第三方反序列化 json 包. 
在源码中增加了对第三方 jsonparser 包的引用, 但此时实际上还未曾下载 jsonparser 包本身. 需要一下操作来完成, 并能管理和升级维护等.

import (
    "fmt"
    "io"
    "net/http"

    "github.com/buger/jsonparser"
)

# 结论
1. 需要 go 版本大于 1.13 + 
2. ~/.config/go/env 文件应该默认 GO111MODULE 为on或者不写(直接注释掉), 整个文件内容如下:
#GO111MODULE=on
GOPROXY=https://goproxy.cn,direct
#GOROOT=/home/chenchen/tmp/go1172/go
#GOBIN=/home/chenchen/tmp/go1172/gobin

# troubleshooting:
1. 在项目源码中直接引入第三方包, 比如, import "..."
2. 检查 ~/.config/go/env 文件配置如上所示
3. 在项目源码目录下创建 go.mod 文件, 命令如下
$ go mod init 项目源码目录名(比如, nethttp)
[chenchen@grpc01 nethttp]$ go mod init nethttp
go: creating new go.mod: module nethttp
go: to add module requirements and sums:
        go mod tidy
[chenchen@grpc01 nethttp]$ go mod tidy
go: finding module for package github.com/buger/jsonparser
go: found github.com/buger/jsonparser in github.com/buger/jsonparser v1.1.1
[chenchen@grpc01 nethttp]$ 
4. 用 go mod tidy 来下载第三方模块实际代码文件, 并能移除不用的模块
5. 至此 go run 源码文件 应该可以跑起来了
```



### go mod 命令

```shell
go mod init 项目源码目录名, 在当前目录初始化一个新模块
go mod tidy, 根据需要自动添加或者删除模块
go mod graph, 打印模块依赖图
go mod download, 下载依赖包
go mod edit, 通过工具或者脚本来编辑 go.mod 文件
go mod verify, 验证依赖是否正确
go mod vendor, 将依赖复制到 vendor 下面
go mod why, 解释为什么需要依赖包
```



### go.mod 文件中的4条语句

```shell
# 以下4条语句用于 go.mod 文件中
module, 语句指定包的名字(路径)
require, 语句指定的依赖项模块
replace, 语句可以替换依赖项模块
exclude, 语句可以忽略依赖项模块
```



### 第三方模块文件下载到了哪里?

```shell
如: GOPATH/pkg/mod/github.com/buger/jsonparser\@v1.1.1/
在 GOPATH/pkg/mod/
在 src/ 平行的 pkg/mod/
```



## FAQ & troubleshooting

```shell
1. ~/.bashrc 有关的内容, 参见, ~.bashrc.md 文件
```

```go
2. 
// 问题:
[chenchen@grpc01 linux_amd64]$ go get github.com/mailru/easyjson && go install github.com/mailru/easyjson/...@latest
        'go get' 在模块之外不再受支持.
        要 build构建 和 install安装 一个命令, 请使用 'go install' 和版本,
        比如 'go install example.com/cmd@latest'
        有关详细信息, 请参阅 https://golang.org/doc/go-get-install-deprecation
        或运行 'go help get' 或 'go help install'.
[chenchen@grpc01 linux_amd64]$

// 解决:
到模块目录下(含有 go.mod 文件), 执行 go get 和 go install
[chenchen@grpc01 nethttp]$ l
total 36K
440448792 drwxrwxr-x. 4 chenchen chenchen   34 Jul 10 16:49 ..
247550030 -rw-rw-r--. 1 chenchen chenchen  206 Jul 11 02:58 stru.go
247550028 -rw-rw-r--. 1 chenchen chenchen 8.3K Jul 11 03:16 jqfir.json
247550029 -rw-rw-r--. 1 chenchen chenchen  173 Jul 11 06:06 go.sum
247550027 -rw-rw-r--. 1 chenchen chenchen   68 Jul 11 06:12 go.mod
247550037 -rw-rw-r--. 1 chenchen chenchen  681 Jul 13 20:39 edh.go
247550036 -rw-rw-r--. 1 chenchen chenchen 1.2K Jul 13 20:40 srg.go
247550035 -rw-rw-r--. 1 chenchen chenchen 2.4K Jul 13 22:01 fir.go
247550026 drwxrwxr-x. 2 chenchen chenchen  109 Jul 14 00:02 .
[chenchen@grpc01 nethttp]$ go get github.com/mailru/easyjson && go install github.com/mailru/easyjson/...@latest
go: downloading github.com/mailru/easyjson v0.7.7
go: downloading github.com/josharian/intern v1.0.0
go: added github.com/josharian/intern v1.0.0
go: added github.com/mailru/easyjson v0.7.7
[chenchen@grpc01 nethttp]$ l
```



## See Also

https://upx.github.io/

https://en.wikipedia.org/wiki/UPX

https://github.com/upx/upx

https://github.com/upx/upx/releases

http://compression.ca/





