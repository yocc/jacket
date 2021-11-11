

https://pkg.go.dev/errors



```shell
errors 包实现对错误的操作.
New() 创建 errors对象, 而 errors 对象的唯一内容是一段文本字符串.
Unwrap(), Is(), As() 用于解包一个已经被打包的 errors 对象
e.Unwrap() 返回非零错误 w, 那么我们称为 e包装了w
Unwrap() 解包一个已经被打包的错误. 如果参数的类型是一个 Unwrap() 方法, 那么就会调用一次. 否则, 返回零.

通过调用 fmt.Errorf 可以简单的创建一个打包过的错误对象, 并且用%w动词来错误参数.
fmt.Errorf("...%w ...", ..., err, ...)
errors.Unwrap(fmt.Errorf("...%w ...", ..., err, ...))
返回 err

Is 解包, 寻找第一个参数, 然后匹配第二个参数. 用于相等判断检查.
if errors.Is(err, fs.ErrExist)
类似
if err == fs.ErrExist
如果 err 打包了 fs.ErrExist 那么等式成立

As 解包, 寻找第一个参数是一个错误, 然后分配给第二个参数, 第二个参数必须是一个指针. 
如果成功, 执行这个分配指针, 并且返回 true. 否则, 返回 false. 
var perr *fs.PathError
if errors.As(err, &perr) {
		fmt.Println(perr.Path)
}
类似
if perr, ok := err.(*fs.PathError); ok {
		fmt.Println(perr.Path)
}
如果 err 打包 一个 *fs.PathError 那么共识将成功

例子 ¶
package main

import (
	"fmt"
	"time"
)

// MyError is an error implementation that includes a time and message.
type MyError struct {
	When time.Time
	What string
}

func (e MyError) Error() string {
	return fmt.Sprintf("%v: %v", e.When, e.What)
}

func oops() error {
	return MyError{
		time.Date(1989, 3, 15, 22, 30, 0, 0, time.UTC),
		"the file system has gone away",
	}
}

func main() {
	if err := oops(); err != nil {
		fmt.Println(err)
	}
}
Output:
1989-03-15 22:30:00 +0000 UTC: the file system has gone away

索引¶
func As(err error, target interface{}) bool
func Is(err, target error) bool
func New(文本字符串) error
func Unwrap(err error) error

例子 ¶
Package
As
Is
New
New(Errorf)

常量 ¶
这部分是空的。
```





概述 ¶
errors 包 实现了操作错误的功能.

New() 创建 errors，其唯一内容是一段文本消息.

Unwrap、Is 和 As 函数处理可能会包装其他错误的错误。如果错误类型具有方法，则错误会包装另一个错误

Unwrap() error
如果 e.Unwrap() 返回一个非零错误 w，那么我们说 e 包装了 w。

Unwrap 解包一个已经被包装的错误。如果它的参数类型有一个 Unwrap 方法，它会调用该方法一次。否则，它返回零。

创建包装错误的一种简单方法是调用 fmt.Errorf 并将 %w 动词应用于错误参数：

errors.Unwrap(fmt.Errorf("... %w ...", ..., err, ...))
返回错误。

Is按顺序展开其第一个参数以查找与第二个参数匹配的错误。它报告是否找到匹配项。它应该优先于简单的相等性检查：

if errors.Is(err, fs.ErrExist)
更可取

如果错误 == fs.ErrExist
因为如果 err 包装 fs.ErrExist，前者会成功。

As 依次展开它的第一个参数，查找可以分配给它的第二个参数的错误，该参数必须是一个指针。如果成功，则执行赋值并返回 true。否则，它返回 false。表格

var perr *fs.PathError
如果错误.As(err, &perr) {
fmt.Println(perr.Path)
}
更可取

如果 perr, ok := err.(*fs.PathError);行 {
fmt.Println(perr.Path)
}
因为如果 err 包装了 *fs.PathError，前者会成功。

例子 ¶
指数 ¶
func As(err error, target interface{}) bool
func Is(err, target error) bool
func New（文本字符串）错误
func Unwrap(err 错误) 错误
例子 ¶
包裹
作为
是
新的
新的 (Errorf)
常量 ¶
这部分是空的。

变量 ¶
这部分是空的。