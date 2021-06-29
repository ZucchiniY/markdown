---
title: Homebrew 入门
tags:
  - homebrew
categories:
  - 工具环境
  - Mac
description: homebrew 安装配置和国内镜像源的选择。
abbrlink: 711ab194
date: 2018-04-27 09:19:00
update: 2021-06-29 11:12:00
---

## homebrew 安装 

使用下面的命令进行安装，但是需要先安装 `curl` :

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 常用命令 

-   搜索

```shell
brew search mysql
```

-   查询

```shell
brew info mysql
```

主要看具体的信息，比如目前的版本，依赖，安装后注意事项等

-   更新

```shell
brew update
```

这会更新 Homebrew 自己，并且使得接下来的两个操作有意义

-   检查过时

```shell
brew outdated
```

这回列出所有安装的软件里可以升级的那些

-   升级

```shell
brew upgrade
```

升级所有可以升级的软件们

-   清理

```shell
brew cleanup
```

清理不需要的版本极其安装包缓存

## 后台启用服务

`brew services` 命令是用来管理 Mac 系统中后台服务的，比如在 Mac 上安装了 MySQL ，当我希望将这个变成一个后台服务启动的时候，可以使用，有点像 Linux 下的 `service` 和 `systemctl` 两个命令。

具体的使用命令也非常简单：

```shell
brew services list  # 查看使用brew安装的服务列表
brew services run formula|--all  # 启动服务（仅启动不注册）
brew services start formula|--all  # 启动服务，并注册
brew services stop formula|--all   # 停止服务，并取消注册
brew services restart formula|--all  # 重启服务，并注册
brew services cleanup  # 清除已卸载应用的无用的配置
```

## 配置国内镜像 

使用了一段时间的 **Homebrew** 之后，发现网络波动有点大，好多时间都是更新10多分钟，所以就想到了国内镜像问题。

其实无论是什么内容，只要是需要更新的，就有两个优先选择的，一个是清华源，一个是科大源，这两个是最好用的镜像了。

对于我的使用，主要是两个，一个是 **formula** 索引，另一个是 **bottles** 。

Homebrew 的 bottles 文件迁移到了 GitHub Packages ，镜像地址需要修改，但是使用的 **清华源** 和 **腾讯源** 都没有做更新，所以更新的时候报错 404 了，查了一下发现 **科大源** 已经更新了，所以就将所有的配置迁移到 **科大源** 之上。

看了一下帮助文档，`homebrew-cask-fonts` 和 `homebrew-cask-drivers` 两个仓库在 **科大源** 中是没有的，所以还是使用 **清华源** 作为镜像。

**formula** 更新使用。

```shell
# brew 程序本身，Homebrew/Linuxbrew 相同
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git

git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
git -C "$(brew --repo homebrew/cask-fonts)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask-fonts.git
git -C "$(brew --repo homebrew/cask-drivers)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask-drivers.git

# 更换后测试工作是否正常
brew update
```

**bottles** 镜像则需要配置到环境变量中，我使用的是 zsh shell 所以配置到 **.zprofile** 文件中

```shell
echo `export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles/bottles` >> ~/.zprofile 
source ~/.zprofile 
```

如果你想临时使用的话，则需要在终端中输入 `export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles` ，当然如果你使用的是 bash shell 则可以将其配置到 **.bash_profile** 文件中。

如果不再希望使用图内的镜像，也可以重置为 *GitHub* 镜像。

```shell
# brew 程序本身，Homebrew/Linuxbrew 相同
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git

# 以下针对 mac OS 系统上的 Homebrew
git -C "$(brew --repo homebrew/core)" remote set-url origin https://github.com/Homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://github.com/Homebrew/homebrew-cask.git
git -C "$(brew --repo homebrew/cask-fonts)" remote set-url origin https://github.com/Homebrew/homebrew-cask-fonts.git
git -C "$(brew --repo homebrew/cask-drivers)" remote set-url origin https://github.com/Homebrew/homebrew-cask-drivers.git

# 更换后测试工作是否正常
brew update
```
