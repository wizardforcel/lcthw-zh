# 练习4：Valgrind 介绍

现在是介绍另一个工具的时间了，在你学习C的过程中，你会时时刻刻用到它，它就是 `Valgrind`。我现在就向你介绍 `Valgrind`，是因为从现在开始你将会在“如何使它崩溃”一节中用到它。`Valgrind`是一个运行你的程序的程序，并且随后会报告所有你犯下的可怕错误。它是一款相当棒的自由软件，我在编写C代码时一直使用它。

回忆一下在上一章中，我让你移除`printf`的一个参数，来是你的代码崩溃。它打印出了一些奇怪的结果，但我病欸有告诉你为什么他会这样打印。这个练习中我们要使用`Valgrind`来搞清楚为什么。

注

这本书前面的几章在讲解一小段代码的东西，掺杂了一些必要的工具，它们在本书的剩余章节会用到。这样做的原因是，阅读这本书的大多数人都不熟悉编译语言，也必然不熟悉自动化的辅助工具。通过先让你懂得如何使用`make`和`Valgrind`，我可以在后面使用它们更快地教你C语言，以及帮助你尽早找出所有的bug。

这一章之后我就不再介绍更多的工具了，每章的内容大部分是代码，以及少量的语法。然而，我也会提及少量工具，我们可以用它来真正了解发生了什么，以及更好地了解常见的错误和问题。

## 安装 Valgrind

你可以用OS上的包管理器来安装`Valgrind`，但是我想让你学习如何从源码安装程序。这涉及到下面几个步骤：

+ 下载源码的归档文件来获得源码
+ 解压归档文件，将文件提取到你的电脑上
+ 运行`./configure`来建立构建所需的配置
+ 运行`make`来构建源码，就像之前所做的那样
+ 运行`sudo make install`来将它安装到你的电脑

下面是执行以上步骤的脚本，我想让你复制它：

```
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

按照这份脚本，但是如果 Valgrind 有新的版本请更新它。如果它不能正常执行，也请试着深入研究原因。

## 使用 Valgrind 

使用 Valgrind 十分简单，只要执行`valgrind theprogram`，它就会运行你的程序，随后打印出你的程序运行时出现的所有错误。在这个练习中，我们会崩溃在一个错误输出上，然后会修复它。

首先，这里有一个`ex3.c`的故意出错的版本，叫做`ex4.c`。出于练习目的，将它再次输入到文件中：

```
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