# go

### 目录

[TOC]

### 仅安装一个版本

##### 镜像

```shell
找了一个中国, go本身下载速度很快,  https://golang.google.cn/
[chenchen@localhost tmp]$ wget https://golang.google.cn/dl/go1.14.6.linux-amd64.tar.gz         # 下载二进制, 而不是源码
[chenchen@localhost tmp]$ sha256sum go1.14.6.linux-amd64.tar.gz                                             # 与官网下载校验
5c566ddc2e0bcfc25c26a5dc44a440fcc0177f7350c1f01952b34d5989a0d287  go1.14.6.linux-amd64.tar.gz


Go 1.13 及以上（推荐）, 其实 go env 是修改了 $HOME/.config/go/env 物理文件, 增加了2条, 这两条优先级高, 覆盖默认配置.
打开你的终端并执行:
$ go env -w GO111MODULE=on
$ go env -w GOPROXY=https://goproxy.cn,direct                # GOPROXY="https://proxy.golang.org,direct"
完成.
$ go env GOPATH            # 查看当前的 GOPATH
# 重要说明
# GOROOT 可以不用加入系统环境变量(~/.bashrc); # export GOPATH=/home/chenchen/tmp/gopath
# GOPATH 必须加入系统环境变量(~/.bashrc), 加入后 go env GOPATH 的返回将和系统环境变量一致了, 这点必须检查清楚;
# GOPATH 可以设置多个, 其中, 第一个将会是默认的包目录, 使用 go get 下载的包都会在第一个 path 中的 src 目录下, 使用 go install 时, 在哪个 GOPATH 中找到了这个包, 就会在哪个 GOPATH 下的 bin 目录生成可执行文件.
# go env, 返回的各项环境变量设置才是运行时的效果, 以此为准



下面是另一种修改配置的办法, 没有上面好, 上面是官方推荐
macOS 或 Linux
打开你的终端并执行:
$ export GO111MODULE=on
$ export GOPROXY=https://goproxy.cn
或者,
$ echo "export GO111MODULE=on" >> ~/.profile
$ echo "export GOPROXY=https://goproxy.cn" >> ~/.profile
$ source ~/.profile
完成.
```

##### GO111MODULE

```shell
[chenchen@localhost hello]$ ~/tmp/goroot/go/bin/go build
go: go.mod file not found in current directory or any parent directory; see 'go help modules'
[chenchen@localhost hello]$ ~/tmp/goroot/go/bin/go help modules
Modules 是 Go 管理依赖项的方式.
一个 module模块 是一个已经发行的, 版本化之后的, 已经分发的 packages 的集合.
Modules模块 可以直接从版本控制仓库下载或者来自模块代理服务器.
有关模块的系列教程, 请参阅 https://golang.org/doc/tutorial/create-module [https://golang.google.cn/doc/tutorial/create-module]
有关模块的详细参考, 请参阅 https://golang.org/ref/mod [https://golang.google.cn/ref/mod]
默认情况下, go 命令可以从 https://proxy.golang.org 下载模块.
它可以使用校验和数据库对模块进行身份验证 https://sum.golang.org 这两项服务均由 Google 的 Go 团队运营.
这些服务的隐私政策可在 https://proxy.golang.org/privacy 和 https://sum.golang.org/privacy 分别
go 命令的 下载行为 可以使用 GOPROXY、GOSUMDB、GOPRIVATE 和其他环境变量. 参见“go help environment” 和 https://golang.org/ref/mod#private-module-privacy 了解更多信息
[chenchen@localhost hello]$ 


```

##### 下载

```shell
[chenchen@localhost tmp]$ yum list | grep wget
wget.x86_64                                 1.14-18.el7_6.1            base
[chenchen@localhost tmp]$ sudo yum -y install wget.x86_64
[chenchen@localhost tmp]$ wget https://golang.google.cn/dl/go1.16.4.linux-amd64.tar.gz
[chenchen@localhost tmp]$ sha256sum go1.16.4.linux-amd64.tar.gz
7154e88f5a8047aad4b80ebace58a059e36e7e2e4eb3b383127a28c711b4ff59  go1.16.4.linux-amd64.tar.gz
[chenchen@localhost tmp]$ tar -zxvf go1.16.4.linux-amd64.tar.gz -C ./
[chenchen@localhost tmp]$ mv ./go/ ~/program/go1.16.4
[chenchen@localhost ~]$ vim .bashrc

PATH="$HOME/.local/bin:$HOME/bin:$HOME/program/python3.9.5/bin:$HOME/program/go1.16.4/bin:$PATH"
export PATH

export GOROOT="$HOME/program/go1.16.4"		# Go本体(解压后拷贝的目录), 可选系统环境变量
export GOPATH="$HOME/gopath1.16.4"				# 工作空间, 必须系统环境变量
export GOPROXY="https://goproxy.cn"				# 包代理, 可选系统环境变量
export GOBIN="$HOME/gopath1.16.4/bin"			# go install 的目录, 可选系统环境变量
[chenchen@localhost ~]$ . .bashrc
[chenchen@localhost ~]$ go version
go version go1.16.4 linux/amd64
[chenchen@localhost ~]$ mkdir gopath1.16.4
```

##### 常量

```shell
GOROOT: Go本地安装的目录(解压后拷贝的目录)
GOPATH: 工作空间
GOPATH: {workspace}/src|pkg|bin/{base path}
GOBIN: 特指开发者编译后的二进制文件位置, 和 GOPATH/bin 是一个位置
import path 对应本地 {workspace} 里的位置或者 {远程仓库} 的位置

Go 代码组织结构:
    总览:
    . Go 程序通常把所有Go代码放在一个独立的 workspace 里
    . 一个工作空间可以包含很多版本控制 repositories, 例如: Git
    . 每个仓库包含一个或者多个 package
    . 每个 package 包在一个独立目录里含一个或者多个 Go 源文件
    . package 的目录路径决定了 import path
    请注意，这与其他编程环境不同，在其他编程环境中，每个项目都有一个单独的工作区，并且这些工作区与版本控制存储库紧密相关。
    工作区:
    一个工作区就是一个目录, 并且是在其根目录下包含两个目录:
        . src 目录, 包含 Go 源文件
            . src 子目录里通常包含多个版本控制仓库(Git)
        . bin 目录, 包含可执行的命令
```

##### 安装

```shell
1. 拷贝到 /usr/local/go 目录, go目录里的东西都放在这目录下面
2. 增加环境变量 /etc/profile(全局) 或者 $HOME/.profile(局部) 配置的是PATH
        export PATH=$PATH:/usr/local/go/bin
        source 之后立即生效
3. workspace 配置(GOPATH, 用来指定工作区, 一定不能与 Go 安装目录一致), (工作空间下必须3目录: src(源码), pkg(库, 包), bin(二进制))
        编辑 ~/.bash_profile 文件
        增加 export GOPATH=$HOME/go 配置 或者 GOBIN 环境变量
         使之立即生效 source ~/.bash_profile

同一台机器同时安装多个 Go 版本: 配置(GOROOT), GOROOT 是描述 Go 安装目录的环境变量.
```

##### 测试

```shell
    1. 在 工作目录/src目录的 hello 目录下创建 hello.go 文件, 即, workspace/src/hello/hello.go
        package main
        import "fmt"
        func main() {
            fmt.Printf("hello, world\n")
        }
    2. 构建,
        $ cd $HOME/go/src/hello
        $ go build
    3. 运行,
        $ ./hello
        hello, world
    4. 安装到 bin 目录, 或者删除
        $ go install        // 安装到了 workspace 的 bin 目录里
        $ go clean -i      // 删除上一步的安装
```

##### 卸载

```shell
    1. 直接删除go本体目录(GOROOT)
    2. 直接删除系统环境变量 /etc/profile, 或者 $HOME/.profile. mac 在 /etc/paths.d/go 文件
```

### 多版本安装

```shell
# 多个版本安装在同一个机器上
# 多版本下载: https://golang.org/dl/
# 这里介绍的方法需要使用到 git, 也就是说要事先安装 git

$ go get golang.org/dl/go1.10.7
$ go1.10.7 download
$ go1.10.7 version
go version go1.10.7 linux/amd64
$ go1.10.7 env GOROOT
```

### 多版本卸载

```shell
1. 删除 GOROOT 环境变量里指定的目录
2. 删除 goX.Y.Z 二进制文件所在目录
```

### 卸载

```shell
1. 删除go安装目录
2. 删除环境变量, /etc/profile 或者 $HOME/.profile 或者 /etc/paths.d/go
```

### 更多信息

```shell
[chenchen@localhost hello]$ go help
Go is a tool for managing Go source code.                                       # Go 是一个管理 Go 源码的工具

Usage:

        go <command> [arguments]

The commands are:

        bug         start a bug report                                          # 启动错误报告
        build       compile packages and dependencies                           # 构建编译包和依赖关系
        clean       remove object files and cached files                        # 清除删除对象文件和缓存文件
        doc         show documentation for package or symbol                    # 显示包裹或符号的文档
        env         print Go environment information                            # 打印 Go 环境信息
        fix         update packages to use new APIs                             # 更新包以便使用新的API
        fmt         gofmt (reformat) package sources                            # 用 gofmt 格式化 go 源码
        generate    generate Go files by processing source                      # 处理源码生成 Go 文件
        get         add dependencies to current module and install them         # 将依赖性添加到当前模块并安装它们
        install     compile and install packages and dependencies               # 编译和安装软件包和依赖关系
        list        list packages or modules                                    # 列表展示包或模块
        mod         module maintenance                                          # 模块维护
        run         compile and run Go program                                  # 编译和运行 Go 程序
        test        test packages                                               # 测试指定包
        tool        run specified go tool                                       # 运行指定的去工具
        version     print Go version                                            # 打印 Go 版本
        vet         report likely mistakes in packages                          # 报告包中可能出现错误

Use "go help <command>" for more information about a command.

Additional help topics:                                                         # 其他帮助主题

        buildconstraint build constraints                                       # 构建约束
        buildmode       build modes                                             # 构建模式
        c               calling between Go and C                                # 在 Go 和 C 之间调用
        cache           build and test caching                                  # 构建和测试缓存
        environment     environment variables                                   # 环境变量
        filetype        file types                                              # 文件类型
        go.mod          the go.mod file                                         # go.mod 文件
        gopath          GOPATH environment variable                             # GOPATH 环境变量
        gopath-get      legacy GOPATH go get                                    # 历史遗留 GOPATH 环境变量, go get
        goproxy         module proxy protocol                                   # 模块代理协议
        importpath      import path syntax                                      # import 路径语法
        modules         modules, module versions, and more                      # 模块、模块版本等
        module-get      module-aware go get                                     # 模块发觉, go get
        module-auth     module authentication using go.sum                      # 使用 go.sum 来模块身份验证
        packages        package lists and patterns                              # 包列表和模式匹配
        private         configuration for downloading non-public code           # 用于下载非公共代码的私人配置
        testflag        testing flags                                           # 测试标记
        testfunc        testing functions                                       # 测试函数
        vcs             controlling version control with GOVCS                  # 用 GOVCS 环境变量来控制版本控制

Use "go help <topic>" for more information about that topic.

[chenchen@localhost hello]$
```
