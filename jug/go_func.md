# func

----

[toc]

## Overview

### 闭包

https://blog.csdn.net/Sunny_Future/article/details/118742729

闭包: 指的是引用了自由变量(未绑定到特定对象的变量, 通常在函数外定义)的函数, 被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的上下文环境也不会被释放（比如传递到其他函数或对象中）。简单来说，「闭」的意思是「封闭外部状态」，即使外部状态已经失效，闭包内部依然保留了一份从外部引用的变量
闭包: 指的是一个函数和与其相关的引用环境组合而成的实体。简单来说，`闭包=函数+引用环境`.

> 我理解: 
>
> 闭包是函数, 不同于常规函数的是, 函数内部的用了函数外的资源. 闭包 = 用了外部资源的函数. 
>
> 一些列连续方法, 彼此相关联, 有顺序, 干完一个干下一个, 干下一个时需要上一个结果





### 函数签名, 函数类型, 函数变量, 函数类型的变量, 匿名函数

函数签名:

```go
'函数签名'
func(参数类型) 返回类型

'函数签名'就是'函数类型'
'函数签名就是一种特殊类型'
package main
import "fmt"
func add(a, b int) int {
    u := a + b
    fmt.Println(u)
    return u
}
func main() {
    var f func(int, int) int	// 用函数签名来声明变量 f, 等同于 'var f 类型', 'func(int, int) int' 等同于 '类型' 
    f = add
    f(2, 1)
    fmt.Println("%T", f)
}
返回:
3
%T 0x47dfe0
```

函数类型:

```go
'函数本身就是一个特殊类型'
package main
import "fmt"
func add(a, b int) int {		// add() 函数本身
    u := a + b
    fmt.Println(u)
    return u
}
func main() {
    f := add								// 函数本身就是一种特定类型, 而函数名就是这个函数及其类型的合体统一代表
    f(2, 1) 
    fmt.Println("%T", f)
}
返回:
3
%T 0x47dfe0
```

函数变量 和 函数类型变量:

```go
'函数签名'就是'函数类型'
type newType 函数签名
type newType func(参数类型) 返回类型
...
var newVar newType

'函数变量', 变量的值是函数, 即, 指向函数代码起始位置的指针. 代表的是一个具体函数. 
例如: 
f = add // f 为函数变量 

'函数类型变量', 函数的值是一类特定函数类型(函数签名). 代表一类函数类型. 
例如: 
type k func(int, int) int
f = k		// f 为函数类型变量


例如:
package main
import "fmt"
func add(a, b int) int {
    u := a + b
    fmt.Println(u)
    return u
}
type k func(int, int) int			// 声明一个新函数类型叫 k
func main() {
    var f k										// f 为'函数类型变量', 函数类型这里是 k, 即 'func(int, int) int', 即'函数签名'
    f = add										// 给函数变量 f 赋值
    f(2, 1)										// 通过函数变量 f 调用函数
  	fmt.Println("%T", f)			// f 的值是指向函数代码起始位置的指针, 所以说它也是引用类型. 函数变量实际就等于给实体函数取了个别名
    add(2, 1)									// 直接调用函数
  	fmt.Println("%T", add)
}
返回:
3
%T 0x47dfe0				// f 函数变量
3
%T 0x47dfe0				// add 实体函数
```

匿名函数:

```go
'匿名函数', 是没有名字的函数.需要使用的时候再定义. 
1. 可以直接将匿名函数赋值给函数变量; 2. 还可以做实参给函数参数赋值以及做返回值; 3. 甚至可以直接调用（声明及调用）

1. '匿名函数' 赋值给 '函数变量', 相当于给'匿名函数'起了一个名字
var add = func(a, b int) int {
		u := a + b
  	fmt.Println(u)
    return u
}

2. '匿名函数' 作为 '函数实参' 和 '函数返回值'
func allCalc(f func(int, int) int, a, b int) int {	// 定义函数 allCalc, 第一个形參是 f, 类型是函数, 用函数签名表示
  	return f(a, b)
}
b := allCalc(func(a, b int) int {			// 匿名函数作为第一个实参, 匿名函数是一个实际普通函数的样子
		return a + b
}, 2, 1)

3. '匿名函数' 声明时立即直接调用.
d := func(x, y int) int {			// 一个实际匿名函数整体作为函数名, 后接实参列表
		return x + y
}(2, 1)					// 实参列表
```

另外的例子:

```go
// 函数签名, 函数类型, 函数变量, 匿名函数
函数签名: 函数定义的首行去掉函数名、参数名和花括号，也就是只留下定义关键字 func、小括号以及类型相关的部分.
函数类型又叫函数签名.
函数可以像其他类型一样声明变量, 称之为函数变量.
函数在特定场景也可以没有名字, 称之为匿名函数.

函数签名:
'func(参数类型) 返回类型'
函数类型:
'type 新类型名字 函数签名'

举例:
package main
import (
		"fmt"
)
func add(a, b int) int {
		return a + b
}
func sub(x int, y int) int {
		return x - y
}
func main() {
	  fmt.Printf("函数 add 的类型: %T\n", add)
    fmt.Printf("函数 add 的类型: %T\n", add)
}
函数 add 的类型: func(int, int) int
函数 sub 的类型: func(int, int) int
从打印输出的结果可以看出, 虽然 add 和 sub 两个函数的名称不同、参数定义格式不同、函数体内代码不同, 但是它们的类型是相同的.
都是 func(int, int) int 这个函数类型.
函数类型 = 函数定义关键字func、参数括号、参数数量和类型、返回值的数量和类型 = 'func(参数类型) 返回类型'

既然函数也是类型, 那么就可以使用 type 关键字定义函数类型. 
函数类型变量是可以作为函数的参数和返回值的. 看下面的示例代码:

package main
import (
		"fmt"
)
func add(a, b int) int {				// 声明 add 函数, 类型为 func(int, int) int
		return a + b
}
func sub(x int, y int) int {		// 声明 sub 函数, 类型为 func(int, int) int
		return x - y
}
type Calc func(int, int) int		// 声明 Calc 类型变量(新类型名称), 代表函数类型 func(int, int) int
func allCalc(f Calc, x, y int) int {
  	return f(x, y)
}
func main() {
  	fmt.Printf("参数为 add 时: %v\n", allCalc(add, 2, 1))
    fmt.Printf("参数为 sub 时: %v\n", allCalc(sub, 2, 1))
}
参数为 add 时: 3
参数为 sub 时: 1

函数类型也是引用类型, 它的指针指向函数代码的开始位置. 函数的作用域与公共变量一样, 在包内任何地方都可用, 如果要其他包可以使用, 需要将函数名首字母大写.

第16行, 自定义了一个新类型 Calc, 它是 func(int, int) int 这个函数类型的别名, 而上面 add、sub 两个函数的类型也都是着个类型, 所以 Calc 就等于是上面两个函数的函数类型.

第18~20行, 声明了一个函数 allCalc, 它的第一个参数 f 就是 Calc 类型，也就等于是一个 func(int, int) int 类型的函数, 叫什么名字无所谓, 函数体什么逻辑也无所谓. 只要是包含两个 int 类型的参数, 返回值是 int 类型的函数即可. 这样做的目的就是可以将函数体内逻辑不同, 但是参数及返回值相同的函数调入到这个函数内部执行, 得到不同的运算逻辑.

第19行, 就是在执行外部传入进来的具体函数, 具体执行的是哪个函数, 取决于调用的地方给赋值的是哪个函数.
第24行的调用赋值, 传入的是 add 函数, 那么第19行执行的就是 add 函数;
第25行的调用赋值, 传入的是 sub 函数, 那么第19行执行的就是 sub 函数.

```

```go
函数变量
实际上前面示例中的第18行的参数 f 就是函数变量。函数变量，就是代表一个函数的变量，其值是指向函数代码起始位置的指针，所以说它也是引用类型
package main
import (
		"fmt"
)
func add(a, b int) int {
		return a + b
}
func main() {
  f := add
  a := f(2, 1)
  b := add(2, 1)
  
  fmt.Println("函数变量调用结果: ", a)
  fmt.Println("函数变量调用结果: ", b)
}
上述代码编译执行结果如下:
函数变量调用结果: 3
函数变量调用结果: 3

通过示例可以看出，函数变量实际就等于给实体函数取了个别名，然后使用该函数的时候就可以通过这个变量调用。因此示例中 a 和 b 的值是相同的，因为本质上是执行了同一个函数。
这里要特别注意，函数变量与函数类型变量是不同的，函数变量代表的是一个具体函数，而函数类型变量是代表一类函数类型，千万不要混淆

```

```go

匿名函数
  在 Go语言中按照名称来区分有两种函数，命名（有名）函数和匿名函数。匿名函数，顾名思义就是没有名字的函数。需要使用的时候再定义，可以直接将匿名函数赋值给函数变量，还可以做实参给函数参数赋值以及做返回值，甚至可以直接调用（声明及调用）。

package main
import (
	"fmt"
)
// 匿名函数直接赋值给函数变量
var add = func(a, b int) int {
	return a + b
}
// 声明一个函数, 参数 f 为函数类型 func(int, int) int
func allCalc(f func(int, int) int, a, b int) int {
  return f(a, b)
} 
// 声明一个函数, 匿名函数作为返回值, 根据sign不同, 返回不同的函数
func selectCalc(sing string) func(int, int) int {
  switch sing {
    default:
    	return nil
    case "+":
      return func(a, b int) int {
        return a + b
      }
    case "-":
      return func(a, b int) int {
        return a + b
      }
  }
}
func main() {
  a := add(2, 1)
  b :=
}

```





## FAQ





## See Also

https://blog.csdn.net/yyykj/article/details/127332253







































