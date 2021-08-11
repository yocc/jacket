# Tutorial: Create a Go module

https://golang.google.cn/doc/tutorial/create-module



先决条件
开始一个其他人可以使用的模块



```shell
这是教程的第一部分, 介绍了 Go 语言的一些基本特性.
如果您刚开始使用 Go, 请务必查看教程: Go 入门, 其中介绍了 go 命令、Go 模块和非常简单的 Go 代码.

在本教程中, 您将创建两个模块.
第一个是旨在由其他库或应用程序导入的库. 第二个是调用者应用程序, 它将使用第一个.

本教程的序列包括七个简短的主题, 每个主题都说明了语言的不同部分. 
1. 创建一个模块 —— 用你可以从另一个模块调用的函数编写一个小模块.
2. 从另一个模块调用您的代码 —— 导入并使用您的新模块.
3. 返回并处理错误 —— 添加简单的错误处理.
4. 返回一个随机的问候 —— 处理切片中的数据(Go 的动态大小的数组).
5. 为多人返回问候 —— 将键/值对存储在映射中.
6. 添加测试 —— 使用 Go 的内置单元测试功能来测试您的代码.
7. 编译并安装应用程序 —— 在本地编译并安装您的代码.
注意：有关其他教程，请参阅教程.
```



### 先决条件
```shell
. 有一定的编程经验. 这里的代码非常简单, 但它有助于了解有关函数、循环和数组的一些知识.
. 一种编辑代码的工具. 您拥有的任何文本编辑器都可以正常工作. 大多数文本编辑器都对 Go 有很好的支持. 最受欢迎的是 VSCode(免费)、GoLand(付费)和 Vim(免费).
. 命令终端. Go 适用于 Linux 和 Mac 上的任何终端，以及 Windows 中的 PowerShell 或 cmd
```



### 开始一个其他人可以使用的模块

```shell
首先创建一个 Go 模块. 
在一个模块中, 您可以为一组离散且有用的功能收集成一个或多个相关包. (何谓模块)
例如, 您可以创建一个包含具有财务分析功能的包的模块, 以便其他编写财务应用程序的人可以使用您的工作. 有关开发模块的更多信息，请参阅开发和发布模块.
Go 代码被分组到包中. 包被分组到模块中. 您的模块指定运行代码所需的依赖项, 包括 Go 版本及其所需的其他模块集.
当您在模块中添加或改进功能时, 您发布模块的新版本. 编写调用模块中函数的代码的开发人员可以导入模块的更新包并在将其投入生产使用之前使用新版本进行测试.

1. 打开命令提示符并 cd 到您的 home 目录
在 Linux 或 Mac 上:
cd
在 Windows 上:
cd %HOMEPATH%

2. 为您的 Go 模块源代码创建一个 greetings 目录.
例如, 从您的主目录使用以下命令:
mkdir greetings
cd greetings

3. 使用 go mod init 命令开始你的模块.
运行 go mod init 命令, 给它你的模块路径 —— 在这里, 使用 example.com/greetings. 如果您发布一个模块, 这必须是 Go 工具可以从中下载您的模块的路径. 那将是您代码的存储库. (如何创建 go.mod 文件)
$ go mod init example.com/greetings
go：creating new go.mod: module example.com/greetings
go mod init 命令创建一个 go.mod 文件来跟踪代码的依赖项. 到目前为止, 该文件仅包含您的模块名称和您的代码支持的 Go 版本. 但是当您添加依赖项时, go.mod 文件将列出您的代码所依赖的版本. 这使构建可重现并让您直接控制要使用的模块版本. (go.mod 文件的作用)

4. 在您的文本编辑器中, 创建一个用于编写代码的文件并将其命名为 greetings.go.
5. 将以下代码粘贴到您的 greetings.go 文件中并保存该文件.

package greetings
import "fmt"

// Hello returns a greeting for the named person. // 为指定的人返回问候语
func Hello(name string) string {
    // Return a greeting that embeds the name in a message. // // 返回在消息中嵌入名称的问候语
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}

这是您的模块的第一个代码. 它向任何要求的呼叫者返回问候语. 您将在下一步中编写调用此函数的代码.
在此代码中, 您:
. 声明一个 greetings package 来收集相关函数.
. 实现一个 Hello 函数来返回问候语.
该函数接受一个类型为字符串的名称参数. 该函数还返回一个字符串. 在 Go 中, 名称以大写字母开头的函数可以被不在同一个包中的函数调用. (public) 这在 Go 中称为导出名称. 有关导出名称的更多信息, 请参阅 Go 导览中的导出名称.

func Hello(name string) string
Hello 函数名
name string 参数类型
string 返回类型

. 声明一个消息变量来保存您的问候语.
在 Go 中, := 运算符是在一行中声明和初始化变量的快捷方式(Go 使用右侧的值来确定变量的类型). 从长远来看, 您可能会将其写为:

var message string
message = fmt.Sprintf("Hi, %v. Welcome!", name)

. 使用 fmt package 的 Sprintf 函数创建问候消息. 第一个参数是格式字符串, Sprintf 用 name 参数的值替换 %v 格式动词. 插入 name 参数的值完成问候文本.
. 将格式化的问候文本返回给调用者.
在下一步中, 您将从另一个模块调用此函数.
```



