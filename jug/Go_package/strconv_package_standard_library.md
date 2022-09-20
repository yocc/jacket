# strconv

----





## Overview [¶](https://pkg.go.dev/strconv#pkg-overview)

- [Numeric Conversions](https://pkg.go.dev/strconv#hdr-Numeric_Conversions)
- [String Conversions](https://pkg.go.dev/strconv#hdr-String_Conversions)

strvonv 包 实现了与 基本数据类型的 字符串表示 的转换. 

"字符串表示" 是指字面字符串, string literal



#### Numeric Conversions [¶](https://pkg.go.dev/strconv#hdr-Numeric_Conversions)

数字转换

数字字符串 转换成 纯数字, 即 Atoi (string to int), 反过来是 Itoa (int to string)

```go
i, err := strconv.Atoi("-42")
s := strconv.Itoa(-42)
```

These assume decimal and the Go int type.

ParseBool, ParseFloat, ParseInt, and ParseUint convert strings to values:

```go
b, err := strconv.ParseBool("true")
f, err := strconv.ParseFloat("3.1415", 64)
i, err := strconv.ParseInt("-42", 10, 64)
u, err := strconv.ParseUint("42", 10, 64)
```

The parse functions return the widest type (float64, int64, and uint64), but if the size argument specifies a narrower width the result can be converted to that narrower type without data loss:

```go
s := "2147483647" // biggest int32
i64, err := strconv.ParseInt(s, 10, 32)
...
i := int32(i64)
```

FormatBool, FormatFloat, FormatInt, and FormatUint convert values to strings:

```go
s := strconv.FormatBool(true)
s := strconv.FormatFloat(3.1415, 'E', -1, 64)
s := strconv.FormatInt(-42, 16)
s := strconv.FormatUint(42, 16)
```

AppendBool, AppendFloat, AppendInt, and AppendUint are similar but append the formatted value to a destination slice.



#### String Conversions [¶](https://pkg.go.dev/strconv#hdr-String_Conversions)

Quote 前缀函数就是将参数中的字符串返回时加上 双引号. 即, 字符串 quote(中国) 返回 "中国".

Quote 和 QuoteToASCII 将字符串转换为带引号的 Go 字符串字面. 

QuoteToASCII 的结果是个字符串, 并且这个字符串的每个字符都是 ASCII 字符. 如果是 非ASCII Unicode 会用 \u 转义, 这样就变成了 ASCII 编码了: 

也就说, 想要 "\u2e3f" 这样的字符串表示, 那就要用 QuoteToASCII()

```go
q := strconv.Quote("Hello, 世界")
q := strconv.QuoteToASCII("Hello, 世界")

s := strconv.Quote(`"Fran & Freddie's Diner	☺"`)
"\"Fran & Freddie's Diner\t☺\""

s := strconv.QuoteRune('☺')
'☺'
s := strconv.QuoteRuneToASCII('☺')
'\u263a'
s = strconv.QuoteRuneToGraphic('	') // tab character
'\t'
```

QuoteRune and QuoteRuneToASCII are similar but accept runes and return quoted Go rune literals.

Unquote and UnquoteChar unquote Go string and rune literals.

Unquote 将 s 解释为单引号、双引号或反引号 Go 字符串文字，返回 s 引用的字符串值。 （如果 s 是单引号，它将是 Go 字符文字；Unquote 返回相应的单字符字符串。）



strconv.Quote(s string)string -> 返回字符串在go语法下的双引号字面值表示，控制字符和不可打印字符会进行转义(\t,\n等)
strconv.Unquote(s string)(t string,err error) -> 函数假设s是一个半引号、双引号、反引号包围的go语法字符串，解析它并返回它表示的值。(如果是单引号括起来的，函数会认为s是go字符字面值，返回一个单字符的字符串)



双引号, double-quoted: ""
Golang 双引号表示一个字符串(Go语言的字符串是一个用UTF-8编码的变宽字符序列, 它的每一个字符都用一个或多个字节表示 , 所以说Go语言不存在乱码问题), 双引号内字符可以转义, 比如: \n, \r, `Go string literal`

反引号, backquoted: ``
反引号引起来的字符串就不支持转义，一些正则表达式，HTML，MySql语句都可以用反引号来表示

单引号, single-quoted: ''
一般只能用来包裹一个字节的ASCII码字符, 比如：var a int = '中', `Go one-character string`



## FAQ and troubleshooting





## See Also



