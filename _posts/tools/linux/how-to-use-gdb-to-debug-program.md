---
title: GDB 调试笔记
tags:
  - gdb
categories:
  - 工具环境
  - Linux
description: GDB 常用命令笔记。
abbrlink: efe1991f
date: 2020-01-08 00:00:00
---

GDB 是一个由 GNU 开源组织发布的 *.nix 下的、基于命令行的一款比较知名的程序调试工具。

GDB 有着相当多的命令，但是常用的只有大概十个左右。

gdb命令一般可以使用 `gdb program` 或者使用 `gdb progrma core` 。如果想对正在执行的进程进行调试，则可以使用 `gdb -p 123` 。


## 常见命令 

<table>
<tr>
    <th>命令</th>
    <th> 解释</th>
    <th> 示例</th>
</tr>
<tr>
    <td>file <文件名></td>
    <td>加载被调试的可执行程序文件。<br />因为一般都在被调试程序所在目录下执行GDB，因而文本名不需要带路径。</td>
    <td>(gdb) file gdb-sample</td>
</tr>
<tr>
    <td>r</td>
    <td>run的简写，运行被调试的程序。<br />如果此前没有下过断点，则执行完整个程序；如果有断点，则程序暂停在第一个可用断点处。</td>
    <td>(gdb) r</td>
</tr>
<tr>
    <td>c</td>
    <td>Continue的简写，继续执行被调试程序，直至下一个断点或程序结束。</td>
    <td>(gdb) c</td>
</tr>
<tr>
    <td>b &lt;行号&gt;<br />b &lt;函数名称&gt;<br />b \*&lt;函数名称&gt;<br />b \*&lt;代码地址&gt; <br />d [编号]</td>
    <td>b: Breakpoint的简写，设置断点。两可以使用“行号”“函数名称”“执行地址”等方式指定断点位置。<br />其中在函数名称前面加“\*”符号表示将断点设置在“由编译器生成的prolog代码处”。如果不了解汇编，可以不予理会此用法。<br />d: Delete breakpoint的简写，删除指定编号的某个断点，或删除所有断点。断点编号从1开始递增。</td>
    <td>(gdb) b 8<br />(gdb) b main<br />(gdb) b \*main<br />(gdb) b \*0x804835c<br />(gdb) d</td>
</tr>
<tr>
    <td>bt</td>
    <td>查看函数运行时堆栈</td>
    <td>(gdb) bt</td>
</tr>
<tr>
    <td>disas <functionName></td>
    <td>默认反汇编对应的方法</td>
    <td>(gdb) disas </td>
</tr>
<tr>
    <td>s, n</td>
    <td>s: 执行一行源程序代码，如果此行代码中有函数调用，则进入该函数；<br />n: 执行一行源程序代码，此行代码中的函数调用也一并执行。<br />s 相当于其它调试器中的“Step Into (单步跟踪进入)”；<br />n 相当于其它调试器中的“Step Over (单步跟踪)”。<br />这两个命令必须在有源代码调试信息的情况下才可以使用（GCC编译时使用“-g”参数）。</td>
    <td>(gdb) s<br />(gdb) n</td>
</tr>
<tr>
    <td>si, ni</td>
    <td>si命令类似于s命令，ni命令类似于n命令。所不同的是，这两个命令（si/ni）所针对的是汇编指令，而s/n针对的是源代码。</td>
    <td>(gdb) si<br />(gdb) ni</td>
</tr>
<tr>
    <td>p &lt;变量名称&gt;</td>
    <td>Print的简写，显示指定变量（临时变量或全局变量）的值。</td>
    <td>(gdb) p i<br />(gdb) p nGlobalVar<br />(gdb) p/a</td>
</tr>
<tr>
    <td>display ... <br />undisplay &lt;编号&gt;</td>
    <td>display，设置程序中断后欲显示的数据及其格式。<br />例如，如果希望每次程序中断后可以看到即将被执行的下一条汇编指令，可以使用命令<br />“display /i $pc”<br />其中 $pc 代表当前汇编指令，/i 表示以十六进行显示。当需要关心汇编代码时，此命令相当有用。<br />undispaly，取消先前的display设置，编号从1开始递增。</td>
    <td>(gdb) display /i $pc<br />(gdb) undisplay 1</td>
</tr>
<tr>
    <td>i</td>
    <td>Info的简写，用于显示各类信息，详情请查阅“help i”。</td>
    <td>(gdb) i r 打印寄存器<br />(gdb) i proc m 检查是否为有效地址</td>
</tr>
<tr>
    <td>reverse-stepi</td>
    <td>回退之前执行过的指令</td>
    <td>(gdb) reverse-stepi</td>
</tr>
<tr>
    <td>q</td>
    <td>Quit的简写，退出GDB调试环境。</td>
    <td>(gdb) q</td>
</tr>
<tr>
    <td>record</td>
    <td>录制程序过程</td>
    <td>(gdb) record</td>
</tr>
<tr>
    <td>help [命令名称]</td>
    <td>GDB帮助命令，提供对GDB名种命令的解释说明。<br />如果指定了“命令名称”参数，则显示该命令的详细说明；如果没有指定参数，则分类显示所有GDB命令，供用户进一步浏览和查询。</td>
    <td>(gdb) help display</td>
</tr>
</table>


## 用法总结 

~这里的总结主要是整理自 [gdb 调试入门，大牛写的高质量指南](http://blog.jobbole.com/107759/)，我觉得这篇文章是可以反复阅读的好文章。~

发现这篇文章已经不存在了，希望我现在写的这篇总结对大家有帮助吧。

{% img https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/gdb.png %}


## 补充小工具 

python dbg工具，可以通过 `apt-get install -y python-dbg` 进行安装，然后可以在其中使用 `py-bt` 、 `py-list` 等命令。

另外还有一个工具是 **cscope** ，主要用来遍历代码用的。

cscope -bqR : 建立查找数据库

cscope -dq : 启动cscope
