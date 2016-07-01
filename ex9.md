# 练习9：数组和字符串

> 原文：[Exercise 9: Arrays And Strings](http://c.learncodethehardway.org/book/ex9.html)

> 译者：[飞龙](https://github.com/wizardforcel)

上一个练习中，我们学习了如何创建基本的数组，以及数组如何映射为字符串。这个练习中我们会更加全面地展示数组和字符串的相似之处，并且深入了解更多内存布局的知识。

这个练习向你展示了C只是简单地将字符串储存为字符数组，并且在结尾加上`'\0'`（空字符）。你可能在上个练习中得到了暗示，因为我们手动这样做了。下面我会通过将它与数字数组比较，用另一种方法更清楚地实现它。

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    int numbers[4] = {0};
    char name[4] = {'a'};

    // first, print them out raw
    printf("numbers: %d %d %d %d\n",
            numbers[0], numbers[1],
            numbers[2], numbers[3]);

    printf("name each: %c %c %c %c\n",
            name[0], name[1],
            name[2], name[3]);

    printf("name: %s\n", name);

    // setup the numbers
    numbers[0] = 1;
    numbers[1] = 2;
    numbers[2] = 3;
    numbers[3] = 4;

    // setup the name
    name[0] = 'Z';
    name[1] = 'e';
    name[2] = 'd';
    name[3] = '\0';

    // then print them out initialized
    printf("numbers: %d %d %d %d\n",
            numbers[0], numbers[1],
            numbers[2], numbers[3]);

    printf("name each: %c %c %c %c\n",
            name[0], name[1],
            name[2], name[3]);

    // print the name like a string
    printf("name: %s\n", name);

    // another way to use name
    char *another = "Zed";

    printf("another: %s\n", another);

    printf("another each: %c %c %c %c\n",
            another[0], another[1],
            another[2], another[3]);

    return 0;
}
```

在这段代码中，我们创建了一些数组，并对数组元素赋值。在`numbers`中我们设置了一些数字，然而在`names`中我们实际上手动构造了一个字符串。

## 你会看到什么

当你运行这段代码的时候，你应该首先看到所打印的数组的内容初始化为0值，之后打印初始化后的内容：

```sh
$ make ex9
cc -Wall -g    ex9.c   -o ex9
$ ./ex9
numbers: 0 0 0 0
name each: a   
name: a
numbers: 1 2 3 4
name each: Z e d
name: Zed
another: Zed
another each: Z e d
$
```

你会注意到这个程序中有一些很有趣的事情：

+ 我并没有提供全部的4个参数来初始化它。这是C的一个简写，如果你只提供了一个元素，剩下的都会为0。
+ `numbers`的每个元素被打印时，它们都输出0。
+ `names`的每个元素被打印时，只显示了第一个元素`'a'`，因为`'\0'`是特殊字符而不会显示。
+ 然后我们首次打印`names`，打印出了`"a"`，因为在初始化表达式中，`'a'`字符之后的空间都用`'\0'`填充，是以`'\0'`结尾的有效字符串。
+ 我们接着通过手动为每个元素赋值来建立数组，并且再次把它打印出来。看看他们发生了什么改变。现在`numbers`已经设置好了，看看`names`字符串如何正确打印出我的名字。
+ 创建一个字符串也有两种语法：第六行的`char name[4] = {'a'}`，或者第44行的`char *another = "name"`。前者不怎么常用，你应该将后者用于字符串字面值。

注意我使用了相同的语法和代码风格来和整数数组和字符数组交互，但是`printf`认为`name`是个字符串。再次强调，这是因为对C语言来说，字符数组和字符串没有什么不同。

最后，当你使用字符串字面值时你应该用`char *another = "Literal"`语法，它会产生相同的东西，但是更加符合语言习惯，也更省事。

## 如何使它崩溃

C中所有bug的大多数来源都是忘了预留出足够的空间，或者忘了在字符串末尾加上一个`'\0'`。事实上，这些bug是非常普遍并且难以改正的，大部分优秀的C代码都不会使用C风格字符串。下一个练习中我们会学到如何彻底避免C风格字符串。

使这个程序崩溃的的关键就是拿掉字符串结尾的`'\0'`。下面是实现它的一些途径：

+ 删掉`name`的初始化表达式。
+ 无意中设置`name[3] = 'A'`，于是它就没有终止字符了。
+ 将初始化表达式设置为`{'a','a','a','a'}`，于是就有过多的`'a'`字符，没有办法给`'\0'`留出位置。

试着想出一些其它的办法让它崩溃，并且在`Valgrind`下像往常一样运行这个程序，你可以看到具体发生了什么，以及错误叫什么名字。有时`Valgrind`并不能发现你犯的错误，则需要移动声明这些变量的地方看看是否能找出错误。这是C的黑魔法的一部分，有时变量的位置会改变bug。

## 附加题

+ 将一些字符赋给`numbers`的元素，之后用`printf`一次打印一个字符，你会得到什么编译器警告？
+ 对`names`执行上述的相反操作，把`names`当成`int`数组，并一次打印一个`int`，`Valgrind`会提示什么？
+ 有多少种其它的方式可以用来打印它？
+ 如果一个字符数组占四个字节，一个整数也占4个字节，你可以像整数一样使用整个`name`吗？你如何用黑魔法实现它？
+ 拿出一张纸，将每个数组画成一排方框，之后在纸上画出代码中的操作，看看是否正确。
+ 将`name`转换成`another`的形式，看看代码是否能正常工作。
