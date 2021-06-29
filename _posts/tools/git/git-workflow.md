---
title: Git 工作流
tags:
  - workflow
  - 工作流
categories:
  - 工具环境
  - Git
type: thematics
description: git 最佳实践——工作流的开发管理方案。
abbrlink: fea0ccee
date: 2016-03-17 08:43:00
---

## 中心化的工作流 

### 优势 

- 首先它让每个开发者都有自己的本地的完整项目副本。隔离的环境使得每个开发都的工作独立于项目的其它修改 —— 他们可以在自己的本地仓库中添加提交，完全无视上游的开发，直到需要的时候。
- 其次，它让你接触到了 Git 分支和合并模型。Git 分支被设计为故障安全的机制，用来在仓库之间整合代码和共享更改。

### 如何工作 

-   中心化的工作将中央仓库作为项目中所有修改的唯一入口。默认的开发分支叫做 master，所有的更改都被提交到这个分支。这种工作流不需要 master 之外的其它分支。
-   开发者将中央仓库克隆到本地后开始工作。在他们的本地项目副本中，他们可以像 SVN 一样修改文件和提交更改；不过这些新的提交被保存在本地 —— 它们和中央仓库完全隔离。这使得开发者可以将和上游的同步推迟到他们方便的时候。
-   为了向官方项目发布修改，开发者将他们本地 master 分支“推送”到中央仓库。这一步等同于 svn commit，除了 Git 添加的是所有不在中央 master 分支上的提交。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-images-01.png)

### 管理冲突 

-   中央仓库代码官方项目，因此它的提交历史应该被视为不可更改的。如果开发者的本地提交和中央仓库分叉了，Git 会拒绝将它们的修改推送上去，因为这会覆盖官方提交。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-images-02.png)

-   在开发在提交功能之前，需要 fetch 更新中央提交，在它们之上 rebase 自己的更改。
-   如果本地修改和上游提交的冲突时，Git 会暂停 rebase 流程，给你机会手工解决这些冲突。Git 很赞的一点是，它将 git status 和 git add 命令同时用来生成提交和解决合并冲突。这使得开发能够轻而易举的管理他们的合并。另外，如果他们改错了什么，Git 能让他们轻易的退出 rebase 过程，然后重试。


### 例子 

-   项目管理员生成一个空的版本库

        :::shell
        ssh user@host git init --bare /path/to/repo.git

-   三个人 A, B, C 同时编写同一个项目，需要先在本地创建一个完整的项目副本。

        :::shell
        git clone ssh://user@host/path/to/repo.git

此时，Git 自动添加了一个名为 origin 的运程连接，指向中央仓库，以方便提交。
A 可以使用标准 Git 提交流程开发功能：编辑、缓存、提交。

    :::shell
    git status
    git add <some file>
    git commit

同时，B 也在本地进行自己的开发工作。

-   A 发布了他们修改

        :::shell
        git push origin master

此时中央仓库会将 master -> origin/master

-   B 试图发布修改

        :::shell
        git push origin master

但是因为 A 已经提交了功能到中央仓库，导致 B 的本地历史和中央仓库分叉，Git 会拒绝本次提交。

-   B 如果想提交，必须要先 rebase 本地仓库

可以使用 git pull 来拉取并修改，

    :::shell
    git pull --rebase origin master

-   --rebase 命令告诉 Git，在同步中央仓库的修改之后，将 B 的所有提交移到 master 分支的顶端。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-images-02.png)


-   如果没有冲突的文件，B 就可以直接进行提交了，但是如果存在冲突，可以根据提示查找冲突的文件，修改之后，可以继续 rebase 操作。

        :::shell
        git add <some-file>
        git rebase --continue

同样的，如果此时不知道自己做了什么，可以回滚一次操作。

    :::shell
    git rebase --abort

-   然后再进行 push 就可以提交到中央版本库了。


## 基于功能人分支的工作流 


### Feature 分支工作流 

-   掌握了中心化工作流的使用姿势，在你的开发流程中添加功能分支是一个简单的方式，来促进协作和开发者之间的交流。这种封装使得多个开发专注自己的功能，而不会打扰主代码库。它还能保证 master 分支永远不会包含损坏的代码，给持续集成环境带来了很大的好处。
-   封装功能的开发使得 pull request 的使用成为可能，用来启动围绕一个分支的讨论。它给了其他开发者在功能并入主项目之前参与决策的机会。或者，如果你开发功能时卡在一半，可以发起一个 pull request，向同事寻求建议。重点是：pull request 使得团队在评论其他人的工作时，变得非常简单。


### 如何工作 

-   Feature 分支工作流同样使用中央仓库，master 同样代码官方的项目历史。但是与其直接提交在本地的 master 分支，开发者每次进行新的工作时创建一个新的分支。Feature 分支应该包含描述性的名称，比如 animated-menu-items(菜单项动画)或 issue-\*1061。每个分支都应该有一个清晰、高度集中的目的。
-   Git 在技术上无法区别 master 和功能分支，所以开发者可以在 feature 分支上编辑、缓存、提交，就和中心化工作流中一样。
-   此外，feature 分支可以被推送到中央仓库。这使得你和其他开发者共享这个功能，而又不改变官方代码。既然 master 只是一个“特殊”的分支，在中央仓库中储存多个 feature 分支不会引出什么问题。当然，这也是备份每个开发者本地提交的好办法。


### Pull Request 

-   除了隔离功能开发之外，分支使得通过 pull request 讨论修改成为可能。一旦有人完成了一个功能，他们不会立即将它并入 master。他们将 feature 分支推送到中央服务器上，发布一个 pull request，请求将他们的修改并入 master。这给了其他开发者在修改并入主代码库之前审查的机会。
-   代码审查是 pull request 的主要好处，但他们事实上被设计成为讨论代码的一般场所。你可以把 pull request 看作是专注某个分支的讨论版。也就是说他们可以用于开发流程之前。比如，一个开发者在某个功能上需要帮助，他只需要发起一个 pull request。感兴趣的小伙伴会自动收到通知，看到相关提交中的问题。
-   一旦 pull request 被接受了，发布功能的行为和中心化的工作流是一样的。首先，确定你本地的 master 和上游的 master 已经同步。然后，将 feature 分支并入 master 已经同步。然后可以将 feature 分支并入 master，将更新的 master 推送回中央仓库。


## Gitflow 工作流 

-   GitFlow 工作流围绕项目发布定义了一个严格的分支模型。有些地方比功能分支工作流更复杂，为管理大型项目提供了框架。
-   和功能分支工作流相比，这种工作流没有增加任何新的概念或命令。它给不同的分支指定了特定的角色，定义它们应该如何、什么时候交流。除了功能分支之外，它还为准备发布、维护发布、记录发布分别使用了单独的分支。当然，还能享受到功能分支工作流带来的所有好处：pull request、隔离实验和更高效的协作。


### 如何工作 

-   GitFlow 工作流仍然使用中央仓库作为开发者沟通的中心。和其它工作流一样，开发者在本地工作，将分支推送到中央仓库。唯一的区别在于项目的分支结构。


#### 历史分支 

-   和单独的 master 分支不同，这种工作流使用两个分支来记录项目历史。master 分支储存官方发布历史，develop 分支用来整合功能分支。同时，这还方便了在 master 分支上给所有提交打上版本号标签。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-images-02.png)


-   工作流剩下的部分围绕这两个分支的差别展开。


#### 功能分支 

-   每个新功能都放置在自己的分支中，可以在备份/协作时推送到中央仓库。但是与其合并到 master，功能分支将开发分支作为父分支。当一个功能完成时，它将被合并回 develop。功能永远不应该支持在 master 上交互。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-images-02.png)


-   功能分支加上 develop 分支就是我们之前据说的功能分支工作流。


#### 发布分支 

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-images-02.png)


-   一旦 develop 分支的新功能足够发布，你可以从 develop 分支 fork 一个发布分支。这个分支的创建开始了下个发布周期，只有和发布相关的任务应该在这个分支进行，如修复 bug、生成文档等。一旦准备好发布，发布分支将合并进 master，打上版本号的标签。另外，它也应该合并回 develop，后者可能在发布启动之后有了新的进展。
-   使用一个专门的分支来准备发布确保一个团队完善当前的发布，其它团队可以继续开发下一个发布的功能。它还建立了清晰的开发阶段。
-   通常约定：
    -   从 develop 创建分支
    -   合并进 master 分支
    -   命名规范 release-\* 或者 release/\*


#### 维护分支 

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-images-02.png)


-   维护或者“紧急修复”分支用来快速给产品发布打上补丁。这是唯一可以从 master 上 fork 的分支。一旦修复完成了，它应该被并入 master 和 develop 分支，master 应该打上更新的版本号的标签。
-   有一个专门的 bug 修复开发线使得团队能够处理 issue，而不打断其它工作流或是要等到下一个发布周期。你可以将维护分支看作在 master 分支上工作的临时发布分支。


### 例子 


#### 创建一个开发分支 

-   为默认的 master 分支创建一个互补的 develop 分支。最简单的办法是在本地创建一个空的 develop 分支，将他推送到服务器上：

        :::shell
        git branch develop
        git push -u origin develop

-   这个分支将会包含项目中所有的历史，而 master 将包含不完全的版本。其他开发者应该将中央仓库克隆到本地，创建一个分支来追踪 develop 分支：

        :::shell
        git clone http://xxx/xx/repo.git
        git checkout -b develop origin/develop

#### 开始了新的功能 

-   当两个人都需要在不同分支上开始工作，即为自己的功能创建单独的分支。且他们的分支都是基于 develop 而不是 master：

        :::shell
        git checkout -b some-feature develop

-   他们都使用“编辑、缓存、提交”的一般约定来向功能分支添加提交：

        :::shell
        git status
        git add <some-file>
        git commit


### 完成功能 

-   添加了一些提交后，可以使用 pull request，现在正是发起的好时机，请求将新功能并入 develop 分支。否则可以先并入本地的 develop 分支，推送到中央仓库：

        :::shell
        git pull origin develop
        git checkout develop
        git merge some-feature
        git push
        git branch -d some-feature

-   第一个命令在尝试并入功能分支之前确保 develop 分支已经是最新的。注意，功能绝不该直接并入 master。冲突的处理方式和中心化工作流相同。


### 发布新功能 

-   当另外的开发人员，仍在他自己的分支上工作时，开始准备项目的第一个官方发布。和开发功能一样，新建一个分支来封装发布的准备工作。这也正是发布的版本号创建的第一步：

        :::shell
        git checkout -b release-0.1 develop

-   这个分支用来整理提交，充分测试，更新文档，为即将到来的发布做各种准备。它就像是一个专门用来完善发布的功能分支。
-   一旦发布准备稳妥，即将其并入 master 和 develop，然后删除发布分支。合并回 develop 很重要，因为可能已经有关键的更新添加到发布分支上，而开发新功能需要用到它们。同样的，如果团队重视代码审查，现在将是发起 pull request 的完美时机。

        :::shell
        git checkout master
        git merge release-0.1
        git push
        git checkout develop
        git merge release-0.1
        git push
        git branch -d release-0.1

-   发布分支是功能开发（develop）分支和公开发布（master）之间的过渡阶段。不论什么时候，将提交并入 msater 时，你应该为提交打上方便引用的标签：

        :::shell
        git tag -a 0.1 -m "Initial public release" master
        git push --tags

-   Git 提供了许多钩子，即仓库中特定事件发生时被执行的脚本。当你向中央仓库推送 master 分支或者标签时，你可以配置一个钩子来自动化构建公开发布。


### 终端用户发现一个 Bug 

-   正式发布之后，两个开发一起为下一个发布开发功能。这时，一个终端用户开了一个 issue 抱怨说当前发布中存在一个 Bug。为了解决这个 bug，先从 master 创建一个维护分支，用几个提交修复这个 issue，然后直接合并回 master。

        :::shell
        git checkout -b issue*001 master
        ##Fix the bug
        git checkout master
        git merge issue-*001
        git push

-   和发布分支一样，维护分支包含了 develop 中需要的重要更新，因此需要执行同样的合并。接下来，可以删除这个分支：

        :::shell
        git checkout develop
        git merge issue-*001
        git push
        git branch -d issue-*001


### 各分支的意义 

-   feature (多个) 主要是自己玩了，差不多的时候要合并回 develop 去。不与 master 交互。
-   develop (同时间一个) 主要是和 feature 以及 release 交互
-   release (同时间一个) 总是基于 develop，最后又合并回 develop。当然对应的 tag 要合并到 master 分支，生命周期短，仅是为了发布程序
-   hotfix (同一时间一个) 总是基于 master，并最后合并到 master 和 develop。生命同期较短，用来修复 bug 或小粒度修改发布
-   master (仅一个) 关联 tag 和发布


### 模型中各个模块内容的使用 

-   在这个模型中，master 和 develop 都具有象征意义。master 分支上的代码总是稳定的 (stable build)，随时可以发布出去。develop 上的代码总是从 feature 上合并过来的，可以进行 Nightly Builds，但不直接在 develop 上进行开发。当 develop 上的 featur 足够多以致于可以进行新版本的发布时，可以创建 release 分支。
-   release 分支基于 develop，进行委阴简单的修改后就被合并到 master，并打上 tag，表示可以发布了。紧接着 release 将被合并到 develop；此时 Develop 可能往前跑了一段，出现合并冲突，需要手工解决冲突后再次合并，这步完成后就删除 release 分支
-   当从已发布版本中发现 bug 要修复时，就应用到 hotfix 分支了。hotfix 基于 master 分支，完成 bug 修复或者紧急修改后，要 merge 回 master，打上一个新的 tag，并 merge 回 develop，删除 hotfix 分支。
-   由此可见 release 和 hotfix 的生命周期都较短，而 master 和 develop 虽然总是存在，但去不常使用。
