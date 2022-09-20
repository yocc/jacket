make & makefile

---









```shell
Make 

Make 这个词, 英语的意思是 "制作".
Make 命令直接用了这个意思, 就是要做出某个文件. 
比如, 要做出文件 a.txt, 就可以执行下面的命令.

代码变成可执行文件，叫做编译(compile);
先编译这个，还是先编译那个(即编译的安排)，叫做构建(build)。

make 是最常用的构建(build)工具
make 只是一个根据指定的 Shell 命令进行构建的工具. 这些 Shell 命令规则都放在 makefile 文件中.
它的规则很简单，你规定要构建哪个文件、它依赖哪些源文件，当那些文件有变动时，如何重新构建它.

$ make -f rules.txt
$ make -f makefile
# 或者
$ make --file=rules.txt
$ make --file=makefile
```

```shell
$ make distclean, 类似 make clean, 但同时也将 configure 生成的文件全部删除掉, 包括 Makefile.
$ make clean, 清除上次的 make 命令所产生的 object 文件(后缀为 ".o" 的文件)及可执行文件.
```

```shell
make 调试方法
驱动、内核等大型工程包含众多 .c .h 文件, 如果手动一个个去编译这些文件是不现实的, 通常的做法是使用 make 命令进行自动化编译.
make 命令执行时, 需要 makefile 文件, 以告诉 make 命令需要怎样去编译和链接程序.

不过这些大型工程的 makefile 文件往往不止一个, 且常常分散在各个层级. 
如果编译时报错, 调试起来就比较麻烦了. 
联想到 C 语言 Debug 的方法, 通常是在出现问题的语句前面添加打印, 来判断变量的值是否符合预期, 语句是否被执行等等. 
那么在 Makefile 中有没有类似的 Debug 方法呢?

控制 make 的函数
答案是有的，那就是控制 make 的函数(Functions That Control Make)
make 提供了一些函数, 可以检测 makefile 的运行时信息, 来控制 make 的运行.

(1) info 函数
$(info <text ...>)
功能: 打印信息

(2) error 函数
$(error <text ...>)
功能: 产生一个致命的错误, 终止 make 运行, <text ...> 是错误信息.

(3) warning 函数
$(warning <text ...>)
功能: 这个函数很像 error 函数, 只是它并不会让 make 退出, 只是输出一段警告信息, 而 make 继续执行.

```



```shell
案例分析
期望: config.mk 作为 Makefile 的配置文件, ARM = y 表明想编译出 ARM 架构程序, 即用 arm-linux-gcc 进行编译.
但是执行后, make 并没有按照预期使用 arm-linux-gcc 进行编译, 而是使用默认的 cc 进行编译. 问题出在哪呢?

先看代码
$ tree
.
├── config.mk
├── hello.c
└── Makefile

config.mk 文件
ARM = y 

Makefile 文件
include config.mk

ifeq ($(ARM), y)
CC = arm-linux-gcc
endif

all:
	$(CC) hello.c

hello.c 文件
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	printf("hello world\n");
	return EXIT_SUCCESS;
}

执行 make
$ make
cc hello.c
看到执行结果, 很显然 CC 没有被正确赋值为 arm-linux-gcc, 难道 ifeq ($(ARM), y) 条件不成立? 测试一下
include config.mk

ifeq ($(ARM), y)
111
CC = arm-linux-gcc
endif

all:
	$(CC) hello.c
在 ifeq ($(ARM), y) 下面加入错误语句 111, 结果执行没有报错, 说明 ifeq ($(ARM), y) 条件确实是不成立的.
但是我已经包含了 config.mk 这个文件了啊, 而且 config.mk 里面定义了变量 ARM, 并且赋值了 y. 
为什么 ifeq ($(ARM), y) 条件就是不成立呢? 百思不得其解!!

这时候就可以启用我们的 make 调试方法了, 先上 info 函数
include config.mk

$(info "ARM=$(ARM)")

ifeq ($(ARM), y)
111
CC = arm-linux-gcc
endif

all:
	$(CC) hello.c
执行
$ make
"ARM=y "
cc hello.c
原来 y 后面多了一个空格, 导致条件判断不通过. 可以看到, 使用 info 函数进行 make 打印调试, 可以快速定位问题.

其实这里 ifeq ($(ARM), y) 的写法不好, 更好的写法是 ifeq ($(strip $(ARM)), y).
这样, 即便在 y 前后有空格, strip 函数也会帮助我们去除, 增强脚本的健壮性.
include config.mk

$(info "ARM=$(ARM)")

ifeq ($(strip $(ARM)), y)
CC = arm-linux-gcc
endif

all:
	$(CC) hello.c
运行
$ make
"ARM=y "
arm-linux-gcc hello.c
可以看到, 即便 y 后面有空格, 由于使用 strip 函数的作用, 帮我们把空格去除了, ifeq ($(strip $(ARM)), y) 条件也就成立了, 最终使用了 arm-linux-gcc 进行编译.
回过头来, 这一切还是要归功于 info 函数给我们调试 make 带来的便利性, 才能有后面我们的解决方案和优化方案.
除了 info 函数, 我们还可以使用 warning 函数, 它比 info 函数打印更多的信息: 文件名称和行号.

不过上述两个函数只能打印调试信息, 并不能控制 make 的运行, 我们在 make 大型工程时, 可能 make 流程已经不符合我们的预设了, 但是 make 还在继续执行, 这时我们只能手动停止, 然后向上滚动 log 查找出错位置.
其实我们可以有更好的方法, 那就是使用 error 函数, 它可以让 make 停止在你想要停止的位置, 方便迅速定位问题.
比如上面示例中, 我们想要使用 arm-linux-gcc 进行编译, 如果 CC 没能被正确赋值为 arm-linux-gcc, 我们就让 make 报错停止下来.
include config.mk

# $(info "ARM=$(ARM)")
# $(error "ARM=$(ARM)")
$(warning "ARM=$(ARM)")

ifeq ($(ARM), y)
CC = arm-linux-gcc
else
$(error "ARM=$(ARM)")
endif

all:
	$(CC) hello.c
运行

$ make
Makefile:5: "ARM=y "
Makefile:10: *** "ARM=y "。 停止。

```





````shell
Makefile
构建规则都写在 Makefile 文件里面, 要学会如何 Make 命令, 就必须学会如何编写 Makefile 文件.

1. 概述:
Makefile文件由一系列规则(rules)构成. 每条规则的形式如下.
:
[tab]
上面第一行冒号前面的部分, 叫做 "目标" (target), 冒号后面的部分叫做 "前置条件" (prerequisites); 
第二行必须由一个 tab 键起首, 后面跟着 "命令" (commands).

"目标" 是必需的, 不可省略; 
"前置条件" 和 "命令" 都是可选的, 但是两者之中必须至少存在一个.

每条规则就明确两件事: 构建目标的前置条件是什么, 以及如何构建. 下面就详细讲解, 每条规则的这三个组成部分.

```
"目标"(target) : "前置条件"(prerequisites)
tab开始 命令
```

2. "目标" (target)
a) "目标" 通常是文件名, 指明 make 命令所要构建的对象, 比如, yocc.txt
b) "目标" 可以是一个文件名, 也可以是多个文件名, 之间用空格分隔
c) "目标" 还可以是某个操作的 名字, 这称为 "伪目标" (phony target).
clean:
	[tab]rm *.o
上面代码的目标是 clean, 它不是文件名, 而是一个操作的名字, 属于 "伪目标", 作用是删除对象文件.
以下通过 `make 命令 + "目标"` 直接调用:
$ make clean
d) 如果当前目录中, 正好有一个文件叫做 clean, 那么这个命令不会执行. (make 制作 + a.txt 文件)
因为 Make 发现 clean 文件已经存在, 就认为没有必要重新构建了, 就不会执行指定的 rm 命令.
为了避免这种情况, 可以明确声明 clean 是 "伪目标", 写法如下.
.PHONY: clean
clean:
rm *.o temp
声明 clean 是 "伪目标" 之后, make 就不会去检查是否存在一个叫做 clean 的文件, 而是每次运行都执行对应的命令.
像 .PHONY 这样的内置目标名还有不少, 可以查看手册.
e) 如果 Make 命令运行时没有指定目标, 默认会执行 Makefile 文件的第一个目标.
$ make
上面代码执行 Makefile 文件的第一个目标.

3. "前置条件"(prerequisites)
"前置条件" 通常是一组文件名, 之间用空格分隔.
它指定了 "目标" 是否重新构建的判断标准: 只要有一个前置文件不存在, 或者有过更新("前置文件" 的 last-modification 时间戳比 "目标" 的时间戳新), "目标" 就需要重新构建.
result.txt: source.txt
	[tab]cp source.txt result.txt
上面代码中, 构建 result.txt ("目标") 的 "前置条件" 是 source.txt. (这里要特别注意, 命令行要以[tab]起首, 并且不能是4空格, 参考FAQ)
如果当前目录中, source.txt 已经存在, 那么 make result.txt 可以正常运行, 否则必须再写一条规则, 来生成 source.txt
source.txt:
echo "this is the source" > source.txt
上面代码中, source.txt 后面没有前置条件, 就意味着它跟其他文件都无关, 只要这个文件还不存在, 每次调用make source.txt, 它都会生成.

$ make result.txt
[chenchen@grpc01 mf]$ make result.txt
cp source.txt result.txt

$ make result.txt
[chenchen@grpc01 mf]$ make result.txt
make: `result.txt' is up to date.

上面命令连续执行两次 make result.txt
第一次执行会先新建 source.txt, 然后再新建 result.txt
第二次执行, Make发现 source.txt 没有变动(时间戳晚于 result.txt), 就不会执行任何操作, result.txt 也不会重新生成.

如果需要生成多个文件, 往往采用下面的写法.
source: file1 file2 file3
上面代码中, source 是一个伪目标, 只有三个前置文件, 没有任何对应的命令.
$ make source
执行make source命令后, 就会一次性生成 file1, file2, file3 三个文件. 这比下面的写法要方便很多.
$ make file1
$ make file2
$ make file3

2.4 命令(commands)
命令(commands)表示如何更新目标文件, 由一行或多行的Shell命令组成. 
它是构建 "目标" 的具体指令, 它的运行结果通常就是生成目标文件.

每行命令之前必须有一个 tab 键. 如果想用其他键, 可以用内置变量 .RECIPEPREFIX 声明.
.RECIPEPREFIX = >
all:
> echo Hello, world
上面代码用 .RECIPEPREFIX指定, 大于号(>)替代 tab键. 所以, 每一行命令的起首变成了大于号, 而不是 tab键.
[chenchen@grpc01 mf_all]$ make all
echo Hello, world
Hello, world

需要注意的是, 每行命令在一个单独的 shell 中执行. 这些Shell之间没有继承关系.
var-lost:
	export foo=bar
	echo "foo=[$$foo]"
[chenchen@grpc01 mf_all]$ make -f Makefile1
export foo=bar
echo "foo=[$foo]"
foo=[]
上面代码执行后(make var-lost), 取不到foo的值.

因为两行命令在两个不同的进程执行. 一个解决办法是将两行命令写在一行, 中间用分号分隔.
var-kept:
		export foo=bar; echo "foo=[$$foo]"
[chenchen@grpc01 mf_all]$ make -f Makefile_kept 
export foo=bar; echo "foo=[$foo]"
foo=[bar]

另一个解决办法是在换行符前加反斜杠转义.
var-kept:
		export foo=bar; \
		echo "foo=[$$foo]"
[chenchen@grpc01 mf_all]$ make -f Makefile_kept2
export foo=bar; \
echo "foo=[$foo]"
foo=[bar]

最后一个方法是加上 .ONESHELL: 命令.
.ONESHELL:
var-kept:
		export foo=bar;
		echo "foo=[$$foo]"
[chenchen@grpc01 mf_all]$ make -f Makefile_kept3 
export foo=bar;
echo "foo=[$foo]"
foo=[bar]

三、Makefile文件的语法
3.1 注释
井号(#) 在 Makefile 中表示注释.
# 这是注释
result.txt: source.txt
# 这是注释
	cp source.txt result.txt # 这也是注释

3.2 回声(echoing)
正常情况下, make 会打印每条命令, 然后再执行, 这就叫做回声(echoing).
test:
	[tab]# 这是测试
执行上面的规则, 会得到下面的结果.
[chenchen@grpc01 mf_all]$ make -f Makefile_kept4
# 这是测试2
# 这是测试3
在命令的前面加上@, 就可以关闭回声.
test:
	[tab]@# 这是测试2
	[tab]# 这是测试3
现在再执行 make test, 就不会有任何输出.
[chenchen@grpc01 mf_all]$ make -f Makefile_kept4
# 这是测试3

由于在构建过程中, 需要了解当前在执行哪条命令, 所以通常只在 注释 和 纯显示的echo 命令前面加上@.
test:
@# 这是测试
@echo TODO

3.3 通配符
通配符(wildcard)用来指定一组符合条件的文件名. 
Makefile 的通配符与 Bash 一致, 主要有星号(*)、问号(？) 和 [...], 比如, *.o 表示所有后缀名为 o 的文件.
clean:
	[tab]rm -f *.o

3.4 模式匹配
Make命令允许对文件名, 进行类似正则运算的匹配, 主要用到的匹配符是%.
比如, 假定当前目录下有 f1.c 和 f2.c 两个源码文件, 需要将它们编译为对应的对象文件.
%.o: %.c
等同于下面的写法.
f1.o: f1.c
f2.o: f2.c
使用匹配符%, 可以将大量同类型的文件, 只用一条规则就完成构建.

3.5 变量 和 赋值符
Makefile 允许使用等号自定义变量.
	[tab]txt = Hello World
test:
	[tab]@echo $(txt)
上面代码中, 变量 txt 等于 Hello World. 调用时, 变量需要放在 $( ) 之中.

调用 Shell 变量, 需要在美元符号前, 再加一个美元符号, 这是因为Make命令会对美元符号转义.
test:
	[tab]@echo $$HOME

有时, 变量的值可能指向另一个变量.
v1 = $(v2)
上面代码中, 变量 v1 的值是另一个变量 v2.
这时会产生一个问题, v1 的值到底在定义时扩展(静态扩展), 还是在运行时扩展(动态扩展)?
如果 v2 的值是动态的, 这两种扩展方式的结果可能会差异很大.

为了解决类似问题, Makefile 一共提供了四个赋值运算符 (=、:=、？=、+=), 它们的区别请看 StackOverflow.
VARIABLE = value
# 在执行时扩展, 允许递归扩展. (动态扩展)

VARIABLE := value
# 在定义时扩展. (静态扩展)

VARIABLE ?= value
# 只有在该变量为空时才设置值.

VARIABLE += value
# 将值追加到变量的尾端.

3.6 内置变量(Implicit Variables)
Make命令提供一系列内置变量, 比如, 
$(CC) 指向当前使用的编译器, 
$(MAKE) 指向当前使用的 Make 工具. 
这主要是为了跨平台的兼容性, 详细的内置变量清单见手册.
output:
	[tab]$(CC) -o output input.c

3.7 自动变量(Automatic Variables)
Make命令还提供一些自动变量, 它们的值与当前规则有关. 
主要有以下几个:
(1) $@
$@ 指代当前目标, 就是Make命令当前构建的那个目标.
比如, make foo 的 $@ 就指代 foo.
a.txt b.txt:
	[tab]touch $@
等同于下面的写法.
a.txt:
	[tab]touch a.txt
b.txt:
	[tab]touch b.txt

(2) $<
$< 指代第一个前置条件. 
比如, 规则为 t: p1 p2, 那么 $< 就指代p1.
a.txt: b.txt c.txt
	[tab]cp $< $@
等同于下面的写法.
a.txt: b.txt c.txt
	[tab]cp b.txt a.txt

(3) $?
$? 指代比目标更新的所有前置条件, 之间以空格分隔.
比如, 规则为 t: p1 p2, 其中 p2 的时间戳比 t 新, $? 就指代p2.

(4) $^
$^ 指代所有前置条件, 之间以空格分隔.
比如, 规则为 t: p1 p2, 那么 $^ 就指代 p1 p2.

(5) $*
$* 指代匹配符 % 匹配的部分, 比如 % 匹配 f1.txt 中的 f1, $* 就表示 f1.

(6) $(@D) 和 $(@F)
$(@D) 和 $(@F) 分别指向 $@ 的目录名和文件名.
比如, $@ 是 src/input.c, 那么 $(@D) 的值为 src, $(@F) 的值为 input.c.

(7) $(
$( 所有的自动变量清单, 请看手册. 
下面是自动变量的一个例子.
dest/%.txt: src/%.txt
	[tab]@[ -d dest ] || mkdir dest
	[tab]cp $< $@
上面代码将 src 目录下的 txt 文件, 拷贝到 dest 目录下.
首先判断 dest 目录是否存在, 如果不存在就新建, 然后, $< 指代前置文件(src/%.txt), $@ 指代目标文件(dest/%.txt).

3.8 判断和循环
Makefile 使用 Bash 语法, 完成判断和循环.
ifeq ($(CC), gcc)
libs = $(libs_for_gcc)
else
libs = $(normal_libs)
endif
上面代码判断当前编译器是否 gcc, 然后指定不同的库文件.

LIST = one two three
all:
	[tab]for i in $(LIST); do \
	echo $$i; \
	done
# 等同于
all:
	[tab]for i in one two three; do \
	echo $i; \
	done
上面代码的运行结果.
[chenchen@grpc01 mf_all]$ make all -f Makefile_for
one
two
three
[chenchen@grpc01 mf_all]$ make -f Makefile_for
one
two
three

3.9 函数
Makefile 还可以使用函数, 格式如下.
$(function arguments)
# 或者
${function arguments}

Makefile 提供了许多内置函数, 可供调用.
下面是几个常用的内置函数.
(1) shell 函数
shell 函数用来执行 shell 命令
srcfiles := $(shell echo src/{00..99}.txt)

(2) wildcard 函数
wildcard 函数用来在 Makefile 中, 替换 Bash 的通配符.
srcfiles := $(wildcard src/*.txt)

(3) subst 函数
subst 函数用来文本替换, 格式如下.
$(subst from, to, text)
下面的例子将字符串 "feet on the street" 替换成 "fEEt on the strEEt".
$(subst ee,EE,feet on the street)
下面是一个稍微复杂的例子.
comma := ,
empty :=
# space 变量用两个空变量作为标识符, 当中是一个空格
space := $(empty) $(empty)
foo := a b c
bar := $(subst $(space), $(comma), $(foo))
# bar is now `a,b,c'.

(4) patsubst 函数
patsubst 函数用于模式匹配的替换, 格式如下.
$(patsubst pattern,replacement,text)
下面的例子将文件名 "x.c.c bar.c", 替换成 "x.c.o bar.o".
$(patsubst %.c, %.o, x.c.c bar.c)

(5) 替换后缀名
替换后缀名函数的写法是: 变量名 + 冒号 + 后缀名替换规则.
它实际上 patsubst 函数的一种简写形式.
min: $(OUTPUT: .js = .min.js)
上面代码的意思是, 将变量 OUTPUT 中的后缀名 .js 全部替换成 .min.js.

四、Makefile 的实例
(1) 执行多个目标
.PHONY: cleanall cleanobj cleandiff

cleanall: cleanobj cleandiff
	rm program

cleanobj:
	rm *.o

cleandiff:
rm *.diff
上面代码可以调用不同目标, 删除不同后缀名的文件, 也可以调用一个目标(cleanall), 删除所有指定类型的文件.

(2) 编译 C 语言项目
edit: main.o kbd.o command.o display.o
	cc -o edit main.o kbd.o command.o display.o

main.o: main.c defs.h
	cc -c main.c

kbd.o: kbd.c defs.h command.h
	cc -c kbd.c

command.o: command.c defs.h command.h
	cc -c command.c

display.o: display.c defs.h
	cc -c display.c

clean:
	rm edit main.o kbd.o command.o display.o

.PHONY: edit clean

今天, Make命令的介绍就到这里. 
下一篇文章我会介绍, 如何用 Make 来构建 Node.js 项目.


````





## FAQ

```shell
1. Makefile:2: *** missing separator.  Stop.

编辑 Makefile 文件
vim Makefile
result.txt: source.txt
    cp source.txt result.txt

makefile 文件规则中有要求, 命令行前面必须是tab键, 所以在. vimrc 中必须设置 tab 是不会自动转换为空格.
如果命令行是用 tab 键来缩进, 那命令行就会变颜色.

2. 
其中-l是指加入的某个库的意思，-lssl 表示加入的库是libssl.so或者是libssl.a的意思，其中lib和扩展名可以不写

至于搜索路径可以-L/lib -L/usr/lib  其中-L/path是搜索路径的意思，可以用find / -name libssl.*查找下在哪个路径下
openssl提供两个库，如果二次开发的话，需要-lssl -lcrypto，注意有先后顺序，而如果顺序反了的话，就会出现例如：

对‘OPENSSL_sk_new_null’未定义的引用等一大推的未定义的引用。
连接库的时候一直分不清这几个的作用

总结一下
-L, 指定库文件目录，可以指定多个文件目录。库目录没有在/lib、/usr/lib、/usr/local/lib中，则必须用-L来指定一个库目录
-l(小写L), 指定具体的库文件。如果没有指定，则默认去/lib、/usr/lib、/usr/local/lib去找。默认寻找的是动态库，可以指定-static,寻找静态库。
-I(大写i), 指定头文件目录
所以，如果是我们想用一个任意文件夹下的库文件，一般做法就是
gcc  xxx.c  -o a.out  -L  库目录  -l(小写L)  具体的库文件名  -l(大写i)  库的头文件

gcc   - 参数

-I ( i 的大写)  ：指定头文件路径（相对路径或觉得路径，建议相对路径）

-i               ：指定头文件名字 (一般不使用，而是直接放在**.c 文件中通过#include<***.h> 添加)

-L              ：指定连接的动态库或者静态库路径（相对路径或觉得路径，建议相对路径）

-l (L的小写)：指定需要链接的库的名字（链接 libc.a :-lc       链接动态库：libc.so  : -lc   注意：-l后面直接添加库名省区“lib”和“.so”或“.a”  ）

 

问题：
问题1：-l(L的小写)链接的到底是动态库还是静态库

答案：如果链接路径下同时有 .so 和 .a  那优先链接 .so  

问题2：如果路径下同时有静态库和动态库如何链接静态库

答案：使用显示链接,       gcc      -l:lib***.a    (将静态库的名字显示写出来)

或者在 gcc 编译的时候 加入参数 -static -lXXX, 则可以添加路径下面的静态库。

验证方法：

可以通过 ldd 命令查看生成的 目标文件链接的库，使用方法： ldd  ***.o

参考：
1.https://blog.csdn.net/youqika/article/details/54617525

2.https://www.cnblogs.com/benio/archive/2010/10/25/1860394.html

3.gcc生成静态库和动态库：https://www.cnblogs.com/woainilsr/archive/2013/07/10/3182891.html

4.静态库和动态库链接路径顺序：https://blog.csdn.net/qq_21034239/article/details/54382311

静态库链接时搜索路径顺序：

1. ld会去找GCC命令中的参数-L
2.再找gcc的环境变量LIBRARY_PATH （用法：LIBRARY_PATH= path）
3.再找内定目录 /lib    /usr/lib    /usr/local/lib这是当初compile gcc时写在程序内的 （因系统版本而定  ：/lib64）

动态库链接时、执行时搜索路径顺序:
1. 去找GCC命令中的参数-L
2. 环境变量LD_LIBRARY_PATH指定的动态库搜索路径 （LD_LIBRARY_PATH=path）
3. 配置文件/etc/ld.so.conf中指定的动态库搜索路径 （修改/etc/ld.so.conf文件，将路径添加进去，运行/sbin/ldconfig）
4.默认的动态库搜索路径/lib（因系统版本而定：/lib64）
5.默认的动态库搜索路径/usr/lib（因系统版本而定）
头文件搜索路径：

1. 去 -I( i 的大写 ) 指定的路径
2. 源程序头（#include ""）文件中指定的路径
3.  /usr/include
4.  /usr/local/include
有关环境变量：

LIBRARY_PATH环境变量：指定程序静态链接库文件搜索路径
LD_LIBRARY_PATH环境变量：指定程序动态链接库文件搜索路径
    （这部分来源：https://blog.csdn.net/qq_21034239/article/details/54382311 ，并修改）

5.Linux中(.a/.la/.so/.o) 区别：http://www.cnblogs.com/findumars/p/5421910.html
```





## See Also

https://blog.csdn.net/weixin_39615402/article/details/116646619

https://blog.csdn.net/lyndon_li/article/details/120814771







