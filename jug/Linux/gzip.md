# gzip

[toc]



## Overview

gzip 命令用于将一个大的文件通过压缩算法 (Lempel-Ziv coding) 变成一个小的文件.

gzip 命令不能直接压缩目录, 因此目录需要先用 tar 包打成一个文件, 然后 tar 包再调用 gzip 进行压缩.



### OPTION

选项与参数

- `-d` 解开压缩文件
- `-v` 显示指令执行的过程
- `-l` 列出压缩文件的内容信息
- `-c` 将内容输出到标准输出 (屏幕), 不改变原始文件, 可通过数据流重导向来处理
- `-r` 对目录下的所有文件递归进行压缩操作
- `-#` 指定压缩率, #为数字, 范围为1-9, 默认为6, 值越大压缩率越高
- `-t` 测试, 检查压缩文件是否完整

```shell
[chenchen@dev3_10.211.21.18 clash]$ gzip --help
Usage: gzip [OPTION]... [FILE]...
Compress or uncompress FILEs (by default, compress FILES in-place).

Mandatory arguments to long options are mandatory for short options too.

  -c, --stdout      write on standard output, keep original files unchanged
  -d, --decompress  decompress
  -f, --force       force overwrite of output file and compress links
  -h, --help        give this help
  -l, --list        list compressed file contents
  -L, --license     display software license
  -n, --no-name     do not save or restore the original name and time stamp
  -N, --name        save or restore the original name and time stamp
  -q, --quiet       suppress all warnings
  -r, --recursive   operate recursively on directories
  -S, --suffix=SUF  use suffix SUF on compressed files
  -t, --test        test compressed file integrity
  -v, --verbose     verbose mode
  -V, --version     display version number
  -1, --fast        compress faster
  -9, --best        compress better
    --rsyncable   Make rsync-friendly archive

With no FILE, or when FILE is -, read standard input.
```

### Compress

```sh
$ gzip *.html
// 1.html.gz, 2.html.gz

说明:
后缀 .gz 是自动添加的
gzip 命令的缺点是压缩后源文件不见了, 它的特性是压缩, 解压都会自动删除源文件

$ gzip -l *.gz
// 查看压缩文件的信息
说明:
因为源文件都是空文件, 所以压缩率都为0.0%

```

### Uncompress

#### 压缩, 解压缩保留源文件

```shell
# 准备一个文件
cp /etc/services .
ll -h

# 压缩
gzip -c services > services.gz
ll -h
gzip -l services.gz

# 解压
gzip -dc services.gz > services2
ll -h
# 对比源文件和解压后的文件是有差别
diff services services2
```



## FAQ



## See Also



