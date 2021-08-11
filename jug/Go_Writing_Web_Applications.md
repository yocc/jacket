# Go, 编写网络应用程序

## 目录

[TOC]

简介
开始
数据结构
介绍 net/http 包 (插曲)
使用 net/http 为维基页面服务
编辑页面
html/template 包
处理不存在的页面
保存页面
错误处理
模板缓存
验证
介绍功能字面和闭包
试试吧！
其他任务

## 简介

本教程中包括:

- 使用负载和保存方法创建数据结构
- 使用 net/http 包构建 Web 应用程序
- 使用 html/template 包处理 HTML 模板
- 使用注册包验证用户输入
- 使用闭包

假设知识:

- 编程体验
- 了解基本网络技术(HTTP, HTML)
- 一些 UNIX/DOS 命令行知识

## 开始

目前, 您需要有一个自由 BSD, Linux, OS X, 或窗口机来运行 Go. 我们将使用 $ 表示命令提示符

安装"Go" (参阅安装说明), https://golang.org/doc/install

在 GOPATH 和 cd 中为此教程制作一个新的目录:

```
$ mkdir gowiki
$ cd gowiki
```

创建名为 wiki.go 的文件, 在您最喜爱的编辑器中打开它, 并添加以下行:

```
package main

import (
	"fmt"
	"io/ioutil"
)

```

我们从 Go 标准库导入 fmt 和 ioutil 包. 稍后, 当我们实施其他功能时, 我们将在此导入报表中添加更多包

## 数据结构

让我们从定义数据结构开始. Wiki 由一系列相互关联的页面组成, 每个页面都有标题和主体 (页面内容). 在这里, 我们将 Page 定义为具有两个字段 (代表标题和主体)
的结构.

```
type Page struct {
    Title string
    Body  []byte
}
```

该类型 []byte 字节表示"字节切片". (请参阅切片: https://golang.org/doc/articles/slices_usage_and_internals.html, 有关切片的更多信息, 请参阅使用和内部. body 元素是一个 []byte 字节, 而不是字符串, 因为这是我们将使用的 io 库所期望的类型, 如下所示

页面结构描述了页面数据如何存储在内存中. 但是, 持久存储呢？我们可以通过在 page:
上创建保存方法来解决这个问题

```
func (p *Page) save() error {
    filename := p.Title + ".txt"
    return ioutil.WriteFile(filename, p.Body, 0600)
}
```

此方法的签名为: "这是一种名为"save"的方法, 用作其接收器 p, 指向 Page 的指针. 它无需参数, 并返回类型错误值.
此方法将将页面的主体保存到文本文件中. 为了简单起多大, 我们将使用标题作为文件名称
保存方法返回错误值, 因为这是 WriteFile 的返回类型 (将字节切片写入文件的标准库函数). 保存方法返回错误值, 以便在编写文件时让应用程序处理它时出现任何问题. 如果一切顺利, page.save() 将返回零值 (指点、接口和其他类型的零值)
八进整数字面 0600, 作为写文件的第三个参数传递, 表示文件应仅在当前用户的读写权限下创建. (有关详细信息, 请参阅打开的 Unix man page open(2).
除了保存页面, 我们也要加载页面:

```
func loadPage(title string) *Page {
    filename := title + ".txt"
    body, _ := ioutil.ReadFile(filename)
    return &Page{Title: title, Body: body}
}
```

函数加载 Page 从标题参数构建文件名称, 将文件内容读入新的可变主体, 并将指点返回到具有正确标题和机身值的页面字面结构
函数可以返回多个值. 标准库功能 io. 阅读档案返回 []字节和错误. 在加载页面中, 错误尚未处理: 下划线 (\_) 符号表示的"空白标识符"用于扔掉错误返回值 (实质上, 将值分配给无值)
但是, 如果阅读档案遇到错误, 会发生什么情况呢？例如, 文件可能不存在. 我们不应忽视这些错误. 让我们修改函数以返回\*页面和错误

```
func loadPage(title string) (*Page, error) {
    filename := title + ".txt"
    body, err := ioutil.ReadFile(filename)
    if err != nil {
        return nil, err
    }
    return &Page{Title: title, Body: body}, nil
}
```

此功能的呼叫者现在可以检查第二个参数: 如果它是零, 那么它已经成功地加载了一个页面. 如果没有, 则呼叫者可以处理该错误 (详情请参阅语言规范 https://golang.org/ref/spec#Errors)
此时, 我们拥有简单的数据结构以及从文件中保存和加载的操作能力. 让我们写一个主要功能来测试我们写的东西:

```
func main() {
    p1 := &Page{Title: "TestPage", Body: []byte("This is a sample Page.")}
    p1.save()
    p2, _ := loadPage("TestPage")
    fmt.Println(string(p2.Body))
}

```

在编译和执行此代码后, 将创建名为 TestPage.txt 其中包含 p1 的内容. 然后, 文件将被读入结构 p2 中, 其"身体"元素打印到屏幕上
您可以像这样编译和运行程序:

```
$ go build wiki.go
$ ./wiki
This is a sample Page.

```

(如果您使用 Windows, 则必须键入"wiki", 而无需"./"即可运行程序.
单击此处查看我们编写的代码, https://golang.org/doc/articles/wiki/part1.go

## 介绍 net/http 包 (插曲)

下面是一个简单的 Web 服务器的完整工作示例:

```
// +build ignore

package main

import (
    "fmt"
    "log"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hi there, I love %s!", r.URL.Path[1:])
}

func main() {
    http.HandleFunc("/", handler)
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

主要功能从呼叫 http.HandleFunc, 它告诉 http 包处理所有请求到 web root ("/") 与处理程序.
然后它调用 http.ListenAndServe, 指定它应在端口 8080 上在任何接口 (": 8080") 上收听. (暂时不用担心它的第二个参数为零). 此功能将阻止, 直到程序终止
ListenAndServe 始终返回错误, 因为它仅在发生意外错误时返回. 为了记录该错误, 我们用日志包裹功能调用 log.Fatal.
函数处理器为 http.HandlerFunc. 这需要一个 http.ResponseWriter 和一个 http.Request 作为其参数
一个 http.ResponseWriter 值组装 HTTP 服务器的响应: 通过写信给它, 我们将数据发送给 HTTP 客户端.
一个 http.Request 是代表客户端 HTTP 请求的数据结构. r.URL.Path 是请求 URL 的路径组件. 尾随 [1: ] 表示"创建从第 1 个字符到末尾的路径子切片". 这将领先的"/"从路径名称中删除  
如果您运行此程序并访问 URL:

```
http://localhost:8080/monkeys
```

程序将呈现一个页面, 其中包含:

```
Hi there, I love monkeys!
```

## 使用 net/http 为维基页面服务

要使用 net/http 包装, 必须导入:

```
import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)
```

让我们创建一个处理程序, viewHandler, 将允许用户查看维基页面. 它将处理以"/view/"为前缀的网址.

```
func viewHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/view/"):]
    p, _ := loadPage(title)
    fmt.Fprintf(w, "<h1>%s</h1><div>%s</div>", p.Title, p.Body)
}
```

再次注意使用\_忽略加载页面中的错误返回值. 这是在这里做的简单和一般被认为是不良的做法. 我们稍后会处理此事.
首先, 此函数从 r.URL.Path 中提取页面标题. 路径、请求 URL 的路径组件. 路径将重新切片, 以[len ("/view/") : ]以删除请求路径中领先的"/view/"组件. 这是因为路径总是以"/view/"开头, 而"/view/"不是页面标题的一部分
然后, 该功能加载页面数据, 用一串简单的 HTML 格式化页面, 并将其写到 w, http.ResponseWriter.

要使用此处理程序, 我们重写主功能, 使用 viewHandler 初始化 http 以处理路径/view/下的任何请求.

```
func main() {
    http.HandleFunc("/view/", viewHandler)
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

单击此处查看我们编写的代码. https://golang.org/doc/articles/wiki/part2.go
让我们创建一些页面数据 (作为 test.txt), 编译我们的代码, 并尝试为 wiki 页面服务.
打开 test.txt 文件在您的编辑器中, 并保存字符串"Hello World"(没有报价) .

```
$ go build wiki.go
$ ./wiki
```

(如果您使用 Windows, 则必须键入"wiki", 而无需"./"即可运行程序.)
在运行此 Web 服务器后, 访问 http://localhost:8080/view/test 应显示一个名为"test"的页面, 其中包含"Hello World"等字样.

## 编辑页面

维基不是没有编辑页面能力的维基. 让我们创建两个新的处理程序: 一个名为编辑手显示"编辑页面"表单, 另一个命名为保存手, 以保存通过表单输入的数据.
首先, 我们将它们添加到主 ():

```
func main() {
    http.HandleFunc("/view/", viewHandler)
    http.HandleFunc("/edit/", editHandler)
    http.HandleFunc("/save/", saveHandler)
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

该函数 editHandler 加载页面 (或者, 如果不存在, 创建一个空的页面结构) , 并显示 HTML 表单.

```
func editHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/edit/"):]
    p, err := loadPage(title)
    if err != nil {
        p = &Page{Title: title}
    }
    fmt.Fprintf(w, "<h1>Editing %s</h1>"+
        "<form action=\"/save/%s\" method=\"POST\">"+
        "<textarea name=\"body\">%s</textarea><br>"+
        "<input type=\"submit\" value=\"Save\">"+
        "</form>",
        p.Title, p.Title, p.Body)
}
```

此功能将正常工作, 但所有硬编码的 HTML 是 ugly 丑陋的. 当然, 还有更好的方法.

## html/template 包

html/template 包是 Go 标准库的一部分. 我们可以使用 html/template 将 HTML 保留在单独的文件中, 从而允许我们在不修改基础 Go 代码的情况下更改编辑页面的布局.
首先, 我们必须在进口清单中添加 html/template. 我们也不会使用 fmt 了, 所以我们必须删除它.

```
import (
	"html/template"
	"io/ioutil"
	"net/http"
)
```

让我们创建包含 HTML 表单的模板文件. 打开名为"edit.html 的新文件, 并添加以下行:

```
<h1>Editing {{.Title}}</h1>

<form action="/save/{{.Title}}" method="POST">
<div><textarea name="body" rows="20" cols="80">{{printf "%s" .Body}}</textarea></div>
<div><input type="submit" value="Save"></div>
</form>
```

修改 editHandler 以使用模板, 而不是硬编码的 HTML:

```
func editHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/edit/"):]
    p, err := loadPage(title)
    if err != nil {
        p = &Page{Title: title}
    }
    t, _ := template.ParseFiles("edit.html")
    t.Execute(w, p)
}
```

函数 template.ParseFiles 将阅读 edit.html 并返回一个 \*template.Template.
t.Execute 执行模板的方法, 将生成的 HTML 写入 http.ResponseWriter. 这 .Title 和 .Body 点缀标识符是指 p.Title 和 p.Body.
模板指令被封闭在双卷曲大括号中. printf "% ". 身体指令是输出的函数调用. 身体作为一个字符串, 而不是字节流, 相同的调用 fmt.printf. html/template 包 有助于保证只有安全且外观正确的 HTML 才能通过模板操作生成. 例如, 它会自动逃逸任何大于符号 (>) , 代之以 &gt;, 以确保用户数据不会损坏表单 HTML.
由于我们现在正在处理模板, 让我们为我们的 viewHandler 创建一个模板, 称为"view.html":

```
<h1>{{.Title}}</h1>

<p>[<a href="/edit/{{.Title}}">edit</a>]</p>

<div>{{printf "%s" .Body}}</div>
```

相应地修改 viewHandler:

```
func viewHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/view/"):]
    p, _ := loadPage(title)
    t, _ := template.ParseFiles("view.html")
    t.Execute(w, p)
}
```

请注意, 我们在两个处理器中都使用了几乎完全相同的模板代码. 让我们通过将模板代码移动到自己的功能来消除此重复:

```
func renderTemplate(w http.ResponseWriter, tmpl string, p *Page) {
    t, _ := template.ParseFiles(tmpl + ".html")
    t.Execute(w, p)
}
```

并修改处理程序以使用该功能:

```
func viewHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/view/"):]
    p, _ := loadPage(title)
    renderTemplate(w, "view", p)
}
```

```
func editHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/edit/"):]
    p, err := loadPage(title)
    if err != nil {
        p = &Page{Title: title}
    }
    renderTemplate(w, "edit", p)
}
```

如果我们评论出我们未实施的保存处理程序的注册, 我们可以再次建立和测试我们的程序. 单击此处查看我们编写的代码. https://golang.org/doc/articles/wiki/part3.go

## 处理不存在的页面

如果您访问 /view/APageThatDoesntExist 不存在的页面怎么办？您将看到包含 HTML 的页面. 这是因为它忽略了加载页面中的错误返回值, 并继续尝试在没有数据的模板中填写模板. 相反, 如果请求的"页面"不存在, 则它应将客户端重定向到编辑页面, 以便创建内容:

```
func viewHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/view/"):]
    p, err := loadPage(title)
    if err != nil {
        http.Redirect(w, r, "/edit/"+title, http.StatusFound)
        return
    }
    renderTemplate(w, "view", p)
}
```

http.Redirect 功能增加了 http 状态代码. 状态已找到 (302) 和 HTTP 响应的位置标题.

## 保存页面

功能保存手将处理位于编辑页面上的表格的提交. 在主行取消调用相关行后, 让我们实施处理程序:

```
func saveHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/save/"):]
    body := r.FormValue("body")
    p := &Page{Title: title, Body: []byte(body)}
    p.save()
    http.Redirect(w, r, "/view/"+title, http.StatusFound)
}
```

页面标题 (在 URL 中提供) 和表单的唯一字段"Body"存储在新页面中. 然后调用 save() 方法将数据写入文件, 并将客户端重定向到/view/页面.
FormValue 返回的价值为类型 string 字符串. 我们必须将该值转换为 []byte, 然后才能将其转换为页面结构. 我们使用 []byte (body) 执行转换.

## 错误处理

我们的程序中有几个地方存在错误被忽略的情况. 这是不好的做法, 尤其是因为当错误发生时, 程序会有意想不到的行为. 更好的解决方案是处理错误并将错误消息返回给用户. 这样, 如果出现问题, 服务器将按照我们要求运行, 并可以通知用户.

首先, 让我们处理渲染板中的错误:

```
func renderTemplate(w http.ResponseWriter, tmpl string, p *Page) {
    t, err := template.ParseFiles(tmpl + ".html")
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    err = t.Execute(w, p)
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
    }
}
```

http.Error 功能发送指定的 HTTP 响应代码 (在此情况下为"内部服务器错误") 和错误消息. 将此功能置于单独功能中的决定已经有回报了.

现在让我们修复 saveHandler:

```
func saveHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/save/"):]
    body := r.FormValue("body")
    p := &Page{Title: title, Body: []byte(body)}
    err := p.save()
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    http.Redirect(w, r, "/view/"+title, http.StatusFound)
}
```

在 p.save() 期间发生的任何错误都将报告给用户.

## 模板缓存

此代码效率低下: 每次渲染页面时, renderTemplate 都会调用 ParseFiles. 更好的方法是在程序初始化时调用 ParseFiles 一次, 将所有模板解析为单个 \*Template. 然后, 我们可以使用执行板法来渲染特定的模板. https://golang.org/pkg/html/template/#Template.ExecuteTemplate
首先, 我们创建一个名为模板的全球变量, 并使用 ParseFiles 初始化它.

```
var templates = template.Must(template.ParseFiles("edit.html", "view.html"))
```

函数 template.Must 是一个方便包装, 当通过非零 error 值时会惊慌失措, 否则将返回\*Template 不变. 恐慌在这里是适当的: 如果模板无法加载, 唯一明智的做法就是退出程序.
ParseFiles 功能需要任意数量的字符串参数来识别我们的模板文件, 并将这些文件解析为以基本文件名称命名的模板. 如果我们要添加更多的模板到我们的程序, 我们将添加他们的名字到 ParseFiles 调用的参数.

然后, 我们修改 renderTemplate 功能以调用 templates.ExecuteTemplate 方法使用相应模板名称执行板法:

```
func renderTemplate(w http.ResponseWriter, tmpl string, p *Page) {
    err := templates.ExecuteTemplate(w, tmpl+".html", p)
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
    }
}
```

请注意, 模板名称是模板文件名称, 因此我们必须在 tmpl 参数中附加".html".

## 验证

正如您可能已经观察到的, 此程序存在严重的安全漏洞: 用户可以提供任意路径, 以便在服务器上读取/编写. 为了减轻这种情况, 我们可以编写一个功能, 以定期表达来验证标题.
首先, 在导入列表中添加"regexp". 然后, 我们可以创建一个全球变量来存储我们的验证表达式:

```
var validPath = regexp.MustCompile("^/(edit|save|view)/([a-zA-Z0-9]+)$")
```

功能 regexp.MustCompile 将分析和编译常规表达式, 并返回一个 regexp.Regexp. MustCompile 与编译不同, 因为如果表达式编译失败, 它会惊慌失措, 而编译则将错误作为第二个参数返回.
现在, 让我们编写一个功能, 使用 validPath 表达式来验证路径并提取页面标题:

```
func getTitle(w http.ResponseWriter, r *http.Request) (string, error) {
    m := validPath.FindStringSubmatch(r.URL.Path)
    if m == nil {
        http.NotFound(w, r)
        return "", errors.New("invalid Page Title")
    }
    return m[2], nil // The title is the second subexpression.
}
```

如果标题有效, 则将返回标题并返回并返回零错误值. 如果标题无效, 该功能将向 HTTP 连接写入"404 未找到"错误, 并将错误返回给处理程序. 要创建新错误, 我们必须导入错误包.
让我们给每个处理程序打电话获取标题:

```
func viewHandler(w http.ResponseWriter, r *http.Request) {
    title, err := getTitle(w, r)
    if err != nil {
        return
    }
    p, err := loadPage(title)
    if err != nil {
        http.Redirect(w, r, "/edit/"+title, http.StatusFound)
        return
    }
    renderTemplate(w, "view", p)
}
```

```
func editHandler(w http.ResponseWriter, r *http.Request) {
    title, err := getTitle(w, r)
    if err != nil {
        return
    }
    p, err := loadPage(title)
    if err != nil {
        p = &Page{Title: title}
    }
    renderTemplate(w, "edit", p)
}
```

```
func saveHandler(w http.ResponseWriter, r *http.Request) {
    title, err := getTitle(w, r)
    if err != nil {
        return
    }
    body := r.FormValue("body")
    p := &Page{Title: title, Body: []byte(body)}
    err = p.save()
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    http.Redirect(w, r, "/view/"+title, http.StatusFound)
}
```

## 介绍功能字面和闭包

捕捉每个处理器中的错误条件会引入大量重复代码. 如果我们能将每个处理程序包裹在进行此验证和错误检查的函数中呢？Go 的函数字面提供了抽象功能的强大方法, 可以帮助我们在这里. https://golang.org/ref/spec#Function_literals
首先, 我们重写每个处理程序的函数定义以接受标题字符串:

```
func viewHandler(w http.ResponseWriter, r *http.Request, title string)
func editHandler(w http.ResponseWriter, r *http.Request, title string)
func saveHandler(w http.ResponseWriter, r *http.Request, title string)
```

现在, 让我们定义一个包装功能, 它具有上述类型的函数, 并返回类型 http.HandlerFunc(适合传递到函数 http.HandlerFunc) :

```
func makeHandler(fn func (http.ResponseWriter, *http.Request, string)) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		// Here we will extract the page title from the Request,
		// and call the provided handler 'fn'
	}
}
```

返回的功能称为关闭, 因为它将外部定义的值所包围. 在这种情况下, 变量 fn (使汉德勒的单一参数) 被关闭所包围. 变量 fn 将是我们的保存、编辑或查看处理程序之一.
现在, 我们可以从获取标题中 getTitle 并在这里使用它 (稍作修改) :

```
func makeHandler(fn func(http.ResponseWriter, *http.Request, string)) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        m := validPath.FindStringSubmatch(r.URL.Path)
        if m == nil {
            http.NotFound(w, r)
            return
        }
        fn(w, r, m[2])
    }
}
```

makeHandler 返回的关闭是一个需要 http.ResponseWriter and http.Request (换句话说, 是一个 http.HandlerFunc). 闭合从请求路径中提取 title 标题, 并使用 validPath 有效的路径重新体验验证标题. 如果标题无效, 将使用 http 向响应编写器写入错误. 未找到功能. 如果标题有效, 将调用封闭的处理器函数 fn 与响应编写器、请求和标题作为参数.
现在, 我们可以在主处理器在 http 包中注册之前, 将处理器功能与 makeHandler 一起包裹起来:

```
func main() {
    http.HandleFunc("/view/", makeHandler(viewHandler))
    http.HandleFunc("/edit/", makeHandler(editHandler))
    http.HandleFunc("/save/", makeHandler(saveHandler))

    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

最后, 我们删除从处理器函数中 getTitle 的呼叫, 使其简单得多:

```
func viewHandler(w http.ResponseWriter, r *http.Request, title string) {
    p, err := loadPage(title)
    if err != nil {
        http.Redirect(w, r, "/edit/"+title, http.StatusFound)
        return
    }
    renderTemplate(w, "view", p)
}
```

```
func editHandler(w http.ResponseWriter, r *http.Request, title string) {
    p, err := loadPage(title)
    if err != nil {
        p = &Page{Title: title}
    }
    renderTemplate(w, "edit", p)
}
```

```
func saveHandler(w http.ResponseWriter, r *http.Request, title string) {
    body := r.FormValue("body")
    p := &Page{Title: title, Body: []byte(body)}
    err := p.save()
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    http.Redirect(w, r, "/view/"+title, http.StatusFound)
}
```

## 试试吧！

单击此处查看最终代码列表. https://golang.org/doc/articles/wiki/final.go

重新编译代码并运行应用:

```
$ go build wiki.go
$ ./wiki
```

访问 http://localhost:8080/view/ANewPage 应向您提供页面编辑表单. 然后, 您应该能够输入一些文本, 单击"Save", 并重定向到新创建的页面.

## 其他任务

以下是一些您可能想要自行处理的简单任务:

- 在 tmpl/ 和页面数据中存储模板 data/.
- 添加处理程序, 使 Web 根重定向到/view/FrontPage.
- 通过使页面模板有效并添加一些 CSS 规则来对页面模板进行细化.
- 通过将[page_2]实例转换为 <a href="/view/PageName">PageName</a>.. (提示: 您可以使用 regexp.ReplaceAllFunc 来做到这一点)

## 参考

https://golang.org/doc/articles/wiki/
