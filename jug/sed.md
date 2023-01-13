# sed

----

[toc]







## Overview

sed 是 Stream Editor (字符流编辑器)的缩写, 简称流编辑器.

处理流程如下: (类似于 awk)

- 一次处理一行内容, 把当前处理的行缓存在临时缓存区中, 称为 “模式空间”, 用 sed 命令处理缓存区的内容, 处理完成后, 把缓冲区的内容送往屏幕. sed 默认不会修改源文件数据.
- 当一行数据匹配完成后, 它会继续读取下一行数据, 并重复这个过程, 直到将文件中所有数据处理完毕.

```shell
sed [OPTION]... {script} [input-file]...
{script}: 为地址命令
```



sed 命令的选项如下:

| 选项 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| -n   | 默认情况下，sed 会在所有的脚本执行完毕后，会自动输出处理后的内容，而该选项会屏蔽自动输出，需使用 p 命令来完成输出. 取消默认 sed 的输出, 常与 sed 内置命令的 p 连用. |
| -e   | 多点编辑，逻辑与，需同时匹配前后两者内容才会输出             |
| -f   | 从指定文件中读取编辑文件                                     |
| -r   | 支持使用扩展正则表达式，默认支持标准正则表达式               |
| -i   | 直接修改文件内容, 而不是输出到终端.                          |



地址定界和编辑命令

- 地址定界

  | 地址定界符    | 说明                     |
  | ------------- | ------------------------ |
  | 不给地址      | 默认进行全文处理         |
  | #             | 指定的#行                |
  | $             | 最后一行                 |
  | /pattern/     | 被此模式匹配到的每一行   |
  | m,n           | 从第m行到第n行           |
  | m,+n          | 从第m行到m+n行           |
  | /pat1/,/pat2/ | 从模式1的行到模式2的行   |
  | m,/pat1/      | 从第m行到被模式1匹配的行 |
  | ~             | 步进                     |

- 编辑命令(内置命令)

  | 命令      | 选项                                                         |
  | --------- | ------------------------------------------------------------ |
  | d         | 删除模式空间的行，并开启下一行                               |
  | p         | 打印当前模式匹配的行，追加到指定行之后                       |
  | a [\]text | 在指定的行后面追加文本，\用于读特殊符号转义，\n换行实现多行文本追加 |
  | i [\]text | 在指定的行之前追加文本                                       |
  | c [\]text | 把指定的行替换为text文本                                     |
  | w FILE    | 保存模式匹配的行到FILE文件                                   |
  | r FILE    | 读取指定文件的行到模式空间中，匹配到指定行后                 |
  | !         | 模式空间的行取反处理                                         |
  | =         | 为模式空间的行打印行号                                       |
  | s         | 替换                                                         |
  | g         | 全局 global                                                  |
  
  

sed
Sed是操作、过滤和转换文本内容的强大工具。
常用功能有对文件实现快速增删改查（增加、删除、修改、查询），
其中查询的功能中最常用的2大功能是过滤（过滤指定字符串）和取行（取出指定行）。

sed [选项] [sed内置命令字符] [文件]

选项：
-n 取消默认sed的输出，常与sed内置命令的p连用※
-i 直接修改文件内容，而不是输出到终端。

如果不使用-i选项sed只是修改在内存中的数据，并不会影响磁盘上的文件※

sed的内置命令字符说明
s 替换
g 全局global
p 打印print
d 删除delete

 

[root@oldboyedu ~/test]# cat oldgirl.txt
I am oldboy teacher!
I like badminton ball ,billiard ball and chinese chess!
our site is http://www.oldboyedu.com
my qq num is 49000448.

功能：增删改查



## sed, 增删改查

### 查找

```shell
sed -n '2,3p' oldgirl.txt
sed -n 2,3p oldgirl.txt

-n 屏蔽输出, 要想输出只能显式指定打印 p
2,3p 打印2~3行, p 是显式的指定打印

sed -n '/hash/p' json
sed -n /hash/p json

```

### 删改

```shell
sed '/oldboy/d' oldgirl.txt
sed /oldboy/d oldgirl.txt
d 删除
sed -i '3d' oldgirl.txt

替换类似于 vim 的替换
vim替换：
:%s#oldboy#oldgirl#g

sed 's#想替换啥#用啥替换#g' oldgirl.txt
sed 's#oldboy#oldgirl#g' oldgirl.txt 
s 替换
g 全局


修改文件:
sed -i 's#oldboy#oldgirl#g' oldgirl.txt
-i 修改源文件

将文件中的 oldboy 字符串全部替换为 oldgirl, 同时将 QQ 号码 49000448 改为 31333741.
sed -e 's#oldboy#oldgirl#g' -e 's#49000448#31333741#g' oldgirl.txt
-e 逻辑与 and &&, 同时满足

```

### 增加

```shell
sed '2a I teacher linux.' oldgirl.txt
a 在后面追加, 在第二行后面加一行

sed '2i I teacher linux.i' oldgirl.txt
i 在前面增加, 在第二行前面增加一行
```



















## FAQ





## See Also

