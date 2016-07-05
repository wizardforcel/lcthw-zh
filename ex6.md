# 练习6：变量类型

> 原文：[Exercise 6: Types Of Variables](http://c.learncodethehardway.org/book/ex6.html)

> 译者：[飞龙](https://github.com/wizardforcel)

你应该掌握了一个简单的C程序的结构，所以让我们执行下一步简单的操作，声明不同类型的变量。

```c
include <stdio.h>

int main(int argc, char *argv[])
{
    int distance = 100;
    float power = 2.345f;
    double super_power = 56789.4532;
    char initial = 'A';
    char first_name[] = "Zed";
    char last_name[] = "Shaw";

    printf("You are %d miles away.\n", distance);
    printf("You have %f levels of power.\n", power);
    printf("You have %f awesome super powers.\n", super_power);
    printf("I have an initial %c.\n", initial);
    printf("I have a first name %s.\n", first_name);
    printf("I have a last name %s.\n", last_name);
    printf("My whole name is %s %c. %s.\n",
            first_name, initial, last_name);

    return 0;
}
```

在这个程序中我们声明了不同类型的变量，并且使用了不同的`printf`格式化字符串来打印它们。

## 你会看到什么

你的输出应该和我的类似，你可以看到C的格式化字符串相似于Python或其它语言，很长一段时间中都是这样。

```sh
$ make ex6
cc -Wall -g    ex6.c   -o ex6
$ ./ex6
You are 100 miles away.
You have 2.345000 levels of power.
You have 56789.453200 awesome super powers.
I have an initial A.
I have a first name Zed.
I have a last name Shaw.
My whole name is Zed A. Shaw.
$
```

你可以看到我们拥有一系列的“类型”，它们告诉编译器变量应该表示成什么，之后格式化字符串会匹配不同的类型。下面解释了它们如何匹配：

整数

　　使用`int`声明，使用`%d`来打印。

浮点

　　使用`float`或`double`声明，使用`%f`来打印。

字符

　　使用`char`来声明，以周围带有`'`（单引号）的单个字符来表示，使用`%c`来打印。

字符串（字符数组）

　　使用`char name[]`来声明，以周围带有`"`的一些字符来表示，使用`%s`来打印。

你会注意到C语言中区分单引号的`char`和双引号的`char[]`或字符串。

> 注

> 当我提及C语言类型时，我通常会使用`char[]`来代替整个的`char SOMENAME[]`。这不是有效的C语言代码，只是一个用于讨论类型的一个简化表达方式。

## 如何使它崩溃

你可以通过向`printf`传递错误的参数来轻易使这个程序崩溃。例如，如果你找到打印我的名字的那行，把`initial`放到`first_name`前面，你就制造了一个bug。执行上述修改编译器就会向你报错，之后运行的时候你可能会得到一个“段错误”，就像这样：

```sh
$ make ex6
cc -Wall -g    ex6.c   -o ex6
ex6.c: In function 'main':
ex6.c:19: warning: format '%s' expects type 'char *', but argument 2 has type 'int'
ex6.c:19: warning: format '%c' expects type 'int', but argument 3 has type 'char *'
$ ./ex6
You are 100 miles away.
You have 2.345000 levels of power.
You have 56789.453125 awesome super powers.
I have an initial A.
I have a first name Zed.
I have a last name Shaw.
Segmentation fault
$
```

在`Valgrind`下运行修改后的程序，来观察它会告诉你什么关于错误“Invalid read of size 1”的事情。

## 附加题

+ 寻找其他通过修改`printf`使这段C代码崩溃的方法。
+ 搜索“`printf`格式化”，试着使用一些高级的占位符。
+ 研究可以用几种方法打印数字。尝试以八进制或十六进制打印，或者其它你找到的方法。
+ 试着打印空字符串，即`""`。
