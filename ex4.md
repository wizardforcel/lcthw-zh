# 练习4：Valgrind 介绍

> 原文：[Exercise 4: Introducing Valgrind](http://c.learncodethehardway.org/book/ex4.html)

> 译者：[飞龙](https://github.com/wizardforcel)

现在是介绍另一个工具的时间了，在你学习C的过程中，你会时时刻刻用到它，它就是 `Valgrind`。我现在就向你介绍 `Valgrind`，是因为从现在开始你将会在“如何使它崩溃”一节中用到它。`Valgrind`是一个运行你的程序的程序，并且随后会报告所有你犯下的可怕错误。它是一款相当棒的自由软件，我在编写C代码时一直使用它。

回忆一下在上一章中，我让你移除`printf`的一个参数，来使你的代码崩溃。它打印出了一些奇怪的结果，但我并没有告诉你为什么它会这样打印。这个练习中我们要使用`Valgrind`来搞清楚为什么。

> 注

> 这本书的前几章讲解了一小段代码，同时掺杂了一些必要的工具，它们在本书的剩余章节会用到。这样做的原因是，阅读这本书的大多数人都不熟悉编译语言，也必然不熟悉自动化的辅助工具。通过先让你懂得如何使用`make`和`Valgrind`，我可以在后面使用它们更快地教你C语言，以及帮助你尽早找出所有的bug。

> 这一章之后我就不再介绍更多的工具了，每章的内容大部分是代码，以及少量的语法。然而，我也会提及少量工具，我们可以用它来真正了解发生了什么，以及更好地了解常见的错误和问题。

## 安装 Valgrind

你可以用OS上的包管理器来安装`Valgrind`，但是我想让你学习如何从源码安装程序。这涉及到下面几个步骤：

+ 下载源码的归档文件来获得源码
+ 解压归档文件，将文件提取到你的电脑上
+ 运行`./configure`来建立构建所需的配置
+ 运行`make`来构建源码，就像之前所做的那样
+ 运行`sudo make install`来将它安装到你的电脑

下面是执行以上步骤的脚本，我想让你复制它：

```sh
# 1) Download it (use wget if you don't have curl)
curl -O http://valgrind.org/downloads/valgrind-3.6.1.tar.bz2

# use md5sum to make sure it matches the one on the site
md5sum valgrind-3.6.1.tar.bz2

# 2) Unpack it.
tar -xjvf valgrind-3.6.1.tar.bz2

# cd into the newly created directory
cd valgrind-3.6.1

# 3) configure it
./configure

# 4) make it
make

# 5) install it (need root)
sudo make install
```

按照这份脚本，但是如果 `Valgrind` 有新的版本请更新它。如果它不能正常执行，也请试着深入研究原因。

## 使用 Valgrind

使用 `Valgrind` 十分简单，只要执行`valgrind theprogram`，它就会运行你的程序，随后打印出你的程序运行时出现的所有错误。在这个练习中，我们会崩溃在一个错误输出上，然后会修复它。

首先，这里有一个`ex3.c`的故意出错的版本，叫做`ex4.c`。出于练习目的，将它再次输入到文件中：

```c
#include <stdio.h>

/* Warning: This program is wrong on purpose. */

int main()
{
    int age = 10;
    int height;

    printf("I am %d years old.\n");
    printf("I am %d inches tall.\n", height);

    return 0;
}
```

你会发现，除了两个经典的错误外，其余部分都相同：

+ 没有初始化`height`变量
+ 没有将`age`变量传入第一个`printf`函数

## 你会看到什么

现在我们像通常一样构建它，但是不要直接运行，而是使用`Valgrind`来运行它（见源码："使用Valgrind构建并运行 ex4.c"）：

```sh
$ make ex4
cc -Wall -g    ex4.c   -o ex4
ex4.c: In function 'main':
ex4.c:10: warning: too few arguments for format
ex4.c:7: warning: unused variable 'age'
ex4.c:11: warning: 'height' is used uninitialized in this function
$ valgrind ./ex4
==3082== Memcheck, a memory error detector
==3082== Copyright (C) 2002-2010, and GNU GPL'd, by Julian Seward et al.
==3082== Using Valgrind-3.6.0.SVN-Debian and LibVEX; rerun with -h for copyright info
==3082== Command: ./ex4
==3082==
I am -16775432 years old.
==3082== Use of uninitialised value of size 8
==3082==    at 0x4E730EB: _itoa_word (_itoa.c:195)
==3082==    by 0x4E743D8: vfprintf (vfprintf.c:1613)
==3082==    by 0x4E7E6F9: printf (printf.c:35)
==3082==    by 0x40052B: main (ex4.c:11)
==3082==
==3082== Conditional jump or move depends on uninitialised value(s)
==3082==    at 0x4E730F5: _itoa_word (_itoa.c:195)
==3082==    by 0x4E743D8: vfprintf (vfprintf.c:1613)
==3082==    by 0x4E7E6F9: printf (printf.c:35)
==3082==    by 0x40052B: main (ex4.c:11)
==3082==
==3082== Conditional jump or move depends on uninitialised value(s)
==3082==    at 0x4E7633B: vfprintf (vfprintf.c:1613)
==3082==    by 0x4E7E6F9: printf (printf.c:35)
==3082==    by 0x40052B: main (ex4.c:11)
==3082==
==3082== Conditional jump or move depends on uninitialised value(s)
==3082==    at 0x4E744C6: vfprintf (vfprintf.c:1613)
==3082==    by 0x4E7E6F9: printf (printf.c:35)
==3082==    by 0x40052B: main (ex4.c:11)
==3082==
I am 0 inches tall.
==3082==
==3082== HEAP SUMMARY:
==3082==     in use at exit: 0 bytes in 0 blocks
==3082==   total heap usage: 0 allocs, 0 frees, 0 bytes allocated
==3082==
==3082== All heap blocks were freed -- no leaks are possible
==3082==
==3082== For counts of detected and suppressed errors, rerun with: -v
==3082== Use --track-origins=yes to see where uninitialised values come from
==3082== ERROR SUMMARY: 4 errors from 4 contexts (suppressed: 4 from 4)
$
```

> 注

> 如果你运行了`Valgrind`，它显示一些类似于`by 0x4052112: (below main) (libc-start.c:226)`的东西，而不是`main.c`中的行号，你需要使用`valgrind --track-origins=yes ./ex4`命令来运行你的`Valgrind`。由于某些原因，`valgrind`的Debian和Ubuntu上的版本会这样，但是其它的不会。

上面那段输出非常长，因为`Valgrind`在明确地告诉你程序中的每个错误都在哪儿。让我们从开头逐行分析一下（行号在左边，你可以参照）：

1

你执行了通常的`make ex4`来构建它。确保你看到的`cc`命令和它一样，并且带有`-g`选项，否则`Valgrind`的输出不会带上行号。

2~6

要注意编译器也会向你报告源码的错误，它警告你“向格式化函数传入了过少的变量”，因为你忘记包含`age`变量。

7

然后使用`valgrind ./ex4`来运行程序。

8

之后`Valgrind`变得十分奇怪，并向你报错：

　　14~18

　　在`main (ex4.c:11)`（意思是文件`ex4.c`的`main`函数的第11行）的那行中，有“大小为8的未初始化的值”。你通过查看错误找到了它，并且在它下面看到了“栈踪迹”。最开始看到的那行`(ex4.c:11)`在最下面，如果你不明白哪里出错了，你可以向上看，比如`printf.c:35`。通常最下面的一行最重要（这个例子中是第18行）。

　　20~24

　　下一个错误位于 `main` 函数中的 `ex4.c:11`。`Valgrind`不喜欢这一行，它说的是一些 if 语句或者 while 循环基于一个未初始化的值，在这个例子中是`height`。

　　25~35

　　剩下的错误都大同小异，因为这个值还在继续使用。

37~46

最后程序退出了，`Valgrind`显示出一份摘要，告诉你程序有多烂。

这段信息读起来会相当多，下面是你的处理方法：

+ 无论什么时候你运行C程序并且使它工作，都应该使用`Valgrind`重新运行它来检查。
+ 对于得到的每个错误，找到“源码:行数”提示的位置，然后修复它。你可以上网搜索错误信息，来弄清楚它的意思。
+ 一旦你的程序在`Valgrind`下不出现任何错误信息，应该就好了。你可能学会了如何编写代码的一些技巧。

在这个练习中我并不期待你马上完全掌握`Valgrind`，但是你应该安装并且学会如何快速使用它，以便我们将它用于后面的练习。

## 附加题

+ 按照上面的指导，使用`Valgrind`和编译器修复这个程序。
+ 在互联网上查询`Valgrind`相关的资料。
+ 下载另一个程序并手动构建它。尝试一些你已经使用，但从来没有手动构建的程序。
+ 看看`Valgrind`的源码是如何在目录下组织的，并且阅读它的Makefile文件。不要担心，这对我来说没有任何意义。
