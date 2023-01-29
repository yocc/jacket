# Shell

----

[toc]





## Overview

### 符号

$, !$, ~

& 丢到后台执行

```shell
$ sleep 200 &
[1] 2965
$ jobs
[1]+  运行中               sleep 200 &
```

; 多条命令写一行，用分号连接

```shell
$ mkdir awen ; cd awen ; pwd
/tmp/awen
```

|| 或者, 前面的代码正确, 就不执行后面的程序了.

```shell
$ ls
a.txt  awen  c.txt
$ ls d.txt || echo "cant't find d"
ls: 无法访问'd.txt': No such file or directory
cant't find d
```

&& 和 || 相反, 它是如果前面的命令成功才执行后面的命令.

```shell
$ ls a.txt && echo "a.txt exist"
a.txt
a.txt exist
```







## FAQ





## See Also





