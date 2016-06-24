# 练习1：启用编译器

> 原文：[Exercise 1: Dust Off That Compiler](http://c.learncodethehardway.org/book/ex1.html)

> 译者：[飞龙](https://github.com/wizardforcel)

这是你用C写的第一个简单的程序：

```c
int main(int argc, char *argv[])
{
    puts("Hello world.");

    return 0;
}
```

把它写进 `ex1.c` 并输入：

```sh
$ make ex1
cc     ex1.c   -o ex1
```

你的编译器可能会使用一个有些不同的命令，但是最后应该会产生一个名为`ex1`的文件，并且你可以运行它。

## 你会看到什么

现在你可以运行程序并看到输出。

```c
$ ./ex1
Hello world.
```

如果没有，则需要返回去修复它。

## 如何使它崩溃

在这本书中我会添加一个小节，关于如何使程序崩溃。我会让你对程序做一些奇怪的事情，以奇怪的方式运行，或者修改代码，以便让你看到崩溃和编译器错误。

对于这个程序，打开所有编译警告重新构建它：

```sh
$ rm ex1
$ CFLAGS="-Wall" make ex1
cc -Wall    ex1.c   -o ex1
ex1.c: In function 'main':
ex1.c:3: warning: implicit declaration of function 'puts'
$ ./ex1
Hello world.
$
```

现在你会得到一个警告，说`puts`函数是隐式声明的。C语言的编译器很智能，它能够理解你想要什么。但是如果可以的话，你应该去除所有编译器警告。把下面一行添加到`ex1.c`文件的最上面，之后重新编译来去除它：

```c
#include <stdio.h>
```

现在像刚才一样重新执行make命令，你会看到所有警告都消失了。

## 附加题

+ 在你的文本编辑器中打开`ex1`文件，随机修改或删除一部分，之后运行它看看发生了什么。
+ 再多打印5行文本或者其它比`"Hello world."`更复杂的东西。
+ 执行`man 3 puts`来阅读这个函数和其它函数的文档。
