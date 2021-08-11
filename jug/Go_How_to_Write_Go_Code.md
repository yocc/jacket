# 如何编写"去代码

## 目录

[TOC]
简介
代码组织
您的第一个程序
从模块导入包
从远程模块导入软件包
测试
接下来呢
寻求帮助

## 简介

本文档演示了模块内简单 Go 包的开发, 并介绍了 Go 工具、获取、构建和安装 Go 模块、包和命令的标准方法. https://golang.org/cmd/go/
注意: 本文档假设您正在使用 Go 1.13 或更晚, 并且未设置 GO111MODULE 环境变量. 如果您正在寻找此文档的较旧的预模块版本, 则将其存档在这里, https://golang.org/doc/gopath_code.html

## 代码组织

Go 程序被组织成包. 包是同一目录中收集的源文件, 这些文件汇编在一起. 同一包中的所有其他源文件可看到一个源文件中定义的函数、类型、变量  
和常数.
存储库包含一个或多个模块. 模块是一起发布的相关 Go 包的集合. Go 存储库通常只包含一个模块, 位于存储库的根部. 名为 go.mod 的文件声明模块路径: 模块内所有包的导入路径前缀. 该模块包含目录中的包, 其中包含其 go.mod 文件以及该目录的子目录, 直至包含另一个 go.mod 文件的下一个子目录 (如果有的话)
请注意, 在构建代码之前, 您不需要将代码发布到远程存储库. 模块可以本地定义, 而不属于存储库. 然而, 组织你的代码, 好像有一天你会发布它, 这是一个好习惯  
每个模块的路径不仅充当其包的导入路径前缀, 而且还指示 Go 命令应将其下载的位置. 例如, 为了下载模块 golang.org/x/tools, go 命令将查阅 https://golang.org/x/tools 指示的存储库 (此处更多说明, https://golang.org/cmd/go/#hdr-Relative_import_paths)
导入路径是用于导入包裹的字符串. 包的导入路径是其模块路径与模块内的子方向连接. 例如, github.com/google/go-cmp 的模块包含目录 cmp/中的包. 该包的导入路径 github.com/google/go-cmp/cmp. 标准库中的包没有模块路径前缀

## 您的第一个程序

要编译和运行一个简单的程序, 首先选择一个模块路径 (我们将使用 example.com/user/hello) , 并创建一个 go.mod 文件, 声明它:

```
$ mkdir hello # Alternatively, clone it if it already exists in version control.
$ cd hello
$ go mod init example.com/user/hello
go: creating new go.mod: module example.com/user/hello
$ cat go.mod
module example.com/user/hello

go 1.16
$
```

Go 源文件中的第一个语句必须是包名. 可执行命令必须始终使用 main 包

接下来, 创建一个名为 Hello.go 的文件, 该目录包含以下 Go 代码:

```
package main

import "fmt"

func main() {
	fmt.Println("Hello, world.")
}
```

现在, 您可以使用 go 工具构建和安装该程序:

```
$ go install example.com/user/hello
$
```

此命令构建你好命令, 生成可执行二进制文件. 然后, 它将二进制文件安装为$HOME/go/bin/hello (或者, 在 Windows 下, %用户证明%=去\bin=hello.exe)

安装目录由 GOPATH 和 GOBIN 环境变量控制. 如果设置 GOBIN, 则将二进制文件安装到该目录中. 如果设置 GOPATH, 二进制文件将安装到 GOPATH 列表中第一个目录的 bin 子目录中. 否则, 二进制文件将安装到默认 GOPATH 的 bin 子标题 ($HOME/go 或 %USERPROFILE%\go) https://golang.org/cmd/go/#hdr-Environment_variables
您可以使用 go env 命令为未来 go 命令设置环境变量的默认值:

```
$ go env -w GOBIN=/somewhere/else/bin
$
```

要取消设置以前由 go env-w 设置的变量, 请使用"去 env-u:

```
$ go env -u GOBIN
$
```

在包含当前工作目录的模块上下文中应用"go install"等命令. 如果工作目录不在 example.com/user/hello 模块内, 则去安装可能会失败
为了方便起见, go 命令接受相对于工作目录的路径, 如果没有给出其他路径, 则会默认当前工作目录中的包. 因此, 在我们的工作目录中, 以下命令都等同:

```
$ go install example.com/user/hello
```

```
$ go install .
```

```
$ go install
```

接下来, 让我们运行程序, 以确保它的工作原理. 为了增加便利性, 我们将在 PATH 中添加安装目录, 以便于运行二进制文件:

```
# Windows users should consult https://github.com/golang/go/wiki/SettingGOPATH
# for setting %PATH%.
$ export PATH=$PATH:$(dirname $(go list -f '{{.Target}}' .))
$ hello
Hello, world.
$
```

如果您使用的是源控制系统, 那么现在是初始化存储库、添加文件并提交第一次更改的好时机. 同样, 此步骤是可选的: 您不需要使用源控制来编写 Go 代码

```
$ git init
Initialized empty Git repository in /home/user/hello/.git/
$ git add go.mod hello.go
$ git commit -m "initial commit"
[master (root-commit) 0b4507d] initial commit
 1 file changed, 7 insertion(+)
 create mode 100644 go.mod hello.go
$
```

Go 命令通过请求相应的 HTTPS URL 并读取嵌入在 HTML 响应中的元数据来定位包含给定模块路径的存储库 (参见 go 帮助导入路径 https://golang.org/cmd/go/#hdr-Remote_import_paths) . 许多托管服务已经为包含 Go 代码的存储库提供了元数据, 因此, 让您的模块可供其他人使用的最简单方法通常是使其模块路径与存储库的 URL 相匹配

## 从模块导入包

让我们写一个更多的串包, 并使用它从你好程序. 首先, 为名为 "$HOME/hello/morestrings" 的软件包创建一个目录, 然后创建一个名为 reverse.go 的文件.

```
// Package morestrings implements additional functions to manipulate UTF-8
// encoded strings, beyond what is provided in the standard "strings" package.
package morestrings

// ReverseRunes returns its argument string reversed rune-wise left to right.
func ReverseRunes(s string) string {
	r := []rune(s)
	for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
		r[i], r[j] = r[j], r[i]
	}
	return string(r)
}
```

因为我们的反向润滑功能从大写字母开始, 所以它是 exported 的 https://golang.org/ref/spec#Exported_identifiers, 可用于其他进口我们更多串包的包装
让我们测试一下, 包编译与去构建:

```
$ cd $HOME/hello/morestrings
$ go build
$
```

这不会生成输出文件. 相反, 它会将编译的包保存在本地生成缓存中.
在确认更多串包生成后, 让我们从 Hello 程序中使用它. 为此, 请修改您的原始 $HOME/hello/hello.go 去使用更多的弦包:

```
package main

import (
	"fmt"

	"example.com/user/hello/morestrings"
)

func main() {
	fmt.Println(morestrings.ReverseRunes("!oG ,olleH"))
}
```

安装 hello 程序:

```
$ go install example.com/user/hello
```

运行新版本的程序, 您应该会看到一个新的反向消息:

```
$ hello
Hello, Go!
```

## 从远程模块导入软件包

导入路径可以描述如何使用修订控制系统 (如 Git 或 Mercurial) 获取包源代码. Go 工具使用此属性自动从远程存储库取取包裹. 例如, 在程序中使用 github.com/google/go-cmp/cmp:

```
package main

import (
	"fmt"

	"example.com/user/hello/morestrings"
	"github.com/google/go-cmp/cmp"
)

func main() {
	fmt.Println(morestrings.ReverseRunes("!oG ,olleH"))
	fmt.Println(cmp.Diff("Hello World", "Hello Go"))
}
```

现在, 您依赖于外部模块, 您需要下载该模块并在 go.mod 文件中记录其版本. go mod 整洁命令增加了导入包缺少的模块要求, 并删除了不再使用的模块  
的要求.

```
$ go mod tidy
go: finding module for package github.com/google/go-cmp/cmp
go: found github.com/google/go-cmp/cmp in github.com/google/go-cmp v0.5.4
$ go install example.com/user/hello
$ hello
Hello, Go!
  string(
- 	"Hello World",
+ 	"Hello Go",
  )
$ cat go.mod
module example.com/user/hello

go 1.16

require github.com/google/go-cmp v0.5.4
$
```

模块依赖性自动下载到 GOPATH 环境变量指示目录的 pkg/mod 子目录. 特定版本的模块的下载内容在所有其他需要该版本的模块之间共享, 因此 go 命令将这些文件和目录标记为仅读取. 要删除所有下载的模块, 您可以通过 - modcache 标志进行清洁:

```
$ go clean -modcache
$
```

## 测试

Go 有一个轻量级的测试框架, 由 go 测试命令和测试包组成  
.

您通过创建一个以\_test.go 结尾的名称的文件来编写测试, 该文件包含名为 TestXXX 的功能, 并带有签名 func (t \*test) . T) . 测试框架运行每个此类功能: 如果该函数调用故障函数 (如 t.错误或 t.失败) , 则测试被视为已失败  
.

通过创建包含以下 Go 代码的文件$HOME/你好/更多串/reverse_test.go, 将测试添加到更多串包  
中.

```
package morestrings

import "testing"

func TestReverseRunes(t *testing.T) {
	cases := []struct {
		in, want string
	}{
		{"Hello, world", "dlrow ,olleH"},
		{"Hello, 世界", "界世 ,olleH"},
		{"", ""},
	}
	for _, c := range cases {
		got := ReverseRunes(c.in)
		if got != c.want {
			t.Errorf("ReverseRunes(%q) == %q, want %q", c.in, got, c.want)
		}
	}
}
```

然后运行测试与去测试:

```
$ go test
PASS
ok  	example.com/user/morestrings 0.165s
$
```

运行去帮助测试 https://golang.org/cmd/go/#hdr-Test_packages, 并查看测试包文档的更多详细信息 https://golang.org/pkg/testing/

## 接下来呢

- 订阅 golang 宣布的邮件列表, 以便在发布新的稳定版本 Go 时通知, https://groups.google.com/group/golang-announce
- 请参阅有效 Go, 了解有关编写清晰、惯用的 Go 代码的提示. https://golang.org/doc/effective_go.html
- 参加一次" A Tour of Go", 学习正确的语言
- 访问文档页面, 了解一组关于 Go 语言及其库和工具的深入文章, https://golang.org/doc/#articles

## 寻求帮助

- 有关实时帮助, 请在社区运行的 gophers Slack 服务器中询问乐于助人的妖精https://gophers.slack.com/messages/general/ (在此处获取邀请 https://invite.slack.golangbridge.org/)
- 讨论围棋语言的官方邮件列表是"去坚果", https://groups.google.com/group/golang-nuts
- 使用 Go 问题跟踪器报告错误. https://golang.org/issue

## A Tour of Go, Go 之旅

"Go"互动介绍分为三个部分.

- 第一部分包括基本语法和数据结构;
- 第二个讨论方法和界面;
- 第三个介绍了围棋的并发原始;
  每个部分以几个练习结束, 这样你就可以练习你学到的东西. 您可以在线参观或在本地安装:

```
$ go get golang.org/x/tour
```

这将将旅游二进制文件放在工作空间的 Bin 目录中。

## 参考

https://golang.org/doc/code.html
