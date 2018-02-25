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

##### 复合类型

### 指针

### 预编译指令

### 标准库



