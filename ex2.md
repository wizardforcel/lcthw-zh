# 练习2：用Make来代替Python

> 原文：[Exercise 2: Make Is Your Python Now](http://c.learncodethehardway.org/book/ex2.html)

> 译者：[飞龙](https://github.com/wizardforcel)

在Python中，你仅仅需要输入`python`，就可以运行你想要运行的代码。Python的解释器会运行它们，并且在运行中导入它所需的库和其它东西。C是完全不同的东西，你需要事先编译你的源文件，并且手动将它们整合为一个可以自己运行的二进制文件。手动来做这些事情很痛苦，在上一个练习中只需要运行`make`就能完成。

这个练习是GNU make 的速成课，由于你在学C语言，所以你就必须掌握它。Make 将贯穿剩下的课程，等效于Python（命令）。它会构建源码，执行测试，设置一些选项以及为你做所有Python通常会做的事情。

有所不同的是，我会向你展示一些更智能化的Makefile魔法，你不需要指出你的C程序的每一个愚蠢的细节来构建它。我不会在练习中那样做，但是你需要先用一段时间的“低级 make”，我才能向你演示“大师级的make”。

## 使用 Make

使用make的第一阶段就是用它已知的方式来构建程序。Make预置了一些知识，来从其它文件构建多种文件。上一个练习中，你已经使用像下面的命令来这样做了：

```
$ make ex1
# or this one too
$ CFLAGS="-Wall" make ex1
```

第一个命令中你告诉make，“我想创建名为ex1的文件”。于是Make执行下面的动作：

+ 文件`ex1`存在吗？
+ 没有。好的，有没有其他文件以`ex1`开头？
+ 有，叫做`ex1.c`。我知道如何构建`.c`文件吗？
+ 是的，我会运行命令`cc ex1.c -o ex1`来构建它。
+ 我将使用`cc`从`ex1.c`文件来为你构建`ex1`。

