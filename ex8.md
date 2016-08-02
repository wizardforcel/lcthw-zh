# 练习8：大小和数组

> 原文：[Exercise 8: Sizes And Arrays](http://c.learncodethehardway.org/book/ex8.html)

> 译者：[飞龙](https://github.com/wizardforcel)

在上一个练习中你做了一些算术运算，并且使用了`'\0'`（空）字符。这对于其它语言来说非常奇怪，因为它们把“字符串”和“字节数组”看做不同的东西。但是C中的字符串就是字节数组，并且只有不同的打印函数才知道它们的不同。

在我真正解释其重要性之前，我先要介绍一些概念：`sizeof`和数组。下面是我们将要讨论的一段代码：

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    int areas[] = {10, 12, 13, 14, 20};
    char name[] = "Zed";
    char full_name[] = {
        'Z', 'e', 'd',
         ' ', 'A', '.', ' ',
         'S', 'h', 'a', 'w', '\0'
    };

    // WARNING: On some systems you may have to change the
    // %ld in this code to a %u since it will use unsigned ints
    printf("The size of an int: %ld\n", sizeof(int));
    printf("The size of areas (int[]): %ld\n",
            sizeof(areas));
    printf("The number of ints in areas: %ld\n",
            sizeof(areas) / sizeof(int));
    printf("The first area is %d, the 2nd %d.\n",
            areas[0], areas[1]);

    printf("The size of a char: %ld\n", sizeof(char));
    printf("The size of name (char[]): %ld\n",
            sizeof(name));
    printf("The number of chars: %ld\n",
            sizeof(name) / sizeof(char));

    printf("The size of full_name (char[]): %ld\n",
            sizeof(full_name));
    printf("The number of chars: %ld\n",
            sizeof(full_name) / sizeof(char));

    printf("name=\"%s\" and full_name=\"%s\"\n",
            name, full_name);

    return 0;
}
```

这段代码中我们创建了一些不同数据类型的数组。由于数组是C语言工作机制的核心，有大量的方法可以用来创建数组。我们暂且使用`type name[] = {initializer};`语法，之后我们会深入研究。这个语法的意思是，“我想要那个类型的数组并且初始化为{..}”。C语言看到它时，会做这些事情：

+ 查看它的类型，以第一个数组为例，它是`int`。
+ 查看`[]`，看到了没有提供长度。
+ 查看初始化表达式`{10, 12, 13, 14, 20}`，并且了解你想在数组中存放这5个整数。
+ 在电脑中开辟出一块空间，可以依次存放这5个整数。
+ 将数组命名为`areas`，也就是你想要的名字，并且在当前位置给元素赋值。

在`areas`的例子中，我们创建了一个含有5个整数的数组来存放那些数字。当它看到`char name[] = "Zed";`时，它会执行相同的步骤。我们先假设它创建了一个含有3个字符的数组，并且把字符赋值给`name`。我们创建的最后一个数组是`full_name`，但是我们用了一个比较麻烦的语法，每次用一个字符将其拼写出来。对C来说，`name`和`full_name`的方法都可以创建字符数组。

在文件的剩余部分，我们使用了`sizeof`关键字来问C语言这些东西占多少个字节。C语言无非是内存块的大小和地址以及在上面执行的操作。它向你提供了`sizeof`便于你理解它们，所以你在使用一个东西之前可以先询问它占多少空间。

这是比较麻烦的地方，所以我们先运行它，之后再解释。

## 你会看到什么

```sh
$ make ex8
cc -Wall -g    ex8.c   -o ex8
$ ./ex8
The size of an int: 4
The size of areas (int[]): 20
The number of ints in areas: 5
The first area is 10, the 2nd 12.
The size of a char: 1
The size of name (char[]): 4
The number of chars: 4
The size of full_name (char[]): 12
The number of chars: 12
name="Zed" and full_name="Zed A. Shaw"
$
```

现在你可以看到这些不同`printf`调用的输出，并且瞥见C语言是如何工作的。你的输出实际上可能会跟我的完全不同，因为你电脑上的整数大小可能会不一样。下面我会过一遍我的输出：

> 译者注：16位机器上的`int`是16位的，不过现在16位机很少见了吧。

　　5

　　我的电脑认为`int`的大小是4个字节。你的电脑上根据位数不同可能会使用不同的大小。

　　6

　　`areas`中含有5个整数，所以我的电脑自然就需要20个字节来储存它。

　　7

　　如果我们把`areas`的大小与`int`的大小相除，我们就会得到元素数量为5。这也符合我们在初始化语句中所写的东西。

　　8

　　接着我们访问了数组，读出`areas[0]`和`areas[1]`，这也意味着C语言的数组下标是0开头的，像Python和Ruby一样。

　　9~11

　　我们对`name`数组执行同样的操作，但是注意到数组的大小有些奇怪，它占4个字节，但是我们用了三个字符来打出"Zed"。那么第四个字符是哪儿来的呢？

　　12~13

　　我们对`full_name`数组执行了相同的操作，但它是正常的。

　　13

　　最后我们打印出`name`和`full_name`，根据`printf`证明它们实际上就是“字符串”。

确保你理解了上面这些东西，并且知道这些输出对应哪些创建的变量。后面我们会在它的基础上探索更多关于数组和存储空间的事情。

## 如何使它崩溃

使这个程序崩溃非常容易，只需要尝试下面这些事情：

+ 将`full_name`最后的`'\0'`去掉，并重新运行它，在`valgrind`下再运行一遍。现在将`full_name`的定义从`main`函数中移到它的上面，尝试在`Valgrind`下运行它来看看是否能得到一些新的错误。有些情况下，你会足够幸运，不会得到任何错误。
+ 将`areas[0]`改为`areas[10]`并打印，来看看`Valgrind`会输出什么。
+ 尝试上述操作的不同变式，也对`name`和`full_name`执行一遍。

## 附加题

+ 尝试使用`areas[0] = 100;`以及相似的操作对`areas`的元素赋值。
+ 尝试对`name`和`full_name`的元素赋值。
+ 尝试将`areas`的一个元素赋值为`name`中的字符。
+ 上网搜索在不同的CPU上整数所占的不同大小。
