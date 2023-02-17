# daemon

---



[toc]



## Overview

### 库

因为种种原因，在go语言中我们无法很好的直接操作 fork 调用。我们转换一下思路，启动自身为一个子进程，也可以看做是调用外部程序。标准库中找到下面三种方法: - [syscall.ForkExec](https://link.zhihu.com/?target=https%3A//studygolang.com/static/pkgdoc/pkg/syscall.htm%23ForkExec) - [os.StartProcess](https://link.zhihu.com/?target=https%3A//studygolang.com/static/pkgdoc/pkg/os.htm%23StartProcess) - [exec.Cmd](https://link.zhihu.com/?target=https%3A//studygolang.com/static/pkgdoc/pkg/os_exec.htm%23Cmd)

`syscall.ForkExec`的文档说明不是很多，我没有深入研究。阅读那些开源库，发现基本都是使用`os.StartProcess`或`exec.Cmd`。 `os.StartProcess`在文档里有明确说明，这是一个低水平的接口，建议使用os/exec包提供的高水平接口，也就是`exec.Cmd`。





## FAQ







## See Also

https://zhuanlan.zhihu.com/p/146192035

