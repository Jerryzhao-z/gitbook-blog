# 面向非初学者的C++笔记

这个时代，越来越多的程序员（比如我）是从javascript/java/python起步了解整个计算机体系的，鉴于知识背景，我想可以用一篇blog对C++进行入门。

### 基础

##### 基本类型
基本类型中除了前文中提到的C语言中的类型，C++定义了用于扩展字符集的类型 wchar_t(用于存放机器基本字符集中任意字符对应的数字), char16_t, char32_t(为unicode服务).

另外需要提的是浮点型的有效数字，float由一个字来表示，而double有两个字来表示，这个字长单位即CPU的浮点长度。

至于signed和unsigned可以修饰整型，一般signed修饰符可有可无，有一个特例是char，char与signed char的含义并不完全相同，由编译器来决定char与signed char还是unsigned char保持一致。

另外需要注意的是：unsigned和signed的符号不要混用做操作。

### 面向对象

### STL

### operator重载

### 泛型

### 零碎小东西



