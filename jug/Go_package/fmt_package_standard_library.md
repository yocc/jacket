# fmt_package_standard_library

## 常见快查

fmt.Print, 标准输出, 同 echo, 可变参数函数, 返回字节数和错误
fmt.Printf, 标注输出, 按给定格式, 返回字节数和错误
fmt.Println, 同 Print, 结尾带换行
fmt.Sprint, 类 Print, 但不标准输出而是返回字符串变量赋值
fmt.Sprintf, 同 Printf + Sprint
fmt.Sprintln, 同 Println + Sprint

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
