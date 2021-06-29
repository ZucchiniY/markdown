---
title: 忘记 Django admin 管理员用户密码
tags:
  - Django admin
categories:
  - 后台技术
  - Python
description: 如何能过 Python 程序修改 Django 管理员密码。
abbrlink: 9658455d
date: 2021-03-17 13:56:00
---


## 通过 orm 修改用户密码

首先通过 `python manage.py shell` 启动 django shell 环境。

然后通过下面的代码修改用户密码：

``` python
from django.contrib.auth.models import User
user = User.objects.get(username = 'username')
user.set_password('new_password')
user.save()
```

执行上面的代码，就可以直接修改对应的密码了。
