# 面向非初学者的C语言笔记

这个时代，越来越多的程序员（比如我）是从javascript/java/python起步了解整个计算机体系的，鉴于知识背景，我想可以用一篇blog对C语言进行入门。

### 环境

C语言是典型的编译型语言，在linux下我们可以使用gcc完成代码的预处理-编译-汇编-链接，最终把代码打包成可应用程序/静态链接库/动态链接库。  
检测Linux下是否安装gcc的命令如下：

```bash
gcc -v
```

### 基础

##### hello world

```c
# include <stdio.h> //预编译指令
int main() // 变量名/函数名只能以大小写字母或下划线开头，由数字/字母/下划线构成
{
    printf("%s\n","hello world"); // 打印
    return 0; // 每行语句以分号为结束符
}

/*
块状注释
*/

// 行注释
```

将改代码存入hello\_world.c之后使用以下命令编译和运行改代码。

```bash
gcc hello_world.c -o hello #把hello_world.c文件编译后输出为hello程序
./hello #打印hello world
```

##### 保留字

| auto | else | long | switch |
| :--- | :--- | :--- | :--- |
| break | enum | register | typedef |
| case | extern | return | union |
| char | float | short | unsigned |
| const | for | signed | void |
| continue | goto | sizeof | volatile |
| default | if | static | while |
| do | int | struct | \_Packed |
| double |  |  |  |
|  |  |  |  |

##### 基本类型

| 类型 | 存储大小 | 值范围 |
| :--- | :--- | :--- |
| char | 1 字节 | -128 到 127 或 0 到 255 |
| unsigned char | 1 字节 | 0 到 255 |
| signed char | 1 字节 | -128 到 127 |
| int | 2 或 4 字节 | -32,768 到 32,767 或 -2,147,483,648 到 2,147,483,647 |
| unsigned int | 2 或 4 字节 | 0 到 65,535 或 0 到 4,294,967,295 |
| short | 2 字节 | -32,768 到 32,767 |
| unsigned short | 2 字节 | 0 到 65,535 |
| long | 4 字节 | -2,147,483,648 到 2,147,483,647 |
| unsigned long | 4 字节 | 0 到 4,294,967,295 |

![](http://www.runoob.com/wp-content/uploads/2014/09/32-64.jpg)

实际在工程中，我们常常会用到类似uint32，uint64命名的类。这些强调位数的类型，是方便统一代码在32位，64位cpu下运行的逻辑，一般是在某个头文件中定义。  
另外void代表没有可用的值，即空。有以下几种用法

```c
void exit(int errno); // 表示没有返回值
int rand(void); //表示没有参数
void* malloc(size_t size) //表示一个可以转为任意类型的指针
```

##### 枚举类型

```c
enum week 
{
    Mon,
    Tues,
    Wed,
    Thurs,
    Fri,
    Sat,
    Sun
}; //枚举类型默认回给第一个枚举值赋为1，之后逐个加一。我们也可以逐个给枚举值赋值为整数
```

##### 复合类型

```
struct book
{

} // 结构体大小不仅仅和内部成员变量有关，而且和对齐方式有关，可以参考http://www.cnblogs.com/0201zcr/p/4789332.html

union name
{
} // 共同体的大小应该能容纳最大的成员变量，所以和最大成员变量保持一致。
```

### 指针

```c
type* var_name //定义一个变量指针（地址）

type a = *var_name //获取指针指向的值
type* b = &a //重新获取指针

// 在没有确切地址时，应该为其赋值为NULL

// 一个返回值为指针的函数
void* func(void);

//定义一个函数指针
void (*func)(int, int)

//回调函数：通过函数指针调用的函数

#include <stdlib.h>  
#include <stdio.h>

// 回调函数
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{
    for (size_t i=0; i<arraySize; i++)
        array[i] = getNextValue();
}

// 获取随机值
int getNextRandomValue(void)
{
    return rand();
}

int main(void)
{
    int myarray[10];
    populate_array(myarray, 10, getNextRandomValue);
    for(int i = 0; i < 10; i++) {
        printf("%d ", myarray[i]);
    }
    printf("\n");
    return 0;
}
```

### 预编译指令

预编译指令比较有意思的是\#和\#\#的使用

```c
#include <stdio.h>

#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n") //变量名级组合

int main(void)
{
   message_for(Carole, Debra);
   return 0;
}
//输出结果是Carole and Debra: We love you!



#define tokenpaster(n) printf ("token" #n " = %d", token##n) // token级组合

int main(void)
{
   int token34 = 40;

   tokenpaster(34);
   return 0;
}
```

### 标准库

C语言的标准库比较简单，仅仅提供了一些基本类型，IO和数学计算的支持。

