# perfermence 

[toc]

##

PHP静态方法与非静态方法在性能上有什么区别？
先贴代码如下：

复制代码
class class1
{
    public static function test(){}
}
class class2
{
    public function test(){}
}

var_export(-1111111111111111111);
//代码段1
//写法1
class1::test(); 
var_export(-1111111111111111111);
//代码段2
//写法2
$c2 = new class2(); 
$c2->test();
var_export(-1111111111111111111);
//代码段3
$c2->test();
var_export(-1111111111111111111);
复制代码
写法1和写法2到底有什么不同呢，其本质就是一个是静态方法，一个是非静态方法。

个人平时喜欢用写法1

　　原因一：只有一行，看着好看。

　　原因二：可能性能会好一些。

但性能的比较实在是无从下手，如果看执行时间的话，我想是永远也看不出来，因为时间肯定太小了，没法统计。

 

但想统计，总会有方法，可以使用gdb调试工具来统计c语言代码的行数来粗略的估计

原理：

php的var_export函数，在C代码中对应是sapi_cli_single_write。所以可以在sapi_cli_single_write函数上，设置一个断点。然后利用gdb的单步执行，来统计每个断点之间的代码行数。

步骤如下：

1，构造测试用的PHP脚本，如上方所示。

2，构造测试用的gdb脚本gdb.input，如下：

b sapi_cli_single_write
r /home/users/huangxuan01/test.php
s
s
....//以下是10000个s，代表10000个单步执行，也代表10000行C语言代码，应该够用了
解释：

b sapi_cli_single_write 就代表在var_export处设置断点

r /home/users/huangxuan01/test.php 就代表运行测试php脚本。

3，执行以下命令

gdb php < gdb.input  >gdb.output
4，统计gdb.output文件，如下：

复制代码
(gdb) -1111111111111111111271           } while (ret <= 0 && errno == EAGAIN && sapi_cli_select(STDOUT_FILENO TSRMLS_CC));
(gdb) 270                       ret = write(STDOUT_FILENO, str, str_length);
(gdb) 271               } while (ret <= 0 && errno == EAGAIN && sapi_cli_select(STDOUT_FILENO TSRMLS_CC));
(gdb) 277               return ret;

........



(gdb) -1111111111111111111271           } while (ret <= 0 && errno == EAGAIN && sapi_cli_select(STDOUT_FILENO TSRMLS_CC));
(gdb) 270                       ret = write(STDOUT_FILENO, str, str_length);
(gdb) 271               } while (ret <= 0 && errno == EAGAIN && sapi_cli_select(STDOUT_FILENO TSRMLS_CC));
(gdb) 277               return ret;
复制代码
重点是-11111111111111，已经飘红。然后统计gdb.output中，-1111111111111之间的行数，就可以粗略的统计写法1和写法2的性能了，结果如下：

代码段1：754

代码段2：1174

代码段3：747　　

可以看出来写法2比写法1多执行了55.7%的代码，因为C语言要经过编译，所以只能说是粗略的统计代码的行数。

所以为了美观和性能的考虑，我还是坚持写法1。

至于到底有什么区别，那就得熟悉熟悉PHP的实现了。



## See Also

https://www.cnblogs.com/hxdoit/p/5193598.html







