# go get

---



## 目录

[TOC]



## 总揽

```shell
# 也就是说，go get 只用来下载普通的包，安装可执行程序，应该使用 go install。
$ go get, 只是仅仅下载源码, 只用于调整当前模块的依赖关系.
$ go install, 用来给当前模块安装一个有依赖关系的包

module cache 在哪里?
# go get 下载源码到这个位置, 自带环境变量
$ go env
GOMODCACHE="/home/chenchen/tmp/go1172/gopath/pkg/mod"
```



## go get

```shell
# 为包添加依赖, 或将其升级到最新版本:
$ go example.com/pkg

# 将软件包升级或降级到特定版本:
$ go example.com/pkg@v1.2.3

# 删除对一个模块的依赖关系同时包不再需要他:
$ go example.com/mod@none
```





## $ go help get

```shell
[chenchen@localhost goproj]$ go get help
go get: malformed module path "help": missing dot in first path element
[chenchen@localhost goproj]$ go help get
usage: go get [-d] [-t] [-u] [-v] [-insecure] [build flags] [packages]

Get resolves its command-line arguments to packages at specific module versions,
updates go.mod to require those versions, downloads source code into the
module cache, then builds and installs the named packages.

To add a dependency for a package or upgrade it to its latest version:

        go get example.com/pkg

To upgrade or downgrade a package to a specific version:

        go get example.com/pkg@v1.2.3

To remove a dependency on a module and downgrade modules that require it:

        go get example.com/mod@none

See https://golang.org/ref/mod#go-get for details.

The 'go install' command may be used to build and install packages. When a
version is specified, 'go install' runs in module-aware mode and ignores
the go.mod file in the current directory. For example:

        go install example.com/pkg@v1.2.3
        go install example.com/pkg@latest

See 'go help install' or https://golang.org/ref/mod#go-install for details.

In addition to build flags (listed in 'go help build') 'go get' accepts the
following flags.

The -t flag instructs get to consider modules needed to build tests of
packages specified on the command line.

The -u flag instructs get to update modules providing dependencies
of packages named on the command line to use newer minor or patch
releases when available.

The -u=patch flag (not -u patch) also instructs get to update dependencies,
but changes the default to select patch releases.

When the -t and -u flags are used together, get will update
test dependencies as well.

The -insecure flag permits fetching from repositories and resolving
custom domains using insecure schemes such as HTTP, and also bypassess
module sum validation using the checksum database. Use with caution.
This flag is deprecated and will be removed in a future version of go.
To permit the use of insecure schemes, use the GOINSECURE environment
variable instead. To bypass module sum validation, use GOPRIVATE or
GONOSUMDB. See 'go help environment' for details.

The -d flag instructs get not to build or install packages. get will only
update go.mod and download source code needed to build packages.

Building and installing packages with get is deprecated. In a future release,
the -d flag will be enabled by default, and 'go get' will be only be used to
adjust dependencies of the current module. To install a package using
dependencies from the current module, use 'go install'. To install a package
ignoring the current module, use 'go install' with an @version suffix like
"@latest" after each argument.

For more about modules, see https://golang.org/ref/mod.

For more about specifying packages, see 'go help packages'.

This text describes the behavior of get using modules to manage source
code and dependencies. If instead the go command is running in GOPATH
mode, the details of get's flags and effects change, as does 'go help get'.
See 'go help gopath-get'.

See also: go build, go install, go clean, go mod.
[chenchen@localhost goproj]$ 

也就是说，go get 只用来下载普通的包，安装可执行程序，应该使用 go install。
```



## GOINSECURE, GOPRIVATE

```shell
# Go1.17 直接废弃了 -insecure 这个 flag, 必须使用 GOINSECURE 环境变量. 但这个环境变量不会禁用数据库校验和检查.
# 因此, 对于私有仓库, 如果没有提供 HTTPS, 应该配置 GOINSECURE, 指明哪些地址启用 INSECURE 模式, 同时配置 GOPRIVATE 环境变量, 避免数据库校验和检查.

$ go get -insecure github.com/labstack/echo/v4go get: -insecure flag is no longer supported; use GOINSECURE instead
```







## See Also

https://golang.org/ref/mod#go-get

https://golang.org/ref/mod#go-install

https://golang.org/ref/mod

