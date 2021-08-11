# Return a random greeting

https://golang.google.cn/doc/tutorial/random-greeting





```shell
在本节中, 您将更改代码, 以便不是每次都返回一个问候语, 而是返回多个预定义的问候语消息之一. 
注意: 本主题是从创建 Go 模块开始的多部分教程的一部分. 
为此, 您将使用 Go 切片. 切片类似于数组, 不同之处在于它的大小会随着您添加和删除项目而动态变化.  slice 是 Go 最有用的类型之一. 
您将添加一小部分来包含三个问候消息, 然后让您的代码随机返回其中一个消息. 有关切片的更多信息, 请参阅 Go 博客中的 Go 切片. 

1. 在 greetings/greetings.go 中, 更改您的代码, 使其看起来如下所示. 
package greetings

import (
    "errors"
    "fmt"
    "math/rand"
    "time"
)

// Hello returns a greeting for the named person. // Hello 为指定的人返回问候语.
func Hello(name string) (string, error) {
    // If no name was given, return an error with a message. // 如果没有给出名字, 返回一个带有消息的错误.
    if name == "" {
        return name, errors.New("empty name")
    }
    // Create a message using a random format. // 使用随机格式创建消息.
    message := fmt.Sprintf(randomFormat(), name)
    return message, nil
}

// init sets initial values for variables used in the function. // init 为函数中使用的变量设置初始值.
func init() {
    rand.Seed(time.Now().UnixNano())
}

// randomFormat returns one of a set of greeting messages. The returned // randomFormat 返回一组问候消息中的一个. 返回的
// message is selected at random. // 消息是随机选择的.
func randomFormat() string {
    // A slice of message formats. // 消息格式的切片. 
    formats := []string{
        "Hi, %v. Welcome!",
        "Great to see you, %v!",
        "Hail, %v! Well met!",
    }

    // Return a randomly selected message format by specifying // 通过指定返回随机选择的消息格式
    // a random index for the slice of formats. // 格式切片的随机索引.
    return formats[rand.Intn(len(formats))]
}
在此代码中, 您: 
. 添加一个 randomFormat 函数, 该函数为问候消息返回随机选择的格式. 请注意,  randomFormat 以小写字母开头, 使其只能由其自己的包中的代码访问(换句话说, 它不会被导出). 
. 在 randomFormat 中, 声明具有三种消息格式的格式切片. 声明切片时, 您可以在括号中省略其大小, 如下所示: []string. 这告诉 Go 切片底层数组的大小可以动态更改. 
. 使用 math/rand 包生成一个随机数, 用于从切片中选择一个项目. 
. 添加一个 init 函数以使用当前时间为 rand 包做种子.  Go 在程序启动时自动执行 init 函数, 在全局变量初始化之后. 有关 init 函数的更多信息, 请参阅 Effective Go. 
. 在 Hello 中, 调用 randomFormat 函数获取您将返回的消息的格式, 然后将格式和名称值一起使用来创建消息. 
. 像以前一样返回消息(或错误). 

2. 在 hello/hello.go 中, 更改您的代码, 使其看起来如下所示. 
您只是将 Gladys 的名字(或不同的名字, 如果您喜欢)作为参数添加到 hello.go 中的 Hello 函数调用中. 
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
    message, err := greetings.Hello("Gladys")
    // If an error was returned, print it to the console and // 如果返回错误, 则将其打印到控制台并
    // exit the program. // 退出程序. 
    if err != nil {
        log.Fatal(err)
    }

    // If no error was returned, print the returned message // 如果没有返回错误, 则打印返回的消息
    // to the console. // 到控制台. 
    fmt.Println(message)
}

3. 在命令行的 hello 目录中, 运行 hello.go 以确认代码有效. 多次运行它, 注意到问候语发生了变化. 
$ go run .
Great to see you, Gladys!

$ go run .
Hi, Gladys. Welcome!

$ go run .
Hail, Gladys! Well met!

接下来, 您将使用切片来问候多个人. 
```

