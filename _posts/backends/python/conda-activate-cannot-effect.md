---
title: conda activate 不生效
tags:
  - conda
  - conda activate
categories:
  - 后台技术
  - Python
description: 最近在折腾 Mac OS 下的时候，发现使用 conda activate 环境后，pip 已经是对应的环境上的，但是 python 依然是旧的。
abbrlink: 5adf061e
date: 2021-03-26 16:27:00
---

今天重新看了一下 conda 环境的问题，之前的理解是错误的，因为默认的环境变量配置在不启动 Tmux 的时候是能正常使用的，现在看主要原因是在启动 Tmux 的过程中。

重新检查了一下启动前后的环境变量，发现在启动 Tmux 之后，conda 的相关内容被转移到最后的，导致前面的环境变更覆盖掉了 conda 的环境变量。

正确的方法应该是在 $HOME 下新增一个 **.zprofile** 文件，然后再在文件中增加下面的内容：

```shell
if [ -x /usr/libexec/path_helper ]; then
        PATH="" # <- ADD THIS LINE (right before path_helper call)
        eval `/usr/libexec/path_helper -s`
fi
```

重新加载一下配置文件，就可以正常的使用 conda 命令切换环境了。
