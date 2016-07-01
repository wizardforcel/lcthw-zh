# 练习7：更多变量和一些算术

> 原文：[Exercise 7: More Variables, Some Math](http://c.learncodethehardway.org/book/ex7.html)

> 译者：[飞龙](https://github.com/wizardforcel)

你可以通过声明`int`，`float`，`char`和`double`类型的变量，来对它们做更多的事情，让我们来熟悉它们吧。接下来我们会在各种数学表达式中使用它们，所以我会向你介绍C的基本算术操作。

```c
int main(int argc, char *argv[])
{
    int bugs = 100;
    double bug_rate = 1.2;

    printf("You have %d bugs at the imaginary rate of %f.\n",
            bugs, bug_rate);

    long universe_of_defects = 1L * 1024L * 1024L * 1024L;
    printf("The entire universe has %ld bugs.\n",
            universe_of_defects);

    double expected_bugs = bugs * bug_rate;
    printf("You are expected to have %f bugs.\n",
            expected_bugs);

    double part_of_universe = expected_bugs / universe_of_defects;
    printf("That is only a %e portion of the universe.\n",
            part_of_universe);

    // this makes no sense, just a demo of something weird
    char nul_byte = '\0';
    int care_percentage = bugs * nul_byte;
    printf("Which means you should care %d%%.\n",
            care_percentage);

    return 0;
}
```

下面是这一小段无意义代码背后发生的事情：

　　ex7.c:1-4

　　C程序的通常开始。

　　ex7.c:5-6

　　为一些伪造的bug数据声明了一个`int`和一个`double`变量。

　　ex7.c:8-9

　　打印这两个变量，没有什么新东西。

　　ex7.c:11

　　使用了一个新的类型`long`来声明一个大的数值，它可以储存比较大的数。

　　ex7.c:12-13

　　使用`%ld`打印出这个变量，我们添加了个修饰符到`%d`上面。添加的"l"表示将它当作长整形打印。

　　ex7.c:15-17

　　只是更多的算术运算和打印。

　　ex7.c:19-21

　　编撰了一段你的bug率的描述，这里的计算非常不精确。结果非常小，所以我们要使用`%e`以科学记数法的形式打印它。

　　ex7.c:24

　　以特殊的语法`'\0'`声明了一个字符。这样创建了一个“空字节”字符，实际上是数字0。

　　ex7.c:25

　　使用这个字符乘上bug的数量，它产生了0，作为“有多少是你需要关心的”的结果。这条语句展示了你有时会碰到的丑陋做法。

　　ex7.c:26-27

　　将它打印出来，注意我使用了`%%`（两个百分号）来打印一个`%`字符。

　　ex7.c:28-30

　　`main`函数的结尾。

这一段代码只是个练习，它演示了许多算术运算。在最后，它也展示了许多你能在C中看到，但是其它语言中没有的技巧。对于C来说，一个“字符”同时也是一个整数，虽然它很小，但的确如此。这意味着你可以对它做算术运算，无论是好是坏，许多软件中也是这样做的。

在最后一部分中，你第一次见到C语言是如何直接访问机器的。我们会在后面的章节中深入。

## 你会看到什么

通常，你应该看到如下输出：

```sh
$ make ex7
cc -Wall -g    ex7.c   -o ex7
$ ./ex7
You have 100 bugs at the imaginary rate of 1.200000.
The entire universe has 1073741824 bugs.
You are expected to have 120.000000 bugs.
That is only a 1.117587e-07 portion of the universe.
Which means you should care 0%.
$
```

## 如何使它崩溃

像之前一样，向`printf`传入错误的参数来使它崩溃。对比`%c`，看看当你使用`%s`来打印`nul_byte`变量时会发生什么。做完这些之后，在`Valgrind`下运行它看看关于你的这次尝试会输出什么。

## 附加题

+ 把为`universe_of_defects`赋值的数改为不同的大小，观察编译器的警告。
+ 这些巨大的数字实际上打印成了什么？
+ 将`long`改为`unsigned long`，并试着找到对它来说太大的数字。
+ 上网搜索`unsigned`做了什么。
+ 试着自己解释（在下个练习之前）为什么`char`可以和`int`相乘。
