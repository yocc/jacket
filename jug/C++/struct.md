# struct

[toc]

## Overview

程序结构代码

[结构体](https://baike.baidu.com/item/结构体/3709485?fromModule=lemma_inlink)就是一个可以包含不同[数据类型](https://baike.baidu.com/item/数据类型/10997964?fromModule=lemma_inlink)的一个结构, 它是一种可以自己定义的数据类型, 它的特点和数组主要有两点不同: 

首先, 结构体可以在一个结构中声明不同的数据类型.

第二, 相同结构的结构体变量是可以相互赋值的, 而数组是做不到的, 因为数组是单一数据类型的[数据集](https://baike.baidu.com/item/数据集/4745883?fromModule=lemma_inlink)合, 它本身不是数据类型(而结构体是), 数组名称是[常量指针](https://baike.baidu.com/item/常量指针/1673669?fromModule=lemma_inlink), 所以不可以做为[左值](https://baike.baidu.com/item/左值/2327412?fromModule=lemma_inlink)进行运算, 所以数组之间就不能通过数组名称相互复制了, 即使数据类型和数组大小完全相同.

整型(int)、长整型(long long)、字符型(char)以及浮点型(float)等这些数据类型指南记录单一的数据, 而这些数据只能被称为`基础数据类型`.

如果需要定义某种类型, 同时包含以上几种的基本数据类型. 比如一个人同时含有身高、体重以及年龄的属性. 而结构体就是将这些变量类型包含在一起, 大大减少程序代码的离散性, 使程序代码阅读更加符合逻辑. 

### 结构体定义

```c++
// 定义
struct 结构体类型名称
{
		成员类型 成员名；
		...
		成员类型 成员名；
};

/**
"struct" 就是定义结构体的关键字.
"结构体类型名称" 就是一种标识符, 该标识符表示一个新的变量. 
结构体是以大括号({})括起来且以分号(;)结尾. 
*/

// 例如定义一个学生结构体，包含年龄、分数和姓名属性.
struct Student {
    string name; //学生姓名
    int age;     //学生年龄
    int score;   //学生分数
};
```

> ⚠️ 注意: 结构体由不同类型的数据组成的数据集合, 而数组是相同元素的集合.



### 结构体声明

#### 常规声明

```c++
// 声明一个名为 Student1 的结构体
struct Student1 {
    int age;
};
```

#### 在声明之后立刻创建

```c++
// 声明一个变量
struct Student2 {
    int age;
} stu2;

// 也可以同时声明多个变量
struct Student2 {
    int age;
} stu2, stu22;
```

#### typedef声明(类型别名声明)

```C++
typedef struct Student3 {
    int age;
} stu3;
```

#### 匿名结构体

```c++
struct {
    int age;
} stu4;
```

#### 匿名结构体+typedef

```c++
typedef struct {
    int age;
} Student5;
```





### 结构体成员及初始化

引用结构体成员有两种方式, 一种是声明[结构体变量](https://so.csdn.net/so/search?q=结构体变量&spm=1001.2101.3001.7020)后, 通过成员运算符"."引用; 一种是声明结构体指针变量, 使用指向"->"运算符引用.

#### 使用"."应用结构体成员

```c++
struct Student {
    string name;
    int score;
    int age;
};

int main() { 
    struct Student student;			//struct可以省略
    student.age = 18;
    student.name = "树哥";
    student.score = 60;
    return 0;
}

// 注意: 结构体可以在定义时直接对结构体变量赋值, 例如:
struct Student student2 = { "树哥",60,18};
```

#### 在定义结构体时, 可以同时声明[结构体指针](https://so.csdn.net/so/search?q=结构体指针&spm=1001.2101.3001.7020)变量, 例如:

```c++
struct Student {
    string name;	// 姓名
    int score;		// 分数
    int age;			// 年龄
} *student;
 
int main() {
    student->name = "树哥";
    student->age = 18;
    student->score = 60;
    return 0;
}
```

> ⚠️ 注意: 指针结构体指针只有初始化后才可以使用.



### 结构体与函数

结构体数据类型在 C++ 语言中是可以作为函数参数传递的, 可以直接使用结构体变量做函数的参数, 也可以使用结构体指针变量做函数参数.

#### 结构体变量做函数参数

```c++
#include <iostream>
using namespace std;
 
// 定义学生结构体定义
struct student {
    string name;
    int age;
    int score;
};

// 自定义函数, 打印学生信息
void printfStudent(student stu) {
    cout << "姓名：" << stu.name << " 年龄： " << stu.age << " 分数："
         << stu.score << endl;
}
 
int main() {
	// 声明结构体
    student stu = {"张三", 18, 100};
   // 调用自定义函数
    printfStudent(stu);
 
    return 0;
}
```

#### 结构体指针做函数参数

使用结构体指针变量做函数参数时传递的只是地址, 这样的方式减少了时间和空间上的开销, 能够有效提高程序的运行效率.

```c++
#include <iostream>
using namespace std;
 
// 定义学生结构体定义
struct student {
    string name;
    int age;
    int score;
};

// 自定义函数 打印学生信息
void printfStudent2(student* stu) {
    cout << "姓名：" << stu->name << " 年龄： " << stu->age << " 分数：" << stu->score << endl;
}
 
int main() {
    student stu = {"张三", 18, 100};
 
    // 地址传递
    printfStudent2(&stu);
    return 0;
}
```

> ⚠️ 注意: 两者的最主要的区别是结构体指针作为函数参数时, 如果函数内部修改了结构体内的数值, 则会修改成员结构体的数据, 而结构体变量则不会.

```c++
#include<iostream>
 
using namespace std;
// 全局变量声明区
 
// 学生结构体定义
struct Student
{
	string name;
	int age;
	int score;
};
// 函数声明
void printfStudent(student stu);
void printfStudent2(student* stu);
 
int main()
{
	Student stu = { "张三", 18, 100 };
  
	// 值传递
	printfStudent(stu);
	cout << "主函数中 姓名：" << stu.name << " 年龄： " << stu.age << " 分数：" << stu.score << endl;
	cout << endl;
 
	// 地址传递
	printfStudent2(&stu);
	cout << "主函数中 姓名：" << stu.name << " 年龄： " << stu.age << " 分数：" << stu.score << endl;
  
	return 0;
}
 
// 值传递
void printfStudent(student stu)
{
	stu.age = 28;
	cout << "子函数中 姓名：" << stu.name << " 年龄： " << stu.age << " 分数：" << stu.score << endl;
}
 
// 地址传递
void printfStudent2(student* stu)
{
	stu->age = 28;
	cout << "子函数中 姓名：" << stu->name << " 年龄： " << stu->age << " 分数：" << stu->score << endl;
}
```

以上代码唯一的区别就是函数传入的参数类型不同, 一个结构体变量参数一个结构体指针变量参数, 然后同样在函数中修改 age 的值并且结构体中 age 初始值都是 18, 一起看下运行结果:

```c++
开始运行...
子函数中 姓名：张三 年龄： 28 分数：100
主函数中 姓名：张三 年龄： 18 分数：100
 
子函数中 姓名：张三 年龄： 28 分数：100
主函数中 姓名：张三 年龄： 28 分数：100
 
运行结束.
```

可以看到函数参数作为结构体指针进行地址传递修改数据时, student 的 age 从 18 被修改为 28 了.

### 结构体的嵌套

定义完结构体后形成一个新的数据类型, C++中在定义结构体时可以声明其他已定义好的结构体变量, 也可以在定义结构体时定义子结构体.

#### 在定义结构体中定义子结构体

```c++
struct Student {
 
    struct StudentCar {
        string car_name;
    };
    // 结构体内的变量和方法默认都是public的
    string name;
    int age;
    int score;
    // 设置学生信息
    void setStuMsg(string name, int age, int score) {
        this->name = name;
        this->age = age;
        this->score = score;
    }
    // 展示学生信息
    void showStuMsg() {
        cout << "name=" << name << ",age=" << age << ",score=" << score << endl;
    }
};
```

定义了一个名为 Student 结构体里面包含学生的姓名、年龄和分数, 同时在 Student 里面定义了一个 StudentCar 的结构体, 里面有 car_name 属性. 然后看下这种结构体嵌套如何调用:

```c++
int main() {
    Student::StudentCar we;				 // 声明结构体Student内部的结构体StudentCar的对象
    we.car_name = "bmw";					 // 对car_name进行赋值操作 
    cout << we.car_name << endl;	 // 打印car_name的值 
    Student stu;									 // 声明外部Student的对象
    stu.setStuMsg("jack", 38, 88); // 设置Student的属性值
    stu.showStuMsg();							 // 调用自定义函数 打印结构体Student的值
    return 0;
}

// 打印输出结果：
bmw
name=jack,age=38,score=88
```

可以看到我们使用 "::" 引用到结构体内部的结构体变量进行操作. 当然输了基本数据类型的访问, 自定义的函数也是能访问的, 操作也是一样的.

#### 在定义时声明其他已经定义好的结构体变量

```c++
struct StudentCar {
    string car_name;
};
struct Student {
    StudentCar studentCar;

    // 结构体内的变量和方法默认都是public的
    string name;
    int age;
    int score;
  
    // 设置学生信息
    void setStuMsg(string name, int age, int score) {
        this->name = name;
        this->age = age;
        this->score = score;
    }
  
    // 展示学生信息
    void showStuMsg() {
        cout << "name=" << name << ",age=" << age << ",score=" << score << endl;
    }
};
```

以上这种结构体嵌套的话就比较好理解了, 把它当成基本数据类型调用使用就行了. 先调用 studentCar 对象, 再访问里面的属性, 例如:

```c++
Student stu;
stu.studentCar.car_name="benz";
```

### 结构体数组

结构体数组可以在定义结构体时声明, 可以使用结构体变量声明, 也可以直接声明结构体数组而无需定义结构体名.

#### 在定义结构体时直接声明

```c++
struct Student {
    string name; //学生姓名
    int age;     //学生年龄
    int score;   //学生分数
} student[5];
```

#### 在使用结构体变量声明

```c++
struct Student {
    string name; //学生姓名
    int age;     //学生年龄
    int score;   //学生分数
};
 
Student student[2] = {
        {"tome",  67, 88},
        {"jerry", 86, 99},
    };
```

#### 直接声明结构体数组

```c++
struct Student {
    string name; //学生姓名
    int age;     //学生年龄
    int score;   //学生分数
} student[2] = {
    {"tome",  67, 88},
    {"jerry", 86, 99},
};
```

## FAQ





## See Also

https://blog.csdn.net/csj731742019/article/details/126333378

