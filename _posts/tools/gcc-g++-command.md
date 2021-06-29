---
title: gcc/g++ 命令
author: zucchini
tags:
  - gcc
  - g++
categories:
  - 工具环境
  - GCC
abbrlink: a4921194
date: 2015-12-23 02:26:00
---

gcc -E sourcefile.c :  -E，只执行到预编译，直接输出预编译结果

gcc -S sourcefile.c :  -S，只执行到源代码到汇编代码的转换，输出汇编代码

gcc -c sourcefile.c :  -c，只执行到编译，输出目标文件

gcc (-E/-S/-c) sourcefile.c -o output-file :  -o，指定输出文件名，可以使用以上三种标签中的一种。

-o 参数可以被省略，这种情况下编译器按以下默认名方式输出: <br />
-E 预编译结果将被输出到标准输出端口<br />
-S 生成名为 sourcefile.s 的汇编文件<br />
-c 生成名为 sourcefile.o 的目标文件<br />

**无标签的时候，生成名为 a.out 的可执行文件**

gcc -g sourcefile.c
:  -g 生成供调用的可执行文件，可以在 gdb 中运行。由于文件中包含了调试信息，因此运行效率很低，且文件也大了不少。

这里可以用 strip 把文件中的 debug 信息删除。 `strip a.out`

gcc -s sourcefile.c
:  -s 直接生成与运用 strip 同样的效果的可执行文件


gcc -O sourcefile.c
:  -O 编译器对代码进行自动化编译，输出效率更高的可执行文件

    <p class="verse">
    -O2 可以跟上数字表示优化等级 gcc -O2 sourcefile.c 数字越大越加优化。但是也会有出 bug 的风险<br />
    </p>


gcc -Wall sourcefile.c
:  -W 在编译中开启一些额外的警告信息。-Wall，将所有的警告信息全开。


gcc sourcefile.c -L/path/to/lib -lxxx -l/path/to/include
:  <p class="verse">
    - -l 指定所使用到的函数库，本例中是尝试链接名为 libxxx.a 的函数库<br />
    - -L 指定函数库所在的文件，本例中链接器会尝试搜索/path/to/lib 文件夹<br />
    - -I 指定文件所在的文件夹，本例中预编译器会尝试搜索/path/to/include 文件夹<br />
    </p>
