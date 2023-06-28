# Solving The “Cannot Open Shared Object File: No Such File Or Directory” Error In Linux, 解决Linux中的“无法打开共享目标文件：没有这样的文件或目录”错误



[toc]



## Overview

```sh
[chenchen@dev3_10.211.21.18 bin]$ python3 -V
python3: error while loading shared libraries: libpython3.9.so.1.0: cannot open shared object file: No such file or directory
```

```sh
1. 先找到缺少的 .so 文件 (本地找, 还是安装均可, 总之要有这个文件)
2. 解决办法:
2.1. (方法1) (临时) 配置环境变量, LD_LIBRARY_PATH
2.2. (方法2) (永久) sudo 编辑 /etc/ld.so.conf 文件, 加入 .so 所在目录; 然后执行 sudo ldconfig -vvv; 然后执行 ldconfig -p 清缓存; 然后验证 python3 -V
```

### What Are Shared Libraries?

**[Shared libraries](https://www.baeldung.com/linux/a-so-extension-files#2-shared-libraries)**: Linux 中的共享库为程序提供了各种可重用的功能.

### ldd

使用 ldd 命令列出程序使用的所有共享库:

```sh
[chenchen@dev3_10.211.21.18 bin]$ pwd
/usr/home/chenchen/htdocs/python3916/bin
[chenchen@dev3_10.211.21.18 bin]$ l
total 52K
45230528 -rwxr-xr-x 1 chenchen chenchen  17K Jun 25 17:38 python3.9
45230532 -rwxrwxr-x 1 chenchen chenchen  110 Jun 25 17:39 pydoc3.9
45230531 -rwxrwxr-x 1 chenchen chenchen  125 Jun 25 17:39 idle3.9
45230530 -rwxrwxr-x 1 chenchen chenchen  127 Jun 25 17:39 2to3-3.9
45230529 -rwxr-xr-x 1 chenchen chenchen 3.1K Jun 25 17:39 python3.9-config
23396831 drwxrwxr-x 7 chenchen chenchen 4.0K Jun 25 17:39 ..
45230533 lrwxrwxrwx 1 chenchen chenchen    9 Jun 25 17:39 python3 -> python3.9
45230534 lrwxrwxrwx 1 chenchen chenchen   16 Jun 25 17:39 python3-config -> python3.9-config
45230535 lrwxrwxrwx 1 chenchen chenchen    7 Jun 25 17:39 idle3 -> idle3.9
45230536 lrwxrwxrwx 1 chenchen chenchen    8 Jun 25 17:39 pydoc3 -> pydoc3.9
45230537 lrwxrwxrwx 1 chenchen chenchen    8 Jun 25 17:39 2to3 -> 2to3-3.9
45230568 -rwxrwxr-x 1 chenchen chenchen  255 Jun 25 17:39 pip3.9
45230567 -rwxrwxr-x 1 chenchen chenchen  255 Jun 25 17:39 pip3
45230527 drwxr-xr-x 2 chenchen chenchen 4.0K Jun 25 17:39 .
[chenchen@dev3_10.211.21.18 bin]$ ldd python3
        linux-vdso.so.1 =>  (0x00007ffcbe780000)
        libpython3.9.so.1.0 => not found
        libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007f5f1b9e0000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f5f1b7c4000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f5f1b5c0000)
        libutil.so.1 => /lib64/libutil.so.1 (0x00007f5f1b3bc000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f5f1b0ba000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f5f1acf9000)
        libfreebl3.so => /lib64/libfreebl3.so (0x00007f5f1aaf5000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f5f1bc2a000)

```

### 原因

#### 路径不对(包存在)

这里可以看到 python3 二进制程序需要各种库（例如 libpython3.9.so.1.0）才能运行.
如所见, `共享库` 以 .so 扩展名结尾.
有时, `共享库` 可能不存在或可能位于非标准路径中.

结果, 在程序启动时得到 “无法打开共享对象文件: 没有这样的文件或目录”.

“cannot open shared object file: No such file or directory” 

#### 缺少包

有时, 可能只是缺少提供所需共享库的包.
例如, 如果一个程序抱怨缺少 libzstd.so, 我们可以尝试在系统的包管理器中搜索它. 

搜索不到可以通过 yum 安装.

```sh
$ rpm -aq | grep zstd			(确认本地系统没有)
$ rpm -ql zstd				(确认本地系统没有)
$ sudo yum -y install zstd-xxxxx
```

### LD_LIBRARY_PATH 变量

可以在 LD_LIBRARY_PATH 环境变量中指定要搜索共享库的目录.
LD_LIBRARY_PATH 是以冒号分隔的目录列表, 就像 PATH 变量一样.

默认搜索路径通常限制为 /usr/lib 和 /usr/local/lib.
假设有一个链接到 libfoo.so 的程序, 位于 /home/baeldung/libs/libfoo.so, 它位于默认搜索路径之外.

然后, 可以使用此变量附加到搜索路径并解决问题.
首先, 让我们使用 ldd 命令确认程序链接到的确切库:

```sh
[chenchen@dev3_10.211.21.18 bin]$ python3 -V
python3: error while loading shared libraries: libpython3.9.so.1.0: cannot open shared object file: No such file or directory

[chenchen@dev3_10.211.21.18 bin]$ ldd python3
        linux-vdso.so.1 =>  (0x00007ffcbe780000)
        libpython3.9.so.1.0 => not found
        libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007f5f1b9e0000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f5f1b7c4000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f5f1b5c0000)
        libutil.so.1 => /lib64/libutil.so.1 (0x00007f5f1b3bc000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f5f1b0ba000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f5f1acf9000)
        libfreebl3.so => /lib64/libfreebl3.so (0x00007f5f1aaf5000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f5f1bc2a000)
```


现在，让我们将目录添加到LD_LIBRARY_PATH并使程序正常工作:

```sh
[chenchen@dev3_10.211.21.18 lib]$ echo $LD_LIBRARY_PATH

# edit ~/.bashrc 
# python python3916
export LD_LIBRARY_PATH=/data1/www/htdocs/chenchen/python3916/lib

[chenchen@dev3_10.211.21.18 lib]$ vim ~/.bashrc
[chenchen@dev3_10.211.21.18 lib]$ . ~/.bashrc
[chenchen@dev3_10.211.21.18 lib]$ echo $LD_LIBRARY_PATH
/data1/www/htdocs/chenchen/python3916/lib

[chenchen@dev3_10.211.21.18 bin]$ ldd python3
        linux-vdso.so.1 =>  (0x00007ffca1fa9000)
        libpython3.9.so.1.0 => /data1/www/htdocs/chenchen/python3916/lib/libpython3.9.so.1.0 (0x00007f2c7a758000)
        libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007f2c7a50f000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f2c7a2f3000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f2c7a0ef000)
        libutil.so.1 => /lib64/libutil.so.1 (0x00007f2c79eeb000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f2c79be9000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f2c79828000)
        libfreebl3.so => /lib64/libfreebl3.so (0x00007f2c79624000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f2c7ad2e000)
[chenchen@dev3_10.211.21.18 bin]$ python3 -V
Python 3.9.16
```


此外, 如果不知道库在哪里, 我们可以在常见路径(如 /home 或 /usr) 中使用 find 命令找到它:

```sh
find /home -type f -name libfoo.so
/home/baeldung/libs/libfoo.so
```

### 永久配置库路径

可以使用 `/etc/ld.so.conf` 文件来永久设置库搜索路径. 在此文件中, 我们指定一个换行符分隔的目录列表.
让我们再次修复 libpython3.9.so.1.0 错误:

```sh
[chenchen@dev3_10.211.21.18 bin]$ python3 -V
python3: error while loading shared libraries: libpython3.9.so.1.0: cannot open shared object file: No such file or directory

[chenchen@dev3_10.211.21.18 bin]$ l /etc/ld.so.conf
100 -rw-r--r--. 1 root root 52 Jul 20  2018 /etc/ld.so.conf
[chenchen@dev3_10.211.21.18 bin]$ sudo vim /etc/ld.so.conf			(编辑加一行)
/data1/www/htdocs/chenchen/python3916/lib			(加一行目录, 而不是具体.so文件)

[chenchen@dev3_10.211.21.18 bin]$ sudo ldconfig -vvv
[chenchen@dev3_10.211.21.18 bin]$ ldd python3
        linux-vdso.so.1 =>  (0x00007fff717e8000)
        libpython3.9.so.1.0 => not found
        libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007fe8e39ed000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fe8e37d1000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007fe8e35cd000)
        libutil.so.1 => /lib64/libutil.so.1 (0x00007fe8e33c9000)
        libm.so.6 => /lib64/libm.so.6 (0x00007fe8e30c7000)
        libc.so.6 => /lib64/libc.so.6 (0x00007fe8e2d06000)
        libfreebl3.so => /lib64/libfreebl3.so (0x00007fe8e2b02000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fe8e3c37000)
[chenchen@dev3_10.211.21.18 bin]$ ldconfig -p
[chenchen@dev3_10.211.21.18 bin]$ ldd python3
        linux-vdso.so.1 =>  (0x00007fffb95f3000)
        libpython3.9.so.1.0 => /data1/www/htdocs/chenchen/python3916/lib/libpython3.9.so.1.0 (0x00007fbb5833d000)
        libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007fbb58105000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fbb57ee9000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007fbb57ce5000)
        libutil.so.1 => /lib64/libutil.so.1 (0x00007fbb57ae1000)
        libm.so.6 => /lib64/libm.so.6 (0x00007fbb577df000)
        libc.so.6 => /lib64/libc.so.6 (0x00007fbb5741e000)
        libfreebl3.so => /lib64/libfreebl3.so (0x00007fbb5721a000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fbb58924000)
```

必须运行一次 ldconfig 命令以使系统知道新路径.

### ldconfig 命令

可能最近安装了新的共享库或修改了共享库搜索路径. 因此, 需要运行 ldconfig 命令.
它更新链接器的缓存, 使其知道新的共享库.
链接器(称为 ld.so) 加载程序的共享库.
我们可以调用带有 -p 标志的 ldconfig 来检查当前缓存:

```sh
[chenchen@dev3_10.211.21.18 bin]$ ldconfig -p
885 libs found in cache `/etc/ld.so.cache'
        p11-kit-trust.so (libc6,x86-64) => /lib64/p11-kit-trust.so
```

从这个输出中, 我们可以确定各种共享库的确切位置.

### 在编译时设置库路径

如果可以访问程序的源代码, 则可以在链接过程中使用特殊标志对其进行编译, 以便可以找到共享库.
我们可以在运行时使用 ld 的 -rpath 标志将库路径传递给动态链接器.
让我们编译一个基本程序并将其链接到我们的库:

```sh
pwd
/home/baeldung/libs
ls
libfoo.so
echo "int main() {}" > program.c  Dummy program
cc program.c libfoo.so # Link to our library
./a.out 
./a.out: error while loading shared libraries: libfoo.so: cannot open shared object file: No such file or directory
ldd ./a.out 
./a.out:
	linux-vdso.so.1 (0x00007ffcbc1f8000)
	libfoo.so => not found
	libc.so.6 => /usr/lib/libc.so.6 (0x00007feb0ee04000)
	/lib/ld-linux-x86-64.so.2 => /usr/lib/ld-linux-x86-64.so.2 (0x00007feb0f029000)
```

我们可以看到程序无法加载我们的共享库.
现在, 让我们尝试使用 -rpath 标志编译它, 将 /home/baeldung/libs 添加到库搜索路径中:

```sh
gcc program.c libfoo.so -Wl,-rpath=/home/baeldung/libs
ldd ./a.out 
./a.out:
	linux-vdso.so.1 (0x00007ffd0fbad000)
	libfoo.so => /home/baeldung/libs/libfoo.so (0x00007fba09d99000)
	libc.so.6 => /usr/lib/libc.so.6 (0x00007fba09b7b000)
	/lib/ld-linux-x86-64.so.2 => /usr/lib/ld-linux-x86-64.so.2 (0x00007fba09da5000)
```

在这里, 我们使用 gcc 的 -wl 标志将参数传递给 ld.

### 结论

在本文中, 我们介绍了缺少共享库的各种原因及其解决方案. 

此外, 我们还介绍了为程序加载共享库的动态链接器的工作原理。





## Troubleshotting





## FAQ





## See Also

https://www.baeldung.com/linux/solve-shared-object-error
