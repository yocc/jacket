# Return and handle an error

https://golang.google.cn/doc/tutorial/handle-errors



```shell
处理错误是可靠代码的基本特征. 在本节中, 您将添加一些代码来从 greetings 模块返回错误, 然后在调用者中处理它. 
注意: 本主题是从创建 Go 模块开始的多部分教程的一部分. 

1. 在 greetings/greetings.go 中, 添加下面突出显示的代码. 
如果您不知道该向谁打招呼, 则发送问候是没有意义的. 如果名称为空, 则向调用者返回错误. 将以下代码复制到 greetings.go 并保存文件. 
package greetings

import (
    "errors"
    "fmt"
)

// Hello returns a greeting for the named person. // Hello 为指定的人返回问候语
func Hello(name string) (string, error) {
    // If no name was given, return an error with a message. // 如果没有给出名字, 返回一个带有消息的错误
    if name == "" {
        return "", errors.New("empty name")
    }

    // If a name was received, return a value that embeds the name. // 如果接收到名称, 则返回一个嵌入名称的值
    // in a greeting message. // 在问候消息中
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message, nil
}
在此代码中, 您: 
. 更改函数, 使其返回两个值: 一个字符串和一个错误. 您的调用者将检查第二个值以查看是否发生错误.  ( 任何 Go 函数都可以返回多个值. 有关更多信息, 请参阅 Effective Go. ) 
. 导入 Go 标准库错误包, 以便您可以使用它的 errors.New 函数. 
. 添加 if 语句以检查无效请求( 名称应为空字符串) 并在请求无效时返回错误.  errors.New 函数返回一个错误, 其中包含您的消息. 
. 添加 nil( 意味着没有错误) 作为成功返回的第二个值. 这样, 调用者就可以看到函数成功了. 

2. 在您的 hello/hello.go 文件中, 处理现在由 Hello 函数返回的错误以及非错误值. 
将以下代码粘贴到 hello.go 中. 
package main

import (
    "fmt"
    "log"

    "example.com/greetings"
)

func main() {
    // Set properties of the predefined Logger, including // 设置预定义Logger的属性, 包括
    // the log entry prefix and a flag to disable printing // 日志条目前缀和禁用打印的标志
    // the time, source file, and line number. // 时间、源文件和行号. 
    log.SetPrefix("greetings: ")
    log.SetFlags(0)

    // Request a greeting message. // 请求问候消息.
    message, err := greetings.Hello("")
    // If an error was returned, print it to the console and // 如果返回错误, 则将其打印到控制台并
    // exit the program. // 退出程序. 
    if err != nil {
        log.Fatal(err)
    }

    // If no error was returned, print the returned message // 如果没有返回错误, 则打印返回的消息
    // to the console. // 到控制台.
    fmt.Println(message)
}
在此代码中, 您: 
. 将日志包配置为在其日志消息的开头打印命令名称( “greetings:”) , 不带时间戳或源文件信息. 
. 将两个 Hello 返回值( 包括错误) 分配给变量. 
. 将 Hello 参数从 Gladys 的名称更改为空字符串, 以便您可以尝试错误处理代码. 
. 寻找非零错误值. 在这种情况下继续下去是没有意义的. 
. 使用标准库的日志包中的函数输出错误信息. 如果出现错误, 则使用日志包的 Fatal 函数打印错误并停止程序. 

3. 在 hello 目录的命令行中, 运行 hello.go 以确认代码有效. 
现在您传入一个空名称, 您将收到错误消息. 
$ go run .
greetings: empty name
exit status 1

这是 Go 中常见的错误处理: 将错误作为值返回, 以便调用者可以检查它. 
接下来, 您将使用 Go 切片返回随机选择的问候语. 
```



