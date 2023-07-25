# with

[toc]



## Overview

with 语句, 用于上下文管理器 context manager 定义的方法包装块的执行.

在处理非托管资源 unmanaged resources (如文件流)时使用 with 关键字. 

他确保在使用资源的代码完成运行时, “清理” 资源, 即使抛出异常也是如此. 

他为 try/finally 块提供语法糖 syntatic sugar.

非常适合用于对资源进行访问的场景, 确保不管使用过程中是否发生异常都会执行必要的 “清理” 操作, 释放资源, 比如文件使用后自动关闭、线程中锁的自动获取和释放等.

要在用户自定义对象中使用 with 语句, 只需要在对象实现的方法中加入 `__enter__()` 和 `__exit__()` 方法.

```sh
with 语法
with 表达式 [as 目标]:
		SUITE
```

```py
若想要导入的模块不在 sys.path 包含的路径 (当前目录, 内置模块, 第三方模块) 中.
需要用 sys.path.append('路径名') 添加到搜索目录中.

import sys
## 将上级目录加入python系统路径
sys.path.append(r'..')
# print(sys.path)	# 查看所有搜索路径
sys.exit()
from utils.common.env import env01

# 
from 包名就是目录名.模块名就是文件名 import 类名
```

```py

```







## Troubleshooting





## FAQ





## See Also

https://wenku.baidu.com/view/efc15d4ea75177232f60ddccda38376bae1fe043.html?_wkts_=1688109471704