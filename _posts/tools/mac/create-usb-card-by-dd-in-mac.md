---
title: 使用 Mac 电脑制作 U 盘
tags:
  - dd
  - diskutil
categories:
  - 工具环境
  - Mac
keywords:
  - dd
  - diskutil
  - hdiutil
description: 在 Mac 电脑制作 U 盘启动盘。
abbrlink: 74995ec8
date: 2018-04-25 02:47:00
---

## Mac 下写入命令 

-   找出 U 盘挂载位置

```shell
diskutil list
```

-   将 U 盘移除

```shell
diskutil unmountDisk /dev/disk[num]
```

-   写入 U 盘

```shell
sudo dd if=isopath of=/dev/disk[num] bs=1m rdisk
```

`rdisk` 是指定方式后，可以加快写入速度。

## iso 转换为 dmg 

```shell
sudo hdiutil convert -format UDRW -o linux.dmg kali.iso
```


## 弹出 U 盘 

```shell
diskutil eject /dev/disk[num]
```
