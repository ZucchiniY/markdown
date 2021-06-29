---
title: Android 刷机实践
author: zucchini
tags:
  - 安卓系统
  - 手动更新手机系统
categories:
  - 工具环境
  - Android
keywords:
  - android
  - android-platform-tools
  - fastboot
description: 手动升级安卓系统，如何解锁 V 版手机。
abbrlink: feff8405
date: 2019-04-10 08:44:00
---

## 获取 

在刷机之前，需要在电脑上下载 **Android Preview** 包，一般我都是到[安卓中国](https://developer.android.google.cn/preview/download#flash) ，这里可以下载最新的包。

## 手机 

相对下载包的获取，比较难的是有一部支持最新的安卓系统的手机，一般 **Preview** 版的系统都是默认支持 **Google** 自己的手机的。

目前只支持 Pixel 系列的手机，包括 XL 系列。

- Pixel n
- Pixel n XL

## 刷机 

刷机目前有两个比较麻烦的地方，第一就是需要安装 `adb` 的命令，也就是 **Android** 的功能模块，第二就是需要解锁手机。

### adb 配置 

即将 **Android SDK** 下载下来，然后将其配置到环境变量中即可

#### Windows 

1.  配置 **ANDROID_HOME** 变量到环境变量中
2.  配置 **%ANDROID_HOME%\platform-tools** 到 **path** 中
3.  配置 **%ANDROID_HOME%\tools** 到 **path** 中

#### Linux & Mac 

打开 **profile** 文件，默认为 _.bash\_profile_ 如果使用的是 **zsh** 则编辑 _.zshrc_ 文件。

将下面的内容放到 **profile** 文件中

    :::shell
    ANDROID_HOME=~/developerTools/adt-mac/sdk
    export ANDROID_HOME
    PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

#### Mac 

Mac 电脑提供了一个自动安装的内容，可以将 `adb` 相关的内容直接安装，但是如果是想开发 **Android** 应用的话，则必须要按上面的方案进行配置。

首先需要先安装 **brew** ，具体方案见 [Homebrew](https://brew.sh/index%5Fzh-cn) ，或者可以直接看其 **GitHub** 的主页 [Homebrew/brew](https://github.com/Homebrew/brew) 。

然后执行下面的命令

    :::shell
    brew install --cask android-platform-tools

如果执行刷机的时候，提示 **fastboot is too old** 则需要重新安装 _android-platform-tools_ , 因为 `brew update` 更新是不能更新 _cask_ 库的内容的。

    :::shell
    brew cask reinstall android-platform-tools

最后，在命令行中执行 `adb devices` 不报错刚配置成功。如果配置之后，还依然报错的话，可以检查一下是否在使用过程中，将 **USB 调试功能** 关闭了。

### 操作 

1.  连接手机
2.  `adb devices` 获取手机的 **device id**
3.  `adb reboot bootloader` 进入 **bootloader** 模式
4.  **如果已经解锁了，则进入第8步，如果未解锁则进入第五步**
5.  进入到 **bootloader** 之后，执行 `fastboot flashing unlock`
6.  如果是 **Pixel 2 XL** 则执行 `fastboot flashing unlock_critical`
7.  如果是更早的设备，则需要执行 `fastboot oem unlock`
8.  进入下载的目录，然后执行 **flash-all** 脚本，如果是 _Windows_ 则是 `flash-all.bat` ，其它的则执行 `flash-all.sh`
9.  执行结束后，手机就已经刷好了，重启就可以使用了
10. 如果执行失败的话，就需要解压目录下的 _image_ 对应的包，然后执行下面的命令

        :::shell
        fastboot flash vendor vendor.img
        fastboot flash boot boot.img
        fastboot flash system system.img

然后重启手机就可以了。


### V 版手机解锁 

需要刷入一个工具，才能解锁

    :::shell
    adb push dePixel8 /data/local/tmp
    adb shell chmod 755 /data/local/tmp/dePixel8
    adb shell /data/local/tmp/dePixel8

然后再执行 `adb reboot bootloader` 就可以正常解锁了。

[dePixel8.zip 下载](http://theroot.ninja/depixel8.html)

判断是否 V 版手机

    :::shell
    adb shell getprop|grep cid

如果出现 **VZW_001** 就是 V 版手机
