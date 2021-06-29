---
title: 利用 Travis 自动部署博客
tags:
  - travis
  - hugo
  - 自动部署博客
categories:
  - 工具环境
  - travis
keywords:
  - travis CI
  - github pages
description: 利用 travis 在文章更新到 Github source 路径后自动编译并部署到 GitHub Pages 上。
author: zucchini
abbrlink: 696d7205
date: 2018-07-15 02:27:00
---

**Travis CI** 是一个非常好用持续集成工具。

**集成** 主要是用来将多个用户的开发模块构建成一个可运行版本；而 **持续集成** 则是在集成之上，尽量将每一次提交都进行一次构建，这个个过程就是 **持续集成** 。

## Travis 自动构建 

**Travis Ci** 的自动构建周期分为两步：

1.  install
2.  Script

但是我们可以根据这两步将相关的内容分成更细的步骤：

1.  before_install
2.  install
3.  befor_script
4.  script
5.  after_success 或者 after_failure
6.  before_deploy
7.  deploy
8.  after_deploy
9.  after_script

**持续集成就是把一系列的手工操作合并成一个脚本的过程。**

所以可以这样实现部署脚本:

```shell
sudo: false
language: go
os: osx
install:
brew install hugo

script:
  - hugo --config jane-config.toml

branches:
  only:
    - source
after_success:
    - git add -A
    - git commit -m "update blog"
    - git push -u origin master
```

这个脚本中，我们主要工作是生成 **hugo** 博客这一步，如果成功了，我们就进行提交，也就完成了。

## Travis GitHub Pages 

经过查阅之后，发现 **Travis Ci** 本身就支持直接部署到 **GitHub Pages** 上，并拥有单独的章节。

### 个人令牌 

在 **GitHub** 中的 **Setting** 下的 **Developer settings** 中，有一个 **Personal access tokens** 中，可以生成，然后配置到 **Travis Ci** 对应的 **My Repositories** 中的项目中，一般的话，使用 **public\_repo** 权限就足够了。

如果在 **My Repositories** 中看不到 **Settings** ，可以在 **More options** 中找到 **Settings** 然后在 **Environment Variables** 中配置对应的令牌即可。

### 个人配置 

在项目中新增 **.travis.yml** ，内容如下：

```shell
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_TOKEN
  # Set in the settings page of your repository, as a secure variable
  keep-history: true
  on:
    branch: master
```

GitHub Pages 默认的一些参数：

- local-dir: 推送到 **GitHub Pages** 的目录，当前路径的相对路径，默认为当前路径
- repo: 项目仓库的地址，默认为当前项目
- keep-history: 可选参数，使用递增的推送信息代替强制推送，默认为 **false**
- target-branch: 将 **local-dir** 内容推送的分支，默认为 **gh-pages** ，如果 **keep-history** 为 **true** ，则强制推送 `git push -f`
- fqdn: 可选参数，设置用户的自定义域名，默认为无自定义域名
- project-name: 默认为 **fqdn** 的值或者是仓库的固定路径，用于元数据
- email: 可选参数，提交人信息，默认为 **deploy@travis-ci.org**
- name: 可选参数，提交人，默认为 **Deployment Bot**
- committer-from-gh: 可选参数，默认为 **false** ，允许令牌拥有者使用，会覆盖掉 **email** 和 **name** 两个的值
- allow-empty-commit: 可选参数，默认为 **false** ，仅在 **keep-history** 参数为 **true** 时启用
- github-url: 可选参数，默认为 **github.com** ，是企业级自托管 **GitHub** 项目的 **URL** 地址
- verbose: 可选参数，冗余的内部步骤，默认为 **false**

备注：

1.  默认情况下，travis 会自动执行 `git submodule init` ，但是需要使用 **https** 的路径，否则需要配置 **ssh key** ，而 GITHUB_TOKEN 要保证和 travis 设置的变量一致，名称和值都要一致
2.  fqdn 如果需要设置自定义域名，可以设置这个参数，travis 会自动生成 **CNAME** 文件提交，同时要设置 **config.toml** 中的相应的 **baseURL**
3.  `branches: only: source` 等价于 `deploy: on: branch: source`


### Environment Variables 

除了令牌之后，我们也可以将 **name** 、 **email** 等相关信息配置到这里，比如推送的分支，可以设置为 **P_BRANCH** 然后在推送的时候，使用 **${P_BRANCH}** 来引用。
