# 练习3：格式化输出

> 原文：[Exercise 3: Formatted Printing](http://c.learncodethehardway.org/book/ex3.html)

> 译者：[飞龙](https://github.com/wizardforcel)

不要删除Makefile，因为它可以帮你指出错误，以及当我们需要自动化处理一些事情时，可以向它添加新的东西。

许多编程语言都使用了C风格的格式化输出，所以让我们尝试一下：

```c
#include <stdio.h>

int main()
{
    int age = 10;
    int height = 72;

    printf("I am %d years old.\n", age);
    printf("I am %d inches tall.\n", height);

    return 0;
}
```

写完之后，执行通常的`make ex3`命令来构建并运行它。一定要确保你处理了所有的警告。

这个练习的代码量很小，但是信息量很大，所以让我们逐行分析一下：

+ 首先你包含了另一个头文件叫做`stdio.h`。这告诉了编译器你要使用“标准的输入/输出函数”。它们之一就是`printf`。
+ 然后你使用了一个叫`age`的变量并且将它设置为10。
+ 接着你使用了一个叫`height`的变量并且设置为72。
+ 再然后你使用`printf`函数来打印这个星球上最高的十岁的人的年龄和高度。
+ 在`printf`中你会注意到你传入了一个字符串，这就是格式字符串，和其它语言中一样。
+ 在格式字符串之后，你传入了一些变量，它们应该被`printf`“替换”进格式字符串中。

这些语句的结果就是你用`printf`处理了一些变量，并且它会构造出一个新的字符串，之后将它打印在终端上。

## 你会看到什么

当你做完上面的整个步骤，你应该看到这些东西：

```sh
$ make ex3
cc -Wall -g    ex3.c   -o ex3
$ ./ex3
I am 10 years old.
I am 72 inches tall.
$
```

不久之后我会停下来让你运行`make`，并且告诉你构建过程是什么样子的。所以请确保你正确得到了这些信息并且能正常执行。

## 外部研究

在附加题一节我可能会让你自己查找一些资料，并且弄明白它们。这对于一个自我学习的程序员来说相当重要。如果你一直在自己尝试了解问题之前去问其它人，你永远都不会学到独立解决问题。这会让你永远都不会在自己的技能上建立信心，并且总是依赖别人去完成你的工作。

打破你这一习惯的方法就是强迫你自己先试着自己回答问题，并且确认你的回答是正确的。你可以通过打破一些事情，用实验验证可能的答案，以及自己进行研究来完成它。

对于这个练习，我想让你上网搜索`printf`的所有格式化占位符和转义序列。转义序列类似`\n`或者`\r`，可以让你分别打印新的一行或者 tab 。格式化占位符类似`%s`或者`%d`，可以让你打印字符串或整数。找到所有的这些东西，以及如何修改它们，和可设置的“精度”和宽度的种类。

从现在开始，这些任务会放到附加题里面，你应该去完成它们。

## 如何使它崩溃

尝试下面的一些东西来使你的程序崩溃，在你的电脑上它们可能会崩溃，也可能不会。

+ 从第一个`printf`中去掉`age`并重新编译，你应该会得到一大串的警告。
+ 运行新的程序，它会崩溃，或者打印出奇怪的年龄。
+ 将`printf`恢复原样，并且去掉`age`的初值，将那一行改为`int age;`，之后重新构建并运行。

```sh
# edit ex3.c to break printf
$ make ex3
cc -Wall -g    ex3.c   -o ex3
ex3.c: In function 'main':
ex3.c:8: warning: too few arguments for format
ex3.c:5: warning: unused variable 'age'
$ ./ex3
I am -919092456 years old.
I am 72 inches tall.
# edit ex3.c again to fix printf, but don't init age
$ make ex3
cc -Wall -g    ex3.c   -o ex3
ex3.c: In function 'main':
ex3.c:8: warning: 'age' is used uninitialized in this function
$ ./ex3
I am 0 years old.
I am 72 inches tall.
$
```

## 附加题

+ 找到尽可能多的方法使`ex3`崩溃。
+ 执行`man 3 printf`来阅读其它可用的'%'格式化占位符。如果你在其它语言中使用过它们，应该看着非常熟悉（它们来源于`printf`）。
+ 将`ex3`添加到你的`Makefile`的`all`列表中。到目前为止，可以使用`make clean all`来构建你所有的练习。
+ 将`ex3`添加到你的`Makefile`的`clean`列表中。当你需要的时候使用`make clean`可以删除它。
