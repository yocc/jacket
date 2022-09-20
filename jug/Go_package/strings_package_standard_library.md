# strings_package_standard_libaray

----

### Overview [¶](https://pkg.go.dev/strings#pkg-overview)

strings包 实现了简单函数操作由 UTF-8 编码的 string.

有关 Go 中 UTF-8 字符串的信息, 请参阅: https://blog.golang.org/strings。



总结, 只能操作 UTF-8编码的 string



strings.Replace

Replace 返回字符串 s 的副本，其中前 n 个不重叠的 old 实例替换为 new。如果 old 是空的，它会在字符串的开头和每个 UTF-8 序列之后匹配，产生最多 k+1 个替换 k-rune 字符串。如果 n < 0，则替换次数没有限制。

```shell
func Replace(s, old, new string, n int) string
```





 

https://go.dev/blog/strings

打印 string
fmt.Println([string])
由于 [string] 中不是 ASCII或者UTF-8, 所以 web 页面呈现时出现乱码.
要想弄清楚问题, 就需要通过循环将每一个 byte 单独拿出来看一下.
    for i := 0; i < len(sample); i++ {
        fmt.Printf("%x ", sample[i])
    }
这段代码可以看到, 索引是单个 byte, 而不是 characters(字符), 循环的输出结果:
bd b2 3d bc 20 e2 8c 98
调试技巧:
fmt.Printf("%x\n", sample)
fmt.Printf("% x\n", sample)
fmt.Printf("%q\n", sample)
fmt.Printf("%+q\n", sample)

完整例子文件:
package main

import "fmt"

func main() {
    const sample = "\xbd\xb2\x3d\xbc\x20\xe2\x8c\x98"

    fmt.Println("Println:")
    fmt.Println(sample)
    
    fmt.Println("Byte loop:")
    for i := 0; i < len(sample); i++ {
        fmt.Printf("%x ", sample[i])
    }
    fmt.Printf("\n")
    
    fmt.Println("Printf with %x:")
    fmt.Printf("%x\n", sample)
    
    fmt.Println("Printf with % x:")
    fmt.Printf("% x\n", sample)
    
    fmt.Println("Printf with %q:")
    fmt.Printf("%q\n", sample)
    
    fmt.Println("Printf with %+q:")
    fmt.Printf("%+q\n", sample)
}


[练习：修改上述示例以使用字节片而不是字符串。提示：使用转换创建切片。]
[练习：在每个字节上使用%q格式循环字符串。输出告诉您什么？]

%x, 输出十六进制编码，字母形式为小写 a-f
%q, 源代码中那样带有双引号的输出
%q, 要输出的值是双引号输出就是双引号字符串；另外一种就是 go 自转义的 unicode 单引号字符









UTF-8 and string literals
string 的遍历, 遍历的是每个 byte, 而不是 character 字符, 所以说 string 只是一堆 bytes.
我们创建一个 "原始字符串" (raw string) 是用 反引号 扩起来.
反引号, 只能包含 literal text (字面文本)
双引号, 可以包含转义内容
也就是说, 双引号中有些字符是有转义含义的, 反引号是绝对的字面符号.

go 规定源码必须 UTF-8 编码. 
有些人认为 Go strings 总是 UTF-8, 但事实并非如此: 只有 string literal 是 UTF-8.
Go 源代码始终是 UTF-8
string 包含任意字节
没有字节级转义的字符串文字始终包含有效的 UTF-8 序列


Code points, characters, and runes
术语 code point, 指代由单个值表示的项目. 所有能用一个单个值表示的东西, 其中这个单个值叫码点 code point
"码点"有点拗口, 所以 Go 为这个概念引入了一个较短的术语: 符文rune. 该术语出现在库和源代码中, 其含义与"码点"完全相同, 但有一个有趣的补充.
rune 一词定义为 int32 类型的别名, 4字节
Unicode 码点称为符文
Go 不保证 strings 中的 characters 被规范化








Range loops, 范围循环
Go只有一种处理UTF-8的方法, 那就是使用 for range 循环处理 string
for range 循环在每次迭代中解码一个 UTF-8 编码的 rune (解码一个 UTF-8 编码的码点)
每次循环的索引是当前 rune (码点)的起始位置, 
以 byte 为单位, code point(码点)是索引的值
    const nihongo = "日本語"
    for index, runeValue := range nihongo {
        fmt.Printf("%#U starts at byte position %d\n", runeValue, index)
    }
返回:
U+65E5 '日' starts at byte position 0
U+672C '本' starts at byte position 3
U+8A9E '語' starts at byte position 6




Libraries
包: unicode/utf8, https://go.dev/pkg/unicode/utf8/
函数: DecodeRuneInString, 从字符串中解码出码点, 返回码点和码点宽度(及其 UTF-8 编码字节的宽度)
    const nihongo = "日本語"
    for i, w := 0, 0; i < len(nihongo); i += w {
        runeValue, width := utf8.DecodeRuneInString(nihongo[i:])
        fmt.Printf("%#U starts at byte position %d\n", runeValue, i)
        w = width
    }
返回:
U+65E5 '日' starts at byte position 0
U+672C '本' starts at byte position 3
U+8A9E '語' starts at byte position 6

for range 循环和 DecodeRuneInString 被定义为产生完全相同的迭代序列



结论
为了回答开头提出的问题：字符串是从字节构建的，因此索引它们会生成字节，而不是字符。字符串甚至可能不包含字符。事实上，“字符”的定义是不明确的，如果试图通过定义字符串由字符组成来解决歧义，那将是一个错误。
关于Unicode、UTF-8和多语言文本处理的世界还有很多要说的，但它可以等待另一篇文章。目前，我们希望您能够更好地理解Go字符串的行为，尽管它们可能包含任意字节，但UTF-8是其设计的核心部分。