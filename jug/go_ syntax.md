# go syntax

---



[toc]



## 变量声明和初始化

```go
1. 长变量
声明并且赋值一个变量
无使用限制, 全局和函数内均可使用
var 变量名 类型 = 表达式
var go string = "helllo"

2. 短变量
声明并且赋值一个变量
使用限制: 不能在函数外面使用, 即不能用来声明全局变量
go := "hello"
```



## 常量 const

```go
const uri = "mongodb://10.211.21.19:27018/?replicaSet=Sport"
mongodb://mongodb0.example.com:27017,mongodb1.example.com:27017,mongodb2.example.com:27017/?replicaSet=myRepl
mongodb://myDBReader:D1fficultP%40ssw0rd@mongodb0.example.com:27017,mongodb1.example.com:27017,mongodb2.example.com:27017/?authSource=admin&replicaSet=myRepl
```



## if

```go
// 不存在三元运算符
if 布尔表达式 {													// 没有小括号
   /* 在布尔表达式为 true 时执行 */
}

if i := 1; i == 1 {											// 比较操作符, ==, !=
}

if number := 4; 100 > number {
    number += 3
} else if 100 < number {
    number -= 2
} else {
    fmt.Println("OK!")
}
```



## for

```go
for init; condition; post {
	  // init: 一般为赋值表达式, 给控制变量赋初值;
	  // condition: 关系表达式或逻辑表达式, 循环控制条件;
  	// post: 一般为赋值表达式, 给控制变量增量或减量;
}

/*
for语句执行过程如下:
1, 先对表达式 i 赋初值;
2, 判别赋值表达式 init 是否满足给定条件, 若其值为真, 满足循环条件, 则执行循环体内语句, 然后执行 post, 
	 进入第二次循环, 再判别 condition; 否则判断 condition 的值为假, 不满足条件, 就终止for循环, 执行循环体外语句.
*/
for i := 0; i <= 10; i++ {
  sum += i
}

sum := 1
for ; sum <= 10; {								// init 和 post 参数是可选的, 我们可以直接省略它, 类似 While 语句
  	sum += sum
}

var i int
for ; ; i++ {											// init 和 condition 是可选参数
    if i > 10 {
        break
    }
}

sum := 0
for {															// init, condition 和 post 都是可选参数
  	sum++ // 无限循环下去
}

// for 循环的 range 格式可以对 slice, map, 数组, 字符串等进行迭代循环
for key, value := range oldMap {
    newMap[key] = value
}

strings := []string{"google", "runoob"}
for i, s := range strings {
  	fmt.Println(i, s)
}

numbers := [6]int{1, 2, 3, 5}
for i,x:= range numbers {
  	fmt.Printf("第 %d 位 x 的值 = %d\n", i,x)
} 

// 循环被 break、goto、return、panic 等语句强制退出
```



## 变量和类型

```go
// 变量作用域
. 函数外定义的变量称为全局变量
. 函数内定义的变量称为局部变量, 全局变量与局部变量名称可以相同, 但是函数内的局部变量会被优先考虑
. 函数定义中的变量称为形式参数, 形式参数会作为函数的局部变量来使用

// 声明
var 变量名字 类型 = 表达式
其中 "类型" 或 "= 表达式" 两个部分可以省略其中的一个.
var num int = 10
var num int						// 意思是 var num int = 0, 根据初始化表达式来推导类型信息, 所以int类型默认值初始化为0
var num = 10					// 意思是 var num int = 10

// 整型
int 和 uint 都是指 CPU 的位数(bits)
int8 和 uint8 都是特指固定8位(8 bits), 最大 int64, int3 表示占 3bit 的 int 类型
rune 和 int32 别名, 常用于表示 unicode 码点
byte 和 int8 别名, 常用于表示原始字节数据
uintptr 无特定bits大小的整型, 但足以容纳指针, 常用于底层编程, 特别是Go语言和C语言函数库或操作系统接口相交互的地方

// 有符号, 无符号:
有符号整数采用 2 的补码形式表示, 也就是最高 bit 位用作表示符号位. 
一个 n bit 的有 符号数的值域是从 -2^{n-1} 到 2^{n-1}−1
例如, int8类型整数的值域是从-128 到 127, 而uint8类型整数的值域是从0到255

// 浮点数, 虚数
math 包
float32 和 float64 算术规范由IEEE754浮点数国际标准定义
complex64 和 complex128 分别对应 float32 和 float64 两种浮点数精度, real() 和 imag() 函数分别返回复数的实部和虚部
z := x + yi
x = real(z)
y = imag(z)

// 字符串
一个字符串是一个不可改变的字节序列
取字符串中某个字节的地址是非法的, ＆s [i]无效
len(s) 字节长度, 子字符串 s[i:j], 连接字符串 '+', 比较==是逐字节比较
双引号就是 PHP 的双引号, 反引号是 PHP 的单引号
unicode包, 提供了诸多处理 rune 字符相关功能的函数
unicode/utf8包, 则提供了用于 rune 字符序列的 UTF8 编码和解码的功能
标准库中有四个包对字符串处理尤为重要: bytes、strings、strconv和unicode包
strings包 提供了许多如字符串的查询、替换、比较、截断、拆分和合并等功能.
bytes包 也提供了很多类似功能的函数, 但是针对和字符串有着相同结构的[]byte类型. 因为字符串是只读的, 因此逐步构建字符串会导致很多分配和复制. 在这种情况下, 使用 bytes.Buffer 类型将会更有效.
strconv包 提供了布尔型、整型数、浮点数和对应字符串的相互转换, 还提供了双引号转义相关的转换.
unicode包 提供了IsDigit、IsLetter、IsUpper和IsLower等类似功能.
strconv包 提供字符串和数值之间的转换.
一个字符串是包含的只读字节数组, 一旦创建, 是不可变的. 相比之下, 一个字节slice的元素则可以自由地修改.

// const
编译期计算
const b string = "abc"
const (
        e  = 2.71828182845904523536028747135266249775724709369995957496696763
        pi = 3.14159265358979323846264338327950288419716939937510582097494459
)
const IPv4Len = 4

// 零值
false
0, int, uint, int8, uint32, byte, rune, float32, float64, complex64, complex128
"", string
nil, uintptr, array, slice, map, channel, interface, func
```



## array

```go
// 数组: 是具有相同唯一类型的一组已编号且长度固定的数据项序列. (每个元素必须类型相同, 数组长度固定)

// 声明, 初始化
var 数组名称 [大小] 类型
var 数组名称 = [大小]类型{元素列表}
var balance [10] float32                            		// 声明, 定义了数组 balance 长度为 10 类型为 float32
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}   // 初始化, 
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}      // 声明 + 初始化, 省了 var, 同变量
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}	// 长度不定，用 ... 代替, 编译器会自行推断数组的长度
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance := [5]float32{1:2.0, 3:7.0}											// 指定下标来初始化元素
row1 := []int{1, 2, 3}																	// 同 ... , 切片?
values := [][]int{}																			// 

// 访问
变量名[下标]
balance[4] = 50.0
var salary float32 = balance[9]

// 多维数组
var 数组名称 [尺寸][尺寸][尺寸] 类型
var threedim [5][10][4] int
var a = [5][2]int{{0,0}, {1,2}, {2,4}, {3,6},{4,8}}    // 5 行 2 列

// 多维数组访问
a[i][j]

// 向函数传递数组
声明形参为数组, 以下两种方式来声明:
. 形参设定数组大小, void myFunction(param [10]int) {}
. 形参未设定数组大小, void myFunction(param []int) {}
```



## slice

```go
// 切片(动态数组), 切片是对数组的抽象

// 切片声明
// 声明一个未指定大小的数组来定义切片. 切片不需要说明长度
var 切片名称 []类型		// 声明但没有分配内存
var slice1 []type = make([]type, len)
slice1 := make([]type, len)
make([]T, length, capacity)

// 切片初始化
s := []int{1,2,3 }

s := arr[下标开始:下标结束-1]
s := arr[startIndex:endIndex]
s := arr[startIndex:]
s := arr[:endIndex]
s := arr[:]
s1 := s[startIndex:endIndex]
s := make([]int, len, cap)				// []int, 元素为int切片, len是实际长度, cap是最大长度(容量)
len()		// 获取切片实际长度
cap()		// 测量切片的容量
numbers = append(numbers, 2,3,4)	// 向切片追加多个元素
copy(numbers1,numbers)						// 复制切片

[]byte 表示"字节切片"
```



## range

```go
// range
range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素
在数组(array)和切片(slice)中它返回元素的索引和索引对应的值，在集合(map)中返回 key-value 对儿
for _, num := range nums {
for i, num := range nums {
for k, v := range kvs {
for i, c := range "go" {
```



## map

```go
// 集合
Map 的 key 类型 始终一致, value 类型 始终一致
Map 是一种无序的键值对的集合. Map 最重要的一点是通过 key 来快速检索数据, key 类似于索引, 指向数据的值.
Map 是一种集合, 所以我们可以像迭代数组(array)和切片(slice)那样迭代它. 
不过, Map 是无序的, 我们无法决定它的返回顺序, 这是因为 Map 是使用 hash 表来实现的.

// 定义 Map
. 方法1
使用 map 关键字来定义 Map
/* 声明变量，默认 map 是 nil */
var 集合名字 map[k类型]v类型					// 声明
var countryCapitalMap map[string]string /*创建集合 */
countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}

. 方法2
使用内建函数 make来定义 Map
/* 使用 make 函数 */
集合名称 := make(map[k类型]v类型)
map_variable := make(map[key_data_type]value_data_type)
countryCapitalMap = make(map[string]string)
delete() 函数
delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key
delete(countryCapitalMap, "France")    /*删除元素*/ 

// 查找, 并检查存在性:
// 通过下标的的方式访问map中的元素会得到两个值, 第二个值是一个布尔值, 用来报告该元素是否存在
if v, ok := map_var[map_key]; ok {
	...
}
// 统计个数：
len(map_var)
// range循环:
for k, v := range map_var {
	...
}
// 设置值：
map_var[map_key] = new_map_val

// map 内部结构
Go中的map是一个指针, 它的底层是数组, 而且用到了两个数组, 其中一个更底层的数组用于打包保存key和value.
```



## assertion

```go
// 断言, 有两层含义, 判断和指定定义
fmt.Printf("%+v\n", mk["Sport"].(map[string]interface{})["c_Name"])			// 断言

if uu, ok := mk["Sport"].(map[string]interface{}); ok {			// mk["Sport"]的下一级["c_Name"]需要重新指定类型
  fmt.Println(uu["c_Name"])
}

if uu, ok := mk["Sport"].(map[string]string); !ok {
  fmt.Println(uu)
  fmt.Printf("%T\n", uu)
  fmt.Printf("%+v\n", uu)
}

// 返回
开幕式                                                                                                                                                                                                                                                       
开幕式                                                                                                                                                                                                                                                       
map[]                                                                                                                                                                                                                                                        
map[string]string                                                                                                                                                                                                                                            
map[]  
```



## struct

```go
// 结构体就是自定义的类型
// 不同类型的数据集合(相对于数组要求元素必须类型一致)
// 结构体是由一系列具有相同类型或不同类型的数据构成的数据集合


结构体定义: type + struct
type 语句设定了结构体的名称
struct 语句定义一个新的数据类型, 结构体中有一个或多个成员

type 结构体名字 struct {
   member definition
   member definition
   ...
   member definition
}

结构体名字就相当于一个新类型, 这个新类型是由基础类型组合而来, 甚至是由其他结构体组合而来
type 新类型 struct {
	其他类型组合
}

变量名 := 结构体名字 {成员1, 成员2, ..., 成员3}
变量名 := 结构体名字 {k1: v1, k2: v2, ..., kn: vn}
variable_name := structure_variable_type {value1, value2...valuen}
variable_name := structure_variable_type {key1: value1, key2: value2..., keyn: valuen}

// 访问, 访问结构体成员
需要使用点号 . 操作符
格式为:
结构体.成员名

// 结构体指针
var struct_pointer *Books			// Books型指针变量

// 结构体标签, Struct Tag
结构体标签
结构体的字段除了名字和类型外，还可以有一个可选的标签（tag）：它是一个附属于字段的字符串，可以是文档或其他的重要标记。比如在我们解析json或生成json文件时，常用到encoding/json包，它提供一些默认标签，例如：omitempty标签可以在序列化的时候忽略0值或者空值。而-标签的作用是不进行序列化，其效果和和直接将结构体中的字段写成小写的效果一样。
现在来了解下如何设置自定义的标签，以及如何像官方包一样，可以通过标签，对字段进行自定义处理。要实现这些，我们要用到reflect包。
// 编码时
名字AkeyA 类型 `json: "名字2key2"`		// 编码后, 用 名字2key2
// 解码时
名字AkeyA 类型 `json: "名字2key2"`		// 编码后, 访问用 名字2key2, 结构体定义是 名字AkeyA

// omitempty 和 "-"

// struct 结构体 S 不能再包含 结构体 S, 但能包含结构体 *S, 即指针类型, 如此一来就能构造递归的数据结构, 比如链表和树结构. 同样 array 数组也是同样
```



## func 和 函数签名 和 方法

```go
// 函数签名相同: 必须函数的函数名, 参数和返回值(类型, 个数, 顺序)都相同.
// 函数签名, 函数参数, 返回值以及它们的类型被统称为函数签名. 
// func (int) string, 参数和返回值不包括函数名, 称为函数签名
func 函数名(形参列表和类型) 返回类型列表 {函数体}

func swap(x, y string) (string, string) {
   return y, x
}
两种方式来传递参数: 
值传递(默认): 值传递是指在调用函数时将实际参数复制一份传递到函数中, 这样在函数中如果对参数进行修改, 将不会影响到实际参数
引用传递: 引用传递是指在调用函数时将实际参数的地址传递到函数中, 那么在函数中对参数所进行的修改, 将影响到实际参数
. 函数作为另一个函数的实参
. 闭包, 匿名函数
. 方法, 带接收者的函数

// 方法
方法, 就是有接收者的函数, 而接收者就是方法所依附的类, 类就是结构体
func (接收者变量名 接收者类型) 方法名(参数列表) (返回类型列表) {
	函数体
}

按照 GO 语言的习惯一般不用 this/self, 而是使用接收者类型的第一个小写字母, 可以看标准库中的代码风格.
func (p *Player) m_print_ptr() {			// p 为 *Player 接收者类型的第一个小写字母
  ...
}
```



## construct

```go
构造函数, 用来在创建对象时初始化对象, 即为对象成员变量赋初始值, 总与new运算符一起使用在创建对象的语句中.

1. 声明一个结构体, 因为结构体就相当于类
type ChunkFooter struct {
	ChunkDataTotalSize int
}

2. 声明一个全局函数, 返回的结果是上面1中的结构体. <- 此全局函数就是 ChunkFooter 类的 构造函数
func NewChunkFooter(chunkDataTotalSize int) *ChunkFooter {
  var result = new(ChunkFooter)
  result.ChunkDataTotalSize = chunkDataTotalSize
  return result
}
```



## interface

```go
// 接口
// 接口是一组仅包含方法名、参数、返回值的未具体实现的方法的集合
// 接口相当于是一份契约, 它规定了一个对象所能提供的一组操作.
// 一个类型只需要实现了接口要求的所有函数, 我们就说这个类型实现了该接口
// 接口是一组方法的集合, 抽象的类型. 是非侵入式的接口. 任何类型, 只要实现了该接口中方法集, 那么方法集就属于这个类型.
// 接口的命名大伙儿喜欢使用er结尾
接口即一种数据类型, 任何其他类型只要实现了这些方法就是实现了这个接口.
接口要求必须实现其中规定的方法, 即实现了其中所有方法, 那么就称为实现了这个接口

接口定义: type + interface
type 语句设定了接口的名称
interface 语句定义一个新的数据类型, 接口中有一个或多个方法

/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] { // 结构体struct_name实现了接口interface_name, 隐式, 非侵入
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}
package main
import (
    "fmt"
)
type Phone interface {
    call()
}
type NokiaPhone struct {
}

func (nokiaPhone NokiaPhone) call() {
    fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {
}

func (iPhone IPhone) call() {
    fmt.Println("I am iPhone, I can call you!")
}

func main() {
    var phone Phone
    phone = new(NokiaPhone)
    phone.call()
    phone = new(IPhone)
    phone.call()
}
在上面的例子中，我们定义了一个接口Phone，接口里面有一个方法call()。然后我们在main函数里面定义了一个Phone类型变量，并分别为之赋值为NokiaPhone和IPhone。然后调用call()方法，输出结果如下：
I am Nokia, I can call you!
I am iPhone, I can call you!
```

```go
package main

import (
  "fmt"
)

// 定义一个接口
type People interface {
  ReturnName() string
}

// 定义一个结构体
type Student struct {
  Name string
}

// 定义结构体的一个方法。
// 突然发现这个方法同接口People的所有方法(就一个)，此时可直接认为结构体Student实现了接口People
func (s Student) ReturnName() string {
  return s.Name
}

func main() {
  cbs := Student{Name:"咖啡色的羊驼"}

  var a People
  // 因为Students实现了接口所以直接赋值没问题
  // 如果没实现会报错：cannot use cbs (type Student) as type People in assignment:Student does not implement People (missing ReturnName method)
  a = cbs       
  name := a.ReturnName() 
  fmt.Println(name) // 输出"咖啡色的羊驼"
}

```



## pointer

```go
// 指针 pointer 首先明确是一种类型, 和 int, byte, string 等一样, 是一种类型, 表示为 *+类型名
// 而指针变量是另外一件事儿, 他存放一个内存'地址', 这个'地址'的内存里存放了'类型名'数据

// 指针变量 指向了一个值的内存地址
var name *type
var 指针变量名称 *值类型

指针变量是一种使用方便的占位符, 用于引用内存地址
Go 语言的取地址符是 &, 放到一个变量前使用就会返回相应变量的内存地址
nil 指针也称为空指针

// 指针的数组
var ptr [MAX]*int;
ptr 为整型指针数组. 即每个元素都是一个指针, 每个指针都指向了整型值的内存地址

// 指向指针的指针
一个指针变量存放的又是另一个指针变量的地址，则称这个指针变量为指向指针的指针变量
第一个指针存放第二个指针的地址，第二个指针存放整型变量的地址:
var ptr **int
```



## new & make

```go
// new() 返回的是指针, 即类型 *Type, 存储在堆
// make() 返回的是引用, 即类型 Type, 存储在堆, 不会返回指针

. new() 仅仅是分配了用于存放一个指针'地址'的内存空间. 而这个'地址'指向哪里是另外一件事儿. 这个'地址'指向 make的结果(3个域结构体)
var p *[]int = new([]int)        // *p == nil; with len and cap 0; p 为 ptr指针, *p取值, 值为地址(注释可能是错的)
p := new([]int)

. make() 直接得到分配内存里的实质数据结果的首地址, 结果还是地址(同new()的返回指针表象地址一样, 但意义完全不同, new()返回的是指针, 指针的表象是地址串, 仅仅是地址串和引用返回一样, 但此串非彼串, 意义完全不同. 指针与引用的不同)
make() 只能为 slice, map, channel 分配内存
```

![new和make在内存中的差异](https://img-blog.csdnimg.cn/img_convert/c22ba572b16b84427ef1e915efd9bb92.png#pic_center)

```go
var ptr *int									// ptr 此时并不指向任何内存地址
ptr = new(int)								// new() 返回指针(本质), 表象是一串地址

var foo map[string]int
foo = make(map[string]int)		// make() 返回引用(本质), 表象是一串地址(同指针, 但意义完全不同)
```



## 引用类型

```go
// & 取地址, * 解引用

// golang 中的引用类型有:
1.map, 无序的、键值对的集合;
2.pointers, 计算机内存中变量所在的内存地址; (是否算引用)
3.slice, 数组的抽象;
4.channel, 指管道，用于实现并行计算方程间通信;
5.interface, 指接口，一组方法签名的集合;
6.function, 指函数，不支持嵌套、重载和默认参数;

// 为什么slice、map和channel要由make创建呢?
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
slice的结构体由3部分构成, Pointer 是指向一个数组的指针, len 代表当前切片的长度, cap 是当前切片的容量.
可以看出三个类型的背后都引用了使用前必须初始化的数据结构
而只有 make 用于可以初始化其内部的数据结构并准备好将要使用的值
```



## 指针和引用

```go
// 指针
int a = 1;
int *p = &a;		// 将变量 a 的地址赋值给 指针变量 p, 同时依旧表明变量 *p 的类型为 int, 与变量 a 的类型一致. & 在等号右侧

// 引用
int a = 1;
int &b = a;			// 给变量 a 起一个别名, 叫做 b. & 在等号左侧. b 是 a 的一个引用(reference), a 是被引用物(referent), 对变量 b 的任何操作都是对变量 a 的操作

1. 说在最前面, 为什么而生
为什么先有了指针, 后来又有了引用?
用合适的工具干合适的事儿.
指针可以任意操作内存, 强大而危险! 就像一把刀可以砍柴也可以修指甲! 

举一个实际场景的例子: 
一个变量 n 使用了1G内存, 将变量 n 赋值给变量 m, 然后对变量 m 操作, 操作完毕后, 再将变量 m 赋值给变量 n. 这里的赋值会复制内存, 造成内存不够用. 此时可以用指针解决, 通过指针直接操作变量 n, 由于使用了指针那么操作是无限制的, 除了可以完成需要的操作外, 甚至还有机会干些其他的事儿, 这也包括通常认知的权利以外的事儿. 所以, 为了避免因为需求而又导致权力过大的情况, 于是发明了引用, 引用从概念上显然不是指针, 按这个逻辑就能实现即干事儿又不会因权力过大而发生危险.

类比现实中的例子:
某人需要开一份证明文件, 本来在文件上盖上公章就行了, 但如果把取公章的钥匙交给他, 那么他就获得了不该有的权利.

综上, 尽可能使用引用, 不得已时使用指针

2. 如何选择什么时候用指针, 什么时候用引用?
这个问题要从指针和引用的异同说起: (C++版)
相同:
. 变量值都是地址. 引用是操作受限的指针. 指针变量存储的是地址并且可以赋值修改, 引用变量存储的实质上也是地址, 但不能赋值修改, 只能使用. 

不同:
. 指针是值变量, 存储的是地址. 引用是别名
. 有const指针, 没有const引用
. 指针可以有多级, 但是引用只能是一级(int **p; 合法 而 int &&a是不合法的)
. 指针的值可以为空, 即地址为空, 但是引用的值不能为NULL, 并且引用在定义的时候必须初始化
. 指针的值在初始化后可以改变, 即地址可以重新被赋值改变, 即指向其它的存储单元, 而引用在进行初始化后就不会再改变了
. "sizeof引用" 得到的是所指向的变量(对象)的大小, 而"sizeof指针"得到的是指针本身的大小
. 指针和引用的自增(++)运算意义不一样;(golang 指针不能运算)

2. 从定义入手, 正向解释
指针, ptr, 指针类型变量存储的是一个地址, 这个地址指向内存的一个存储单元, 指针是一个完整的实体.
引用, ref, 别名(绰号), 

3. 不同角度解释
. 从内存分配角度, 指针会开辟新内存, 引用由于是别名不会被分配新内存空间.
. 从初始化角度, 指针可以先定义, 再初始化, 还能修改. 引用定义时必须初始化, 以后也不能修改.
. 从访问方式角度, 指针是间接访问, 引用是直接访问

4. 结论
尽可能使用引用, 不得已时使用指针
引用的主要功能是传递函数的参数和返回值
引用出现的典型场合是对象的表面, 而指针用于对象内部
函数的参数如果是指针类型, 那么通常是暗示此指针的对象是可以修改更换的, 如果不能更换那就用值类型了. 常见于方法的接收者都是指针.

```



## 操作符

```go
```



## string

```go
func stri() {
    ss := "阿迪哈佛安定嘎的高发大舅上看风景啊是"
    s2 := &ss
    s1 := ss[3:16]
    fmt.Printf("%T\n%+v\n%p\n", ss, ss, &ss)
    fmt.Printf("%T\n%+v\n%p\n", s2, s2, s2)
    fmt.Printf("%T\n%+v\n%p\n", s1, s1, &s1)
    fmt.Printf("%T\n%+v\n%p\n", len([]rune(ss)), len([]rune(ss)), &ss)
    fmt.Printf("%T\n%+v\n%p\n", utf8.RuneCountInString(ss), utf8.RuneCountInString(ss), &ss)
    fmt.Printf("%T\n%+v\n%p\n", bytes.Count([]byte(ss), nil)-1, bytes.Count([]byte(ss), nil)-1, &ss)
    fmt.Printf("%T\n%+v\n%p\n", strings.Count(ss, "")-1, strings.Count(ss, "")-1, &ss)
}

[chenchen@grpc01 rou]$ go run sa.go                                                                                                                                                                                                                          
string                                                                                                                                                                                                                                                       
阿迪哈佛安定嘎的高发大舅上看风景啊是                                                                                                                                                                                                                         
0xc000010260                                                                                                                                                                                                                                                 
*string                                                                                                                                                                                                                                                      
0xc000010260                                                                                                                                                                                                                                                 
0xc000010260                                                                                                                                                                                                                                                 
string                                                                                                                                                                                                                                                       
迪哈佛安                                                                                                                                                                                                                                                     
0xc000010270                                                                                                                                                                                                                                                 
int                                                                                                                                                                                                                                                          
18                                                                                                                                                                                                                                                           
0xc000010260                                                                                                                                                                                                                                                 
int                                                                                                                                                                                                                                                          
18                                                                                                                                                                                                                                                           
0xc000010260                                                                                                                                                                                                                                                 
int                                                                                                                                                                                                                                                          
18                                                                                                                                                                                                                                                           
0xc000010260                                                                                                                                                                                                                                                 
int                                                                                                                                                                                                                                                          
18                                                                                                                                                                                                                                                           
0xc000010260                                                                                                                                                                                                                                                 
[chenchen@grpc01 rou]$
```



## goroutine

```go
进程, 线程, 协程, 这里的协程不特指 goroutine, 而是泛协程概念
进程:
. 是操作系统资源分配和调度的基本单位
. 是关于某数据集合上的一次运行活动, 进程就是这个活动的实体
. 多进程多用在CPU密集型和IO密集型的程序上
. 优点, 多进程的优势就是一个子进程崩溃并不会影响其他子进程和主进程的运行, 
. 缺点, 但缺点就是不能一次性启动太多进程, 会严重影响系统的资源调度, 特别是CPU使用率和负载。

线程:
. 是CPU调度和分配的基本单位, 一个线程对应一个核心
. 线程是进程的一个实体, 进程内至少有一个线程, 返回来进程含有多个线程
. 线程间共享内存, 栈
. 多线程多用在IO密集型的程序上
. 优点, 多线程的优势是切换快, 资源消耗低, 但一个线程挂掉则会影响到所有线程，所以不够稳定。
. 缺点, 

协程(Coroutine):
. 用户态的线程(比喻), 线程和进程都是操作系统执行, 协程是用户自身程序, 栈在用户自身程序中维护
. 所以没有内核切换, 只是在golang应用内自己切换
. 一个线程对应多个协程, 一个进程也能直接有多个协程
. 线程, 进程都是同步机制, 但协程是异步机制
. 协程保留了上次调用时的状态, 每次过程重入时, 就相当于进入上一次调用的状态
. 强调非阻塞异步并发的一般都是使用协程


```

```go
goroutine简介
golang语言作者Rob Pike说:
"Goroutine是一个与其他goroutines 并发运行在同一地址空间的Go函数或方法. 一个运行的程序由一个或更多个goroutine组成. 它与线程、协程、进程等不同。它是一个goroutine".
goroutine 通过通道来通信, 而协程通过让出和恢复操作来通信;
goroutine 通过Golang 的调度器进行调度, 而协程通过程序本身调度;
简单的说就是Golang自己实现了协程并叫做goruntine(本文称Go协程), 且比协程更强大.


Go的CSP并发模型是通过Goroutine和channel来实现的
逻辑处理器, 一边负责执行 Goroutine(已经被创建的), 另一边绑定一个线程
调度器, 负责把 goroutine(已经被创建的) 分配给不同的 逻辑处理器, 这个过程是在go运行时里完成
全局队列, 所有刚创建的 goroutine 都放这里
本地队列, 专司逻辑处理器的 goroutine 队列

GPM模型的调度器是将协程与操作系统线程按照 m:n 来对应. 多对多.

整个GPM调度的简单过程如下:
. 新创建的Goroutine会先存放在Global全局队列中, 等待Go调度器进行调度, 随后Goroutine被分配给其中的一个逻辑处理器P, 并放到这个逻辑处理器对应的Local本地运行队列中, 最终等待被逻辑处理器P执行即可.
. 在M与P绑定后, M会不断从P的Local队列中无锁地取出G, 并切换到G的堆栈执行, 当P的Local队列中没有G时, 再从Global队列中获取一个G, 当Global队列中也没有待运行的G时. 则尝试从其它的P窃取部分G来执行相当于P之间的负载均衡.


```

```
GPM模型:
M, Machine, 操作系统线程, 即, 一个线程对应一个核心, 即, 内核级线程. 一个M直接关联了一个内核线程。由操作系统管理
P, processor, 逻辑处理器, 一端与M一对一绑定, 另一端对应一个 goroutine. 代表了M所需的上下文环境, 也是处理用户级代码逻辑的处理器. 它负责衔接M和G的调度上下文, 将等待执行的G与M对接.
G, Goroutine, 一个 Goroutine. 其实本质上也是一种轻量级的线程. 包括了调用栈, 重要的调度信息, 例如channel等.

P 的数量通过 GOMAXPROCS() 设定, 表示有多少个 Goroutine 可以同时运行
上下文P(Processor), 的数量在启动时设置为 GOMAXPROCS 环境变量的值或通过运行时 函数GOMAXPROCS().
context(P)

Process -> Thread(LWP, lightweight process) -> Goroutine(lightweight userspace thread)
协程结构体
保护和恢复上下文的函数
运行队列
编译器将go关键词编译为生成一个协程结构体, 并放入运行队列
```

```go
// golang 是并发, 从语言层面就支持并发, 简化了并发程序的开发过程. 语言内部实现了共享内存
并发, 在同一时刻, 只能有一条指令执行, 但多个进程指令被快速地轮换执行; 2个队伍, 1个窗口, 要求提升软件能力
并行, 在同一时刻, 有多条指令在多个CPU处理器上同时执行; 2个队伍, 2个窗口, 要求硬件支持
```



## Context

```go
// Context 原理
Context 的调用应该是链式的, 通过WithCancel, WithDeadline, WithTimeout或WithValue派生出新的 Context. 
当父 Context 被取消时, 其派生的所有 Context 都将取消.
通过context.WithXXX都将返回新的 Context 和 CancelFunc.
调用 CancelFunc 将取消子代, 移除父代对子代的引用, 并且停止所有定时器.
未能调用 CancelFunc 将泄漏子代, 直到父代被取消或定时器触发.
go vet工具检查所有流程控制路径上使用 CancelFuncs.
```



## channel

```go
```



## 堆heap 和 栈stack

```go
栈stack, https://baike.baidu.com/item/%E6%A0%88/12808149?fr=aladdin
堆heap, https://baike.baidu.com/item/%E5%A0%86/20606834?fr=aladdin
堆(Heap)是计算机科学中对一类特殊的数据结构的统称. 堆通常是一个可以被看做一棵 完全二叉树 的数组对象
堆是非线性数据结构, 相当于一维数组, 有两个直接后继.
```



## module & package

```go
多个包组成一个模块
模块是相关的Go包的集合. 模块是源代码交换和版本控制的单元. 
```



## 查看变量类型

```go
import (
    "fmt"
    "log"
    "os"
    "reflect"
)

func yocc() {
    file, err := os.Open("fclosing.json") // For read access.
    fmt.Println("file 的数据类型是:", reflect.TypeOf(file))				// 方法1
    fmt.Printf("file 的数据类型是: %T\n", file)										// 方法2
    fmt.Printf("file 的数据类型是: %v\n", file)
    fmt.Printf("file 的数据类型是: %+v\n", file)
    fmt.Printf("file 的数据类型是: %#v\n", file)
    fmt.Println("file 的数据类型是:", reflect.TypeOf(err))
    fmt.Printf("file 的数据类型是: %T\n", err)
    fmt.Printf("file 的数据类型是: %v\n", err)
    fmt.Printf("file 的数据类型是: %+v\n", err)
    fmt.Printf("file 的数据类型是: %#v\n", err)
}

// 返回
file 的数据类型是: *os.File                                                                                                                                                                                                                                  
file 的数据类型是: *os.File 
file 的数据类型是: &{0xc00004a180}                                                                                                                                                                                                                           
file 的数据类型是: &{file:0xc00004a180}                                                                                                                                                                                                                      
file 的数据类型是: &os.File{file:(*os.file)(0xc00004a180)}
file 的数据类型是: <nil>                                                                                                                                                                                                                                     
file 的数据类型是: <invalid reflect.Value>                                                                                                                                                                                                                   
file 的数据类型是: <nil>                                                                                                                                                                                                                                     
file 的数据类型是: <nil>                                                                                                                                                                                                                                     
file 的数据类型是: <nil>                                                                                                                                                                                                                                     
file 的数据类型是: <nil>   
```



## See Also

https://www.cnblogs.com/lxmhhy/p/6041001.html

https://bingjian-zhu.github.io/2019/09/12/%E5%BC%84%E6%87%82goroutine%E8%B0%83%E5%BA%A6%E5%8E%9F%E7%90%86/

https://baijiahao.baidu.com/s?id=1657961481257584064&wfr=spider&for=pc *

https://www.zhihu.com/question/20862617

https://studygolang.com/articles/20117

https://blog.csdn.net/cukw6666/article/details/107983171

https://studygolang.com/articles/23247?fr=sidebar context

https://zhuanlan.zhihu.com/p/110085652

https://blog.csdn.net/birdswillbecomdragon/article/details/105335120?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-10-105335120.pc_agg_new_rank&utm_term=go%E8%AF%AD%E8%A8%80%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B%E5%92%8C%E6%8C%87%E9%92%88%E5%8C%BA%E5%88%AB&spm=1000.2123.3001.4430

https://zhuanlan.zhihu.com/p/110085652 context

https://zhuanlan.zhihu.com/p/460958342 map

https://www.cnblogs.com/shijingxiang/articles/12196677.html heap

https://blog.csdn.net/chenmaya8367/article/details/100790549 heap





