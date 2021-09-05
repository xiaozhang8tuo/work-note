## libc和glibc ##

glibc 和 libc 都是 Linux 下的 C 函数库。 
libc 是 Linux 下的 ANSI C 函数库的实现；glibc 是 Linux 下的 GUN C 函数库的实现。 

ANSI C 函数库是基本的 C 语言函数库，包含了 C 语言最基本的库函数。这个库可以根据头文件划分为 15 个部分，其中包括：

> 1. <ctype.h>：包含用来测试某个特征字符的函数的函数原型，以及用来转换大小写字母的函数原型；
> 2. <errno.h>：定义用来报告错误条件的宏；
> 3. <float.h>：包含系统的浮点数大小限制；
> 4. <math.h>：包含数学库函数的函数原型；
> 5. <stddef.h>：包含执行某些计算 C 所用的常见的函数定义；
> 6. <stdio.h>：包含标准输入输出库函数的函数原型，以及他们所用的信息；
> 7. <stdlib.h>：包含数字转换到文本，以及文本转换到数字的函数原型，还有内存分配、随机数字以及其他实用函数的函数原型；
> 8. <string.h>：包含字符串处理函数的函数原型；
> 9. <time.h>：包含时间和日期操作的函数原型和类型；
> 10. <stdarg.h>：包含函数原型和宏，用于处理未知数值和类型的函数的参数列表；
> 11. <signal.h>：包含函数原型和宏，用于处理程序执行期间可能出现的各种条件；
> 12. <setjmp.h>：包含可以绕过一般函数调用并返回序列的函数的原型，即非局部跳转；
> 13. <locale.h>：包含函数原型和其他信息，使程序可以针对所运行的地区进行修改。
> 14. 地区的表示方法可以使计算机系统处理不同的数据表达约定，如全世界的日期、时间、美元数和大数字；
> 15. <assert.h>：包含宏和信息，用于进行诊断，帮助程序调试。

GNU C 函数库是一种类似于第三方插件的东西。由于 Linux 是用 C 语言写的，所以 Linux 的一些操作是用 C 语言实现的，因此，GUN 组织开发了一个 C 语言的库  以便让我们更好的利用 C 语言开发基于 Linux 操作系统的程序。 不过现在的不同的 Linux 的发行版本对这两个函数库有不同的处理方法，有的可能已经集成在同一个库里了。 

**glibc**是linux下面c标准库的实现，即GNU C Library。glibc本身是GNU旗下的C标准库，**后来逐渐成为了Linux的标准c库，而Linux下原来的标准c库Linux libc逐渐不再被维护**。Linux下面的标准c库不仅有这一个，如uclibc、klibc，以及上面被提到的Linux libc，但是**glibc无疑是用得最多的**。glibc在/lib目录下的.so文件为libc.so.6。

glibc都做了些什么呢？ glibc是Linux系统中最底层的API，几乎其它任何的运行库都要依赖glibc。 glibc最主要的功能就是对系统调用的封装，你想想看，你怎么能在C代码中直接用fopen函数就能打开文件？ 打开文件最终还是要触发系统中的sys_open系统调用，而这中间的处理过程都是glibc来完成的。除了封装系统调用，glibc自身也提供了一些上层应用函数必要的功能,如string,malloc,stdlib,linuxthreads,locale,signal等等。

## eglibc ##

eglibc又是什么？ 这里的e是Embedded的意思，也就是前面说到的变种glibc。eglibc的主要特性是为了更好的支持嵌入式架构，可以支持不同的shell(包括嵌入式)，但它是二进制兼容glibc的，就是说如果你的代码之前依赖eglibc库，那么换成glibc后也不需要重新编译。ubuntu系统用的就是eglibc（而不是glibc）,不信，你执行 ldd –version 或者 /lib/i386-linux-gnu/libc.so.6
(64位系统运行/lib/x86_64-linux-gnu）看看，便会显示你系统中eglibc/glibc的版本信息。 这里提到了libc.so.6,这个文件就是eglibc/glibc编译后的生成库文件。

## glib ##

glib看起来也很相似，那它又是什么呢？glib也是个c程序库，不过比较轻量级，glib是用C写的一些utilities，即C的工具库，和libc/glibc没有关系。glib将C语言中的数据类型统一封装成自己的数据类型，提供了C语言常用的数据结构的定义以及处理函数，有趣的宏以及可移植的封装等(注：glib是可移植的，说明你可以在linux下，也可以在windows下使用它）。那它跟glibc有什么关系吗？其实并没有，除非你的程序代码会用到glib库中的数据结构或者函数，glib库在ubuntu系统中并不会默认安装(可以通过apt-get install libglib2.0-dev手动安装)，著名的GTK+和Gnome底层用的都是glib库。想更详细了解glib？ 可以参考[这里](https://developer.gnome.org/glib/)。



## libc++和libstdc++ ##

当然如果你写的是C++代码，还有两个库也要非常重视了，libc++/libstdc++,这两个库有关系吗？有。两个都是C++标准库。libc++是针对clang编译器特别重写的C++标准库，那libstdc++自然就是gcc的事儿了。libstdc++与gcc的关系就像clang与libc++。（不同编译器下的c++的库实现）

libstdc++，glibc的关系。 libstdc++与gcc是捆绑在一起的，也就是说安装gcc的时候会把libstdc++装上。 那为什么glibc和gcc没有捆绑在一起呢？相比glibc，libstdc++虽然提供了c++程序的标准库，但它并不与内核打交道。对于系统级别的事件，libstdc++首先是会与glibc交互，才能和内核通信。相比glibc来说，libstdc++就显得没那么基础了。

## 从代码到可执行文件 ##

你写的C代码.c文件通过gcc首先转化为汇编.s文件，之后汇编器as将.S文件转化为机器代码.o文件，生成的.o文件再与其它.o文件，或者之前提到的libc.so.6库文件通过ld链接器链接在一块生成可执行文件。当然，在你编译代码使用gcc的时候，gcc命令已经帮你把这些细节全部做好了。

## gcc和g++ ##

那g++是做什么的? 慢慢说来，不要以为gcc只能编译C代码，g++只能编译c++代码。 后缀为.c的，gcc把它当作是C程序，而g++当作是c++程序；后缀为.cpp的，两者都会认为是c++程序，注意，虽然c++是c的超集，但是两者对语法的要求是有区别的。在编译阶段，g++会调用gcc,对于c++代码，两者是等价的，但是因为gcc命令不能自动和C++程序使用的库联接，需要这样，gcc -lstdc++, 所以如果你的Makefile文件并没有手动加libstdc++库，一般就会提示错误，要求你安装g++编译器了。

gcc 既是项目的名字，也是一个程序（项目产物）的名字。

g++ 是这个项目的产物（程序）之一。

作为程序的 gcc 和 g++ 的区别只在于对命令行参数的处理（也就是 g++ 默认加 -xc++ 、 -lstdc++ 之类），其他回答对此已经描述了。

二者的源码基本是一样的，只相差一个文件：gcc、g++、cpp 都叫做 compiler driver 。这些都不负责编译代码，只负责调用真正的编译器（compiler proper）。gcc 这个项目中，真正负责编译 C 代码的程序叫做 cc1，负责编译 C++ 代码的程序叫做 cc1plus 。



## 参考文献 ##

https://www.zhihu.com/question/20940822/answer/1768772877
https://blog.csdn.net/haibosdu/article/details/77094833 
https://blog.csdn.net/yasi_xi/article/details/9899599