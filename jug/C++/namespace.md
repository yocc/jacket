# namespace

[toc]

## Overview

定义命名空间
```c++
namespace 名称 {
常量、变量、函数等对象的定义
}
```
定义命名空间要使用关键字 namespace, 例如: 
```c++
namespace people {
		string name;
		int age;
} 
```
在上述代码中, people 就是定义的命名空间的名称, 在大括号中定义了一个字符串变量 name 和整型变量 age, 那么这两个变量就是属于命名空间 people 范围内的.

在一个应用程序的多个文件中可能会存在同名的全局对象, 这样会导致应用程序的链接错误. 使用命名空间是消除命名冲突的最佳方式. 
```c++
namespace people1 {
		string name;
		int age;
} 
namespace people2 {
		string name;
		int age;
} 
```
在上面的代码中, 声明了 2 个命名空间 people1 和 people2. 其中都定义个 string 变量 name 和 int 变量 age. 虽然两个命名空间中定义的变量都是一样的, 但因为在不同的命名空间中, 所以避免了标识符的冲突, 保证了标识符的唯一性.

例如: 
```c++
people1::name = "树先生"；
peole2::name = "树哥“；
```
通过命名空间的方法, 虽然定义相同名称的变量表示不同的值, 但是也可以正确的进行引用显示.

使用作用域限定符 "::" 来引用空间中的成员. 一般形式为: 命名空间的名称::成员

例如上面的 people1 命名空间, 引用其 name 属性
```c++
people::name = "树先生";
```
还有另一种引用命名空间中成员的方法, 就是使用 using namespace 语句. 一般形式为:

using namespace 命名空间名称, 例如
```c++
#include <iostream>

namespace car {					// 注意此处命名空间的声明要在 using namespace 之前
		int wheels = 10;
}
using namespace car;

int main() {
    std::cout << wheels << std::endl;			// 输出语句
    return 0;
}
```
在引用空间中的成员是可以直接使用的.

在多个文件中定义命名空间

在定义命名空间时, 通常在头文件中声明命名空间中的函数, 在源文件中定义命名空间中的杉树实现, 将程序的声明和实现分开. 例如在头文件(Header.h)声明命名空间函数(show):
```c++
// Header.h
namespace Demo {
		void show();
}
```
在源文件定义函数:
```c++
#include "Header.h"					// 注意需要引入头文件
void Demo::show() {
    cout<<"showTime"<<endl;
}
```
在源文件定义函数时, 注意要使用命名空间名作为前缀, 表明实现的是命名空间中定义的函数, 否则将是定义一个全局的函数. 将以上程序结合到一起看下:
```c++
#include <iostream>
#include "Header.h"					// 引入头文件
using namespace std;

void Demo::show() {					// 实现命名空间函数
    cout<<"showTime"<<endl;
}
int main(int argc, const char * argv[]) {
    Demo::show();						// 调用命名空间里的函数
    
    return 0;
}
```
命名空间的嵌套

命名空间可以定义在其他的命名空间中, 在这种情况下, 仅仅通过使用外部的命名空间作为前缀, 程序便可以引用在命名空间之外定义的其他标识符. 然而, 在命名空间内定义的标识符需要作为外部命名空间和内部命名空间的名称的前缀一起调用. 格式:

外部的命名空间名称::内部的命名空间名称::内部的命名空间定义的变量/函数名

```c++
#include <iostream>
using namespace std;
namespace people1 {
    void showPeople1() {
        std::cout << "命名空间people1里的函数showPeople1" << endl;
    }

    namespace people2 {
        void showPeople2() {
            std::cout << "命名空间people2里的函数showPeople2" << endl;
        }
    } 										// namespace people2
} 												// namespace people1

int main() {
    // 调用命名空间 people1 里的函数
    people1::showPeople1();
    // 调用命名空间 people2 里的函数
    people1::people2::showPeople2();

    return 0;
}

输出结果: 
命名空间 people1 里的函数 showPeople1
命名空间 people2 里的函数 showPeople2
```






## FAQ





## See Also

https://blog.csdn.net/csj731742019/article/details/126079711?spm=1001.2014.3001.5502