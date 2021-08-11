# Return greetings for multiple people

https://golang.google.cn/doc/tutorial/greetings-multiple-people





```shell
在您对模块代码所做的最后更改中, 您将添加对在一个请求中为多人获取问候的支持. 换句话说, 您将处理多值输入, 然后将该输入中的值与多值输出配对. 为此, 您需要将一组名称传递给一个可以为每个名称返回问候语的函数. 
注意: 本主题是从创建 Go 模块开始的多部分教程的一部分. 
但有一个障碍. 将 Hello 函数的参数从单个名称更改为一组名称会更改函数的签名. 如果您已经发布了 example.com/greetings 模块并且用户已经编写了调用 Hello 的代码, 那么该更改会破坏他们的程序. 
在这种情况下, 更好的选择是编写一个具有不同名称的新函数. 新函数将采用多个参数. 这保留了旧功能以实现向后兼容性. 

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

// Hellos returns a map that associates each of the named people // Hellos 返回一个映射, 该映射将每个命名的人关联起来
// with a greeting message. // 带有问候消息.
func Hellos(names []string) (map[string]string, error) {
    // A map to associate names with messages. // 将名称与消息关联的映射.
    messages := make(map[string]string)
    // Loop through the received slice of names, calling // 遍历接收到的名称切片, 调用
    // the Hello function to get a message for each name. // Hello 函数为每个名字获取一条消息.
    for _, name := range names {
        message, err := Hello(name)
        if err != nil {
            return nil, err
        }
        // In the map, associate the retrieved message with // 在地图中, 将检索到的消息与
        // the name. // 名字.
        messages[name] = message
    }
    return messages, nil
}

// Init sets initial values for variables used in the function. // Init 为函数中使用的变量设置初始值.
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

    // Return one of the message formats selected at random. // 返回随机选择的一种消息格式.
    return formats[rand.Intn(len(formats))]
}
在此代码中, 您: 
. 添加一个 Hellos 函数, 其参数是一个名称片段而不是单个名称. 此外, 您将其返回类型之一从字符串更改为映射, 以便您可以返回映射到问候消息的名称. 
. 让新的 Hellos 函数调用现有的 Hello 函数. 这有助于减少重复, 同时保留两个功能. 
. 创建消息映射以将每个接收到的名称(作为键)与生成的消息(作为值)相关联. 在 Go 中, 您使用以下语法初始化地图: make(map[key-type]value-type). 您让 Hellos 函数将此映射返回给调用者. 有关地图的更多信息, 请参阅 Go 博客上的 Go 地图实战. 
. 循环遍历您的函数收到的名称, 检查每个名称是否具有非空值, 然后将消息与每个名称关联. 在这个 for 循环中, range 返回两个值: 循环中当前项目的索引和项目值的副本. 您不需要索引, 因此您使用 Go 空白标识符(下划线)来忽略它. 有关更多信息, 请参阅 Effective Go 中的空白标识符. 

2. 在你的 hello/hello.go 调用代码中, 传递一段名称, 然后打印你返回的名称/消息映射的内容. 
在 hello.go 中, 更改您的代码, 使其看起来如下所示. 
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

    // A slice of names. // 一个名字片段.
    names := []string{"Gladys", "Samantha", "Darrin"}

    // Request greeting messages for the names. // 请求姓名的问候消息. 
    messages, err := greetings.Hellos(names)
    if err != nil {
        log.Fatal(err)
    }
    // If no error was returned, print the returned map of // 如果没有返回错误, 则打印返回的map
    // messages to the console. // 消息到控制台. 
    fmt.Println(messages)
}
通过这些更改, 您: 
. 创建一个 names 变量作为包含三个名称的切片类型. 
. 将 names 变量作为参数传递给 Hellos 函数. 

3. 在命令行中, 切换到包含 hello/hello.go 的目录, 然后使用 go run 确认代码有效. 
输出应该是将名称与消息相关联的地图的字符串表示形式, 如下所示: 
$ go run .
map[Darrin:Hail, Darrin! Well met! Gladys:Hi, Gladys. Welcome! Samantha:Hail, Samantha! Well met!]

本主题介绍了用于表示名称/值对的映射. 它还引入了通过为模块中的新功能或更改功能实现新功能来保持向后兼容性的想法. 有关向后兼容性的更多信息, 请参阅保持模块兼容. 
接下来, 您将使用内置的 Go 功能为您的代码创建单元测试. 
```

