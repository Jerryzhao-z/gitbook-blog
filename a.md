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
int main()
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

##### 

### 指针

### 预编译指令

### 标准库


