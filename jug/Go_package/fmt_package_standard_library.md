# fmt_package_standard_library

----

[toc]



## 常见快查

fmt.Print, 标准输出, 同 echo, 可变参数函数, 返回字节数和错误
fmt.Printf, 标注输出, 按给定格式, 返回字节数和错误
fmt.Println, 同 Print, 结尾带换行
fmt.Sprint, 类 Print, 但不标准输出而是返回字符串变量赋值
fmt.Sprintf, 同 Printf + Sprint
fmt.Sprintln, 同 Println + Sprint



打印函数, 有三个:

1. `fmt.Print`: 正常打印字符串和变量, 不会进行格式化, 不会自动换行, 需要手动添加 `\n` 进行换行, 多个变量值之间不会添加空格
2. `fmt.Println`: 正常打印字符串和变量, 不会进行格式化, 多个变量值之间会添加空格, 并且在每个变量值后面会进行自动换行
3. `fmt.Printf`: 可以按照自己需求对变量进行格式化打印. 需要手动添加 `\n` 进行换行



## 总揽

Printing, https://pkg.go.dev/fmt#hdr-Printing
Scanning, https://pkg.go.dev/fmt#hdr-Scanning
fmt 包是 IO 格式化的工具, 类似于 C 的 printf 和 scanf.

### Printing

这是一个动词:

一般:

```
%v      默认格式, 当打印结构体, “+”加号(%+v)标记是增加字段的名字
%#v     Go语法, 值的描述
%T      Go语法, 值类型的描述
%%      百分号; 没有值

通用占位符: 
%v: 以值的默认格式打印
%+v: 类似%v，但输出结构体时会添加字段名
%#v: 值的Go语法表示
%T: 打印值的类型
%%: 打印百分号本身

打印布尔值:
%t: 打印布尔值

打印字符串:
%s: 输出字符串表示（string类型或[]byte)
%q: 双引号围绕的字符串，由Go语法安全地转义
%x: 十六进制，小写字母，每字节两个字符
%X: 十六进制，大写字母，每字节两个字符

打印指针:
%p: 打印指针

打印整型:
%b：以二进制打印
%d：以十进制打印
%o：以八进制打印
%x：以十六进制打印，使用小写：a-f
%X：以十六进制打印，使用大写：A-F
%c：打印对应的的unicode码值
%q：该值对应的单引号括起来的go语法字符字面值，必要时会采用安全的转义表示
%U：该值对应的 Unicode格式：U+1234，等价于”U+%04X”

打印浮点数
%e：科学计数法，如-1234.456e+78
%E：科学计数法，如-1234.456E+78
%f：有小数部分但无指数部分，如123.456
%F：等价于%f
%g：根据实际情况采用%e或%f格式（以获得更简洁、准确的输出）
%G：根据实际情况采用%E或%F格式（以获得更简洁、准确的输出）

宽度标识符
宽度通过一个紧跟在百分号后面的十进制数指定，如果未指定宽度，则表示值时除必需之外不作填充。精度通过（可选的）宽度后跟点号后跟的十进制数指定。

如果未指定精度，会使用默认精度；如果点号后没有跟数字，表示精度为0。举例如下：

func main() {
    n := 12.34
    fmt.Printf("%f\n", n)     // 以默认精度打印
    fmt.Printf("%9f\n", n)   // 宽度为9，默认精度
    fmt.Printf("%.2f\n", n)  // 默认宽度，精度2
    fmt.Printf("%9.2f\n", n)  //宽度9，精度2
    fmt.Printf("%9.f\n", n)    // 宽度9，精度0
}
输出如下

10.240000
10.240000
10.24
    10.24
       10
  
占位符：%+
%+v：若值为结构体，则输出将包括结构体的字段名。
%+q：保证只输出ASCII编码的字符，非 ASCII 字符则以unicode编码表示

占位符：%
%#x：给打印出来的是 16 进制字符串加前缀 0x
%#q：用反引号包含，打印原始字符串
%#U：若是可打印的字符，则将其打印出来
%#p：若是打印指针的内存地址，则去掉前缀 0x

对齐补全
字符串

func main() {
    // 打印的值宽度为5，若不足5个字符，则在前面补空格凑足5个字符。
    fmt.Printf("a%5sc\n", "b")   // output: a    bc
    // 打印的值宽度为5，若不足5个字符，则在后面补空格凑足5个字符。
    fmt.Printf("a%-5sc\n", "b")  //output: ab    c

    // 不想用空格补全，还可以指定0，其他数值不可以，注意：只能在前边补全，后边补全无法指定字符
    fmt.Printf("a%05sc\n", "b") // output: a0000bc
     // 若超过5个字符，不会截断
    fmt.Printf("a%5sd\n", "bbbccc") // output: abbbcccd
}
输出如下

a    bc
ab    c
a0000bc
abbbcccd
浮点数

func main() {
    // 保证宽度为6（包含小数点)，2位小数，右对齐
    // 不足6位时，整数部分空格补全，小数部分补零，超过6位时，小数部分四舍五入
    fmt.Printf("%6.2f,%6.2f\n", 12.3, 123.4567)

    // 保证宽度为6（包含小数点)，2位小数，- 表示左对齐
    // 不足6位时，整数部分空格补全，小数部分补零，超过6位时，小数部分四舍五入
    fmt.Printf("%-6.2f,%-6.2f\n", 12.2, 123.4567)
}
输出如下

 12.30,123.46
12.20 ,123.46
正负号占位
如果是正数，则留一个空格，表示正数

如果是负数，则在此位置，用 - 表示

func main() {
    fmt.Printf("1% d3\n", 22)
    fmt.Printf("1% d3\n", -22)
}
输出如下

1 223
1-223
以上就是参考 golang - fmt 文档 整理而成的 fmt.Printf 的使用手册。
```

布尔:

```
%t	这字是true还是false
```

整型:

```
%b	二进制
%c	unicode 马点
%d	十进制
%o	八进制
%O	带0o前缀的八进制
%q	单引号, 安全转义
%x	小写16进制
%X	大写16进制
%U	Unicode 格式: U+1234; same as "U+%04X"
```

浮点和复杂部分:

```
%b	decimalless scientific notation with exponent a power of two,
	in the manner of strconv.FormatFloat with the 'b' format,
	e.g. -123456p-78
%e	scientific notation, e.g. -1.234456e+78
%E	scientific notation, e.g. -1.234456E+78
%f	decimal point but no exponent, e.g. 123.456             小数点，但没有指数
%F	synonym for %f
%g	%e for large exponents, %f otherwise. Precision is discussed below.
%G	%E for large exponents, %F otherwise
%x	hexadecimal notation (with decimal power of two exponent), e.g. -0x1.23abcp+20
%X	upper-case hexadecimal notation, e.g. -0X1.23ABCP+20
```

```
%s	the uninterpreted bytes of the string or slice
%q	a double-quoted string safely escaped with Go syntax
%x	base 16, lower-case, two characters per byte
%X	base 16, upper-case, two characters per byte
```

切片:

```
%p	切片的地址是用0下标元素的16进制表达方式表示, 0x    address of 0th element in base 16 notation, with leading 0x
```

指针:

```
%p	0x开头的16进制
The %b, %d, %o, %x and %X verbs 也能用于指针, 如果是整数的话也能显式的格式化值
```

对于%v 的默认格式:

```
bool:                    %t     布尔
int, int8 etc.:          %d     整型
uint, uint8 etc.:        %d, %#x if printed with %#v        # 如果被 %#v 打印的话, 用 %#x
float32, complex64, etc: %g     浮点
string:                  %s     字符串
chan:                    %p     指针
pointer:                 %p     指针
```

对于复合对象, 元素使用以下规则进行打印, 递归式, 布局为:

```
struct:             {field0 field1 ...}
array, slice:       [elem0 elem1 ...]
maps:               map[key1:value1 key2:value2 ...]
pointer to above:   &{}, &[], &map[]                            以上指针表示, 同perl
```

宽度由动词正前方的可选小数表示。如果没有，宽度是代表值所必需的。精度在（可选）宽度后按一个周期（小数）来指定。如果没有句点，则使用默认精度。没有以下数字的句点指定精度为零。例子：
小数点前的数字表示: 表示浮点小数的宽度
小数点表示精度:
不写小数点, 表示默认精度
只写小数点但后面无数字, 表示精度 0
写小数点而且还有后面的数字, 表示精度为后面的数字

```
%f     default width, default precision
%9f    width 9, default precision
%.2f   default width, precision 2
%9.2f  width 9, precision 2
%9.f   width 9, precision 0
```

宽度和精度以 Unicode 代码点的单位（即运行）单位进行测量。（这与 C 的打印件不同，C 的打印件总是以字节进行测量。任一或两个标志都可以替换为字符"\*"，导致其值从下一个操作（在操作前到格式）获得，该操作必须是类型 int。

对于大多数值，宽度是输出的最小运行次数，如有必要，用空格填充格式形式。

但是，对于字符串、小节切片和条形阵列，精度限制要格式化的输入长度（而不是输出的大小），如有必要，会截断。通常以运行方式测量，但对于这些类型，当格式化为 %x 或 % X 格式时，则以字节进行测量。

对于浮点值，宽度设置字段的最小宽度，如果适当的话，精度设置小数后的位置数，但 %g/% G 精度设置显著数字的最大数（删除尾随零）。例如，给定 12.345 格式 %6.3f 打印 12.345，而 .3g 打印 12.3。%e、% f 和 % #g 的默认精度为 6;对于 %g 来说，这是唯一识别值所需的最小数字数。

对于复杂数字，宽度和精度独立适用于两个组件，结果为括号，因此 %f 应用于 1.2=3.4i 生产（1.200000=3.400000i）。

其他标记:

```
+	always print a sign for numeric values; 总是打印一个数字值的字符,
	guarantee ASCII-only output for %q (%+q) 双引号保证输出
-	pad with spaces on the right rather than the left (left-justify the field) 右侧填充, 左对齐
#	alternate format: add leading 0b for binary (%#b), 0 for octal (%#o), %#b 二进制0b表示; %#o 八进制0表示; %#x 十六进制0x或者0X表示; %#p 指针0x表示; %q打印反引号 strconv.CanBackquote 返回true
	0x or 0X for hex (%#x or %#X); suppress 0x for %p (%#p);
	for %q, print a raw (backquoted) string if strconv.CanBackquote
	returns true;
	always print a decimal point for %e, %E, %f, %F, %g and %G;
	do not remove trailing zeros for %g and %G;
	write e.g. U+0078 'x' if the character is printable for %U (%#U).
' '	(space) leave a space for elided sign in numbers (% d);
	put spaces between bytes printing strings or slices in hex (% x, % X)
0	pad with leading zeros rather than spaces;
	for numbers, this moves the padding after the sign
```

## FAQ



## See Also

https://golang.org/pkg/fmt/

https://www.liwenzhou.com/posts/Go/go_fmt/
