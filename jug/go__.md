# go__

----



## Overview

```go
    // 中文转unicode
    sText := "hello 你好"
    textQuoted := strconv.QuoteToASCII(sText)
    textUnquoted := textQuoted[1 : len(textQuoted)-1]
    fmt.Println(textUnquoted)

    // unicode 转中文
    v, _ := zhToUnicode([]byte(textUnquoted))
    fmt.Println(string(v))

func zhToUnicode(raw []byte) ([]byte, error) {
    str, err := strconv.Unquote(strings.Replace(strconv.Quote(string(raw)), `\\u`, `\u`, -1))
    if err != nil { 
        return nil, err
    }
    return []byte(str), nil
}
func UnescapeUnicode(raw []byte) ([]byte, error) {
	str, err := strconv.Unquote(strings.Replace(strconv.Quote(string(raw)), `\\u`, `\u`, -1))
	if err != nil {
		return nil, err
	}
	return []byte(str), nil
}


// 把标准 json 序列化字符串中的中文, 能够在终端打印出来, 以便调试
1. 从 curl 获得的 json 序列化响应的字符串是 []uint8 字节流类型
2. 通过 string() 强制将 []uint8 转成 string 类型, string([]uint8)
3. 对 string 类型字符串, 进行 strconv.Quote() 双引号转义, strconv.Quote(string([]uint8)), 结果是 "\"\\uffff", 即, 双引号里面的双引号, 反斜线, 制表符等等都变成反斜线\转义.
4. 对双引号转义后的字符串进行替换, strings.Replace(strconv.Quote(string([]uint8)), `\\u`, `\u`, -1), 这个操作是将 \\uffff 这种 Unicode 字面量, 通过查找替换的方法把 \\u 替换成 \u, 即将字面量变成Unicode 码点 表示
5. 最后通过 strconv.Unquote() 将整个转义过的字符串重新反解双引号, 此时由于其中的 \u Unicode 码点 并不是 \\u 字面量了, 所以反解时是可以被解析成纯中文 Unicode 字符的.
总结: 
. 先判断 \uffff Unicode 码点表达是字面量还是ASCII编码. 
. 如果整个字符串是非转义的话, 需要先转义, 然后替换, 然后再解转义.
. 如果整个字符串是转义的话, 那么直接替换, 然后再解转义.
. 如何判断整个字符串是否转义, 只需要看双引号, 反斜线等的前面是否有转义字符\, 比如: \\", \\u
```

```go
// 只删除字符串前后的双引号, 或者反引号, 其他内容不变
1. 简单粗暴
s = s[1 : len(s)-1]

2. 严谨
if len(s) > 0 && s[0] == '"' {
    s = s[1:]
}
if len(s) > 0 && s[len(s)-1] == '"' {
    s = s[:len(s)-1]
}
```

```go
// /jsonparser
```







## FAQ and troubleshooting

### 请求接口返回 json, 在终端上调试, 中文显示

```gas
func main() {
    du := "https://super.sports.sina.cn/players/api/various/datum?league=cba_1&id=4400"
    resp, err := http.Get(du)
    defer resp.Body.Close()
    if err != nil {
        // handle error
    }   

    fmt.Printf("resp.Status:            %T, %s\n", resp.Status, resp.Status)
    fmt.Printf("resp.Body:              %T, %#v\n", resp.Body, resp.Body)
    fmt.Printf("resp.Close:             %T, %t\n", resp.Close, resp.Close)
    body, err := io.ReadAll(resp.Body)
    zw := strconv.Quote(string(body))
    fmt.Println(zw)
    str, err := strconv.Unquote(strings.Replace(zw, `\\u`, `\u`, -1))
    fmt.Printf("%T, %+v\n\n\n", str, str)
    fmt.Println(str)
    os.Exit(0)
}

func UnescapeUnicode(raw []byte) ([]byte, error) {
	str, err := strconv.Unquote(strings.Replace(strconv.Quote(string(raw)), `\\u`, `\u`, -1))
	if err != nil {
		return nil, err
	}
	return []byte(str), nil
}
```

```go
2. 
问题:
go run 时 报错:
`./eaj.go:41:6: no new variables on left side of :=`

原因: 
Go语言中使用 := 进行赋值导致 no new variables on left side of := 错误, 其原因是 := 左侧没有新变量; := 左侧至少要有一个新的变量;

错误示例:
package main
func main() {
	mystr := "hello world"
	println(mystr)
	mystr := "I was just here"
	println(mystr)
}
这个错误的示例将造成 no new variables on left side of :=
在第9行中的 := 左侧没有新的变量, mystr在第5行就已经被定义.

正确示例:
package main
func main() {
	mystr := "what is your name?"
	println(mystr)
	mystr, name := "my name is","Robot 1"
	println(mystr, name)
}
这段代码正确, 因为左侧有一个新的变量.

使用 "_" 的情况
mystr, _ := "my name is", "Robot 1"
"_" 不属于新的变量, 这也会造成错误!!!
```

```go
3.
问题:
go run 时 报错:
`./eaj.go:41:35: cannot use rs (variable of type *Result) as type easyjson.Unmarshaler in argument to easyjson.Unmarshal:                                                                                                                                     
        *Result does not implement easyjson.Unmarshaler (missing UnmarshalEasyJSON method)`
不能使用 rs（*Result 类型的变量）作为 easyjson 类型。Unmarshaler在争论easyjson。Unmarshal：                                                                                                                                    
        *结果不实现 easyjson。Unmarshaler （Missing UnmarshalEasyJSON method）

原因:


```





## See Also

