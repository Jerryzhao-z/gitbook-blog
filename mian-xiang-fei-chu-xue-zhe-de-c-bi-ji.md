# 面向非初学者的C++笔记

这个时代，越来越多的程序员（比如我）是从javascript/java/python起步了解整个计算机体系的，鉴于知识背景，我想可以用一篇blog对C++进行入门。

### 基础

##### 基本类型
基本类型中除了前文中提到的C语言中的类型，C++定义了用于扩展字符集的类型 wchar_t(用于存放机器基本字符集中任意字符对应的数字), char16_t, char32_t(为unicode服务).

另外需要提的是浮点型的有效数字，float由一个字来表示，而double有两个字来表示，这个字长单位即CPU的浮点长度。

至于signed和unsigned可以修饰整型，一般signed修饰符可有可无，有一个特例是char，char与signed char的含义并不完全相同，由编译器来决定char与signed char还是unsigned char保持一致。

另外需要注意的是：unsigned和signed的符号不要混用做操作。

##### 参数传递

C++中存在三种参数传递方式：
-    值传递：形参把实参的数据赋值一份到栈上进行操作，缺点是这些操作对实参毫无影响，而且无法传递对象
-    指针传递：即把内存地址赋值一份到栈上操作，因为获取的是地址数据，可以直接操作实参
-    引用传递：引用传递的精髓在于形参是实参的一个别名，也就是说对形参的任意操作都会视为对实参的操作
实际工作中，大家普遍以引用传递为主。

##### 类型转换
隐式转换
```c++
// 比int类型小的整型首先提升为较大的整型
short b = 12;
int a = 1 + b;

// 条件中非布尔值转为布尔值
while (1) {}

// 初始化中，初始值转换为变量的类型；赋值语句中，右侧运算对象转为左侧
double a = 1;
Person a = Student() // if Student herite Person class？存疑

// 运算时需要转换为同一种类型
int a = 3.12+ 3

// 函数调用时的类型转换
double f(int a)
{
    return a;
}

```

显式转换

```c++
// cast-name<type>(expression)

double a = static_cast<double>(j)

const char *pc;
char *p = const_cast<char*>(pc); // 把常量对象转换成非常量

//reinterpret_cast 是一种比较另类的转换 不同于 static_cast ，但类似 const_cast ， reinterpret_cast 表达式不编译成任何 CPU 指令。它纯粹地是指示编译器以如同它有 new_type 类型一般，对待 expression 的位序列（对象表示）的编译器指令。

int *ip;
char *pc = reinterpret_cast<char *> ip;

string str(pc) //报错，pc实际上是int*，只是被视为char *。

// dynamic_cast<type*>(e) e必须是一个有效的指针，即指向对象或者对象相邻位置或空指针
// dynamic_cast<type&>(e) e必须是一个左值，所谓左值，就是一个可以获取地址的变量 int b = 2中b为左值，2为右值
// dynamic_cast<type&&>(e) e不能是左值
// dynamic_cast可以转换的关系中，type和e的关系有么为基类-派生类，派生类-基类关系，要么就是同一类型




```
### 面向对象

### STL

### operator重载

### 泛型

### 零碎小东西



