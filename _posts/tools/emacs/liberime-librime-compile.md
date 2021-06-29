---
title: 编译 librime 和 liberime
tags:
  - librime
  - liberime
categories:
  - 工具环境
  - Emacs
description: 编译 librime 和 liberime 方案。
abbrlink: b325b5d6
date: 2020-04-21 14:26:59
---

## 编译 librime

因为本人使用的是 Mac 系统，所以需要先安装一些工具。

```shell
brew install cmake git boost
```

这三个工具是编译 liberime 用的，本来想从 GitHub 上下载，但是有问题，所以决定自己编译一份，这里测试了一下，只使用 CommandLineTools 是不行的，需要安全安装 xcode 才可以。

1. 下载 librime 版本库

```shell
git clone --recursive https://github.com/rime/librime.git
```

2. 编译第三方库

```shell
cd librime
make xcode/thirdparty
```

3. 编译 librime

```shell
make xcode
```

## 编译 liberime

编译这个是依赖于 _librime_ 文件的，需要先将依赖引进来。
1. 下载 _liberime_ 项目

```shell
git clone git@github.com:merrickluo/liberime.git
```

2. 引入 _librime_ 依赖

```shell
export RIME_PATH=~/DEV/librime
```

3. 编译 _liberime_ 文件

```shell
make liberime-core
```


