# Add a test

https://golang.google.cn/doc/tutorial/add-a-test





```shell
既然您已经将代码放到了一个稳定的位置(顺便说一句, 做得很好), 请添加一个测试. 在开发期间测试您的代码可能会暴露在您进行更改时发现的错误. 在本主题中, 您将为 Hello 函数添加一个测试. 
注意: 本主题是从创建 Go 模块开始的多部分教程的一部分. 
Go 对单元测试的内置支持使您可以更轻松地进行测试. 具体来说, 使用命名约定、Go 的测试包和 go test 命令, 您可以快速编写和执行测试. 

1. 在 greetings 目录中, 创建一个名为 greetings_test.go 的文件. 
以 _test.go 结尾的文件名告诉 go test 命令该文件包含测试函数. 

2. 在 greetings_test.go 中, 粘贴以下代码并保存文件. 
package greetings

import (
    "testing"
    "regexp"
)

// TestHelloName calls greetings.Hello with a name, checking // TestHelloName 用名字调用 greetings.Hello, 检查
// for a valid return value. // 对于有效的返回值.
func TestHelloName(t *testing.T) {
    name := "Gladys"
    want := regexp.MustCompile(`\b`+name+`\b`)
    msg, err := Hello("Gladys")
    if !want.MatchString(msg) || err != nil {
        t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`, msg, err, want)
    }
}

// TestHelloEmpty calls greetings.Hello with an empty string, // TestHelloEmpty 使用空字符串调用 greetings.Hello,
// checking for an error. // 检查错误.
func TestHelloEmpty(t *testing.T) {
    msg, err := Hello("")
    if msg != "" || err == nil {
        t.Fatalf(`Hello("") = %q, %v, want "", error`, msg, err)
    }
}
在此代码中, 您: 
. 在与您正在测试的代码相同的包中实现测试功能. 
. 创建两个测试函数来测试 greetings.Hello 函数. 测试函数名称的形式为 TestName, 其中 Name 表示特定测试的一些内容. 此外, 测试函数将指向测试包的 testing.T 类型的指针作为参数. 您可以使用此参数的方法来报告和记录您的测试. 
. 实现两个测试: 
.. TestHelloName 调用 Hello 函数, 传递一个名称值, 该函数应该能够使用该值返回有效的响应消息. 如果调用返回错误或意外响应消息(不包括您传入的名称), 则使用 t 参数的 Fatalf 方法将消息打印到控制台并结束执行. 
.. TestHelloEmpty 使用空字符串调用 Hello 函数. 此测试旨在确认您的错误处理工作正常. 如果调用返回非空字符串或没有错误, 则使用 t 参数的 Fatalf 方法将消息打印到控制台并结束执行. 

3. 在greetings目录下的命令行, 运行go test命令执行测试. 
go test 命令在测试文件(名称以 _test.go 结尾)中执行测试函数(名称以 Test 开头). 您可以添加 -v 标志以获得列出所有测试及其结果的详细输出. 
测试应该通过. 
$ go test
PASS
ok      example.com/greetings   0.364s

$ go test -v
=== RUN   TestHelloName
--- PASS: TestHelloName (0.00s)
=== RUN   TestHelloEmpty
--- PASS: TestHelloEmpty (0.00s)
PASS
ok      example.com/greetings   0.372s

4. 中断 greetings.Hello 函数以查看失败的测试. 
TestHelloName 测试函数检查您指定为 Hello 函数参数的名称的返回值. 要查看失败的测试结果, 请更改 greetings.Hello 函数, 使其不再包含名称. 
在 greetings/greetings.go 中, 粘贴以下代码代替 Hello 函数. 请注意, 突出显示的行会更改函数返回的值, 就好像 name 参数已被意外删除一样. 
// Hello returns a greeting for the named person. // Hello 为指定的人返回问候语.
func Hello(name string) (string, error) {
    // If no name was given, return an error with a message. // 如果没有给出名字, 返回一个带有消息的错误. 
    if name == "" {
        return name, errors.New("empty name")
    }
    // Create a message using a random format. // 使用随机格式创建消息. 
    // message := fmt.Sprintf(randomFormat(), name) // 消息 := fmt.Sprintf(randomFormat(), name)
    message := fmt.Sprint(randomFormat())
    return message, nil
}

5. 在greetings目录下的命令行, 运行go test执行测试. 
这一次, 不带 -v 标志运行 go test. 输出将仅包含失败的测试的结果, 这在您进行大量测试时非常有用.  TestHelloName 测试应该失败——TestHelloEmpty 仍然通过. 
$ go test
--- FAIL: TestHelloName (0.00s)
    greetings_test.go:15: Hello("Gladys") = "Hail, %v! Well met!", <nil>, want match for `\bGladys\b`, nil
FAIL
exit status 1
FAIL    example.com/greetings   0.182s

在下一个(也是最后一个)主题中, 您将看到如何编译和安装代码以在本地运行它. 
```









