# awk

----



[toc]



## Overview

awk 按照行处理文本, **逐行扫描, 默认从第一行到最后一行**, 找到匹配到特定行, 并进行相关操作. 理解为 一条 awk 指令,  表示了一行的处理方式, $0 的作用域是第一行, 当第一行处理完毕后才会加载第二行, 当开始处理第二行时, $0 重新初始化, 再按 awk 指令 跑一遍, 直至所有行跑完.

```shell
awk 选项 '命令' 文件名

-F  定义字段分割符号，默认的分隔符是空格
-v  定义变量并赋值

awk -F: '/root/{print FIELNAME}' input.txt
awk -F: '/root/{print $0}' input.txt
```

awk内置变量

| 变量            | 变量说明                                     |
| --------------- | -------------------------------------------- |
| $0              | 当前处理行的所有记录                         |
| 1,1,2,3...3...n | 文件中每行以间隔符号分割的不同字段. $1第一列 |
| NF              | 当前记录的字段数（列数）                     |
| $NF             | 最后一列. ($NF-1)就是倒数第二列              |
| NR              | 行号                                         |
| FS              | 定义间隔符. BEGIN{FS=":"} 相当于使用选项 -F: |
| OFS             | 定义输出字段分隔符，默认空格                 |
| RS              | 输入记录(行)分割符，默认换行 (行分隔符)      |
| ORS             | 输出记录(行)分割符，默认换行 (行分隔符)      |
| FILENAME        | 当前输入的文件名                             |

```shell
F = Field
R = row
S = Separator
N = number
O = output
```



### awk工作原理

1. awk 使用一行作为输入, 并将这一行赋给内部变量 $0, 每一行也可称为一个记录, 以换行符(RS)结束

2. 每行被间隔符**==:==**(默认为空格或制表符)分解成字段, 每个字段存储在已编号的变量中, 从 $1 开始

3. 问: awk 如何知道用空格来分隔字段的呢？

   答: 因为有一个内部变量 FS 来确定字段分隔符. 初始时. FS赋为空格.

4. awk 使用 print 函数打印字段, 打印出来的字段会以空格分隔, 比如1,1,3之间有一个逗号, 但是逗号比较特殊, 它映射为另一个内部变量, 称为输出字段分隔符OFS, OFS默认为空格.

5. awk 处理完一行后, 将从文件中获取另一行, 并将其存储在 $0 中, 覆盖原来的内容, 然后将新的字符串分隔成字段并进行处理, 该过程将持续到所有行处理完毕.



### ; 分号

```shell
awk 'NR==1, NR==5;/^root/{print $0}' input.txt		
# 分号; 或者 or 关系, 1至5行或者root开头
```

### printf, 格式化输入

```shell
awk -F: '{printf "%-10s %-10s %-10s\n", $1,$2,$3}' input.txt		
# %s 字符串类型占位符, 默认右对齐, 前面加负号-左对齐
```

### BEGIN, END

```shell
BEGIN: 表示在程序开始前执行
END: 表示所有文件处理完后执行
用法: 'BEGIN{开始处理之前};{处理中};END{处理结束后}'

awk -F: 'BEGIN{print "NAME\tDIR\tSHELL\n***************************************"}{printf "%-10s %-10s %-10s\n",$1,$(NF-1),$NF}END{print "****************************************"}' input.txt
```

### 间隔符

```shell
BEGIN{FS=":"} 相当于使用选项 -F:
awk 'BEGIN{FS=":"};/^root/{print $1,$NF}' input.txt

OFS 输出间隔为两个制表符 \t
awk -F: 'BEGIN{OFS="\t\t"};/^root/{print $1,$NF}' input.txt

输入内容(行)以@分隔
awk 'BEGIN{RS="@"};{print $0}' input.txt

输出内容(行)指定以"++++"分隔
awk 'BEGIN{RS="@";ORS="++++"};{print $0}' input.txt

```

### 逻辑运算符

```shell
awk 'NR>1 || NR<4' input.txt
或|| 运算符, 打印第1行至第4行

awk 'NR==1 && NR==4' input.txt
使用 与&& 运算符打印第1行和第4行
```

### 流程控制

```shell
# if 条件判断
和其他编程语言一样, 包括选择结构和循环结构

单分支结构:
{if (表达式) {语句1;语句2;...}}
awk -F: '{if($3==0) {print $0,$1"是超级管理员"}}' input.txt
每行第三列等于0就打印第一列

双分支结构:
{if (表达式) {语句;语句;...} else {语句;语句;...}}
awk -F: '{if($3==0) {print $1"是超级管理员"} else {print $1"不是超级管理员"}}' input.txt

多分支结构:
{if (表达式1) {语句;语句;...} else if (表达式2) {语句;语句;...} else if (表达式3)｛语句;语句;...} else {语句;语句;...}}
awk -F: '{if($3==0) {print $1"是超级管理员"} else if ($3<=999) {print $1"是系统用户"} else {print $1"是普通用户"}}' input.txt
```

### 循环结构

```shell
awk 'BEGIN{i=1;for(i=1;i<=10;i++) {sum+=i;print i};{print sum}}' input.txt
awk 'BEGIN{i=1;while(i<=10){if(i=3)break;print i;i++}}' input.txt
break, continue
```

### 算术运算

```shell
awk 按浮点数进行数学运算
```

### 脚本运行

```shell
awk 作为 shell 脚本

#!/usr/bin/awk -f
BEGIN{FS=":"}
NR=1,NR=3{print $1"\t"$NF}
```







## FAQ





## See Also

https://www.toutiao.com/article/7186931023995929123/?app=news_article&timestamp=1673502997&use_new_style=1&req_id=2023011213563743D9FD210C5C0D4E7C1C&group_id=7186931023995929123&pseries_type=0&pseries_style_type=2&pseries_id=7185378539481498145&wxshare_count=1&tt_from=weixin&utm_source=weixin&utm_medium=toutiao_android&utm_campaign=client_share&share_token=ce030648-0ec4-47b9-915f-c2efc05a1c4b&source=m_redirect&wid=1673503889188