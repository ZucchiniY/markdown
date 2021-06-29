---
title: Mac 下使用 tmux
tags:
  - tmux
categories:
  - 工具环境
  - Mac
description: Mac 上安装 Tmux 进行管理 Terminal。
abbrlink: cd94d23e
date: 2019-08-01 06:37:00
---

## 安装 tmux 

`brew install tmux` 可以直接安装到电脑中。


## 简单使用 


### 打开 

在命令行中，直接输入 `tmux` 即可启


### 切分窗口 

`ctrl + b` 可以启动命令模式，类似 **vim** 下的 **:** 。然后再按 **%** 可以进行水平切分。

如果想到垂直切分，则按下 **"** 即可。


### 后台执行 

`ctrl + b` 然后按 **d** 可以将这个后台隐藏，如果想回到隐藏的进程，可以输入 **tmux attach** 即可。


## 基本概念 

**Session** : 会话，一组窗口的集合，通常来概括一个任务， **Session** 可以有自己的名字用来切换

**Window** : 窗口，单个可见窗口，有自己的编号，可以快捷切换。

**Pane** : 窗格，被划分可小块的窗口，类似于 **vim** 中的 `C-w +v` 。


## 快捷键 

`ctrl + b` 来激活快捷键，开启后可以使用一些特定按键来执行操作。

<table>
    <tr>
        <th>分类</th>
        <th>快捷键</th>
        <th>功能</th>
    </tr>
    <tr>
        <td>基础</td>
        <td>?</td>
        <td>获取帮助信息</td>
    </tr>
    <tr>
        <td rowspan="3">Session管理</td>
        <td>s</td>
        <td>列出所有会话</td>
        <tr>
            <td>$</td>
            <td>重命名当前会话</td>
        </tr>
        <tr>
            <td>d</td>
            <td>断开当前会话</td>
        </tr>
    </tr>
    <tr>
        <td rowspan="11">Window管理</td>
        <td>c</td>
        <td>创建一个新窗口</td>
        <tr>
            <td>,</td>
            <td>重命名当前窗口</td>
        </tr>
        <tr>
            <td>w</td>
            <td>列出所有窗口</td>
        </tr>
        <tr>
            <td>%</td>
            <td>水平分割窗口</td>
        </tr>
        <tr>
            <td>"</td>
            <td>垂直分割窗口</td>
        </tr>
        <tr>
            <td>n</td>
            <td>选择下一个窗口</td>
        </tr>
        <tr>
            <td>p</td>
            <td>选择上一个窗口</td>
        </tr>
        <tr>
            <td>0~9</td>
            <td>选择0~9对应的窗口</td>
        </tr>
        <tr>
            <td>l</td>
            <td>在前后两个窗口间切换</td>
        </tr>
        <tr>
            <td>w</td>
            <td>通过窗口列表切换窗口</td>
        </tr>
        <tr>
            <td>f</td>
            <td>在所有窗口中查找指定文本</td>
        </tr>
    </tr>
    <tr>
        <td rowspan="16">Pane管理</td>
        <td>%</td>
        <td>创建水平窗格</td>
        <tr>
            <td>"</td>
            <td>创建一个垂直窗格</td>
        </tr>
        <tr>
            <td>h</td>
            <td>将光标移入下左侧窗格</td>
        </tr>
        <tr>
            <td>j</td>
            <td>将光标移入下下方窗格</td>
        </tr>
        <tr>
            <td>l</td>
            <td>将光标移入下右侧窗格</td>
        </tr>
        <tr>
            <td>k</td>
            <td>将光标移入下上方窗格</td>
        </tr>
        <tr>
            <td>q</td>
            <td>显示窗格编号</td>
        </tr>
        <tr>
            <td>o</td>
            <td>在窗格间切换</td>
        </tr>
        <tr>
            <td>}</td>
            <td>与下一个窗格交换位置</td>
        </tr>
        <tr>
            <td>{</td>
            <td>与上一个窗格交换位置</td>
        </tr>
        <tr>
            <td>!</td>
            <td>在新窗口中显示当前窗格</td>
        </tr>
        <tr>
            <td>x</td>
            <td>关闭当前窗格</td>
        </tr>
        <tr>
            <td>SPC</td>
            <td>循环切换窗格布局</td>
        </tr>
        <tr>
            <td>Alt + o</td>
            <td>逆时针旋转窗格面板</td>
        </tr>
        <tr>
            <td>Ctrl + o</td>
            <td>顺时针旋转窗格面板</td>
        </tr>
        <tr>
            <td>方向键</td>
            <td>移动光标选择面板</td>
        </tr>
    </tr>
    <tr>
        <td rowspan="2">其它</td>
        <td>t</td>
        <td>在当前窗格显示时间</td>
        <tr>
        <td>z</td>
        <td>最大化和最小化当前窗口</td>
        </tr>
    </tr>
</table>
