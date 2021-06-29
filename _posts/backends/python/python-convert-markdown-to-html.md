---
title: Python 转换 Markdown 方法
tags:
  - django
  - markdown
  - markdown-deux
categories:
  - 后台技术
  - Python
keywords:
  - markdown
  - django
  - html
description: 利用 Python 将 Markdown 转换为 HTML 。
abbrlink: 9a20479
date: 2019-10-12 02:43:00
---


昨天看书的时候，看到了作者留的一个练习，是将 Markdown 的文档转换成 HTML 的方法。类似的标记语言还有 RestruredText 和 Org Mode，但是貌似 Org 比较小众，暂没有。

在 Django 中进行转换有两个方案，一个是直接安装 markdown 的模块，`pip install markdown` ，这种方式是直接将 Markdown 渲染成 HTML ，但是在 Django 项目中需要使用模板进行渲染，会导致样式改变，所以需要增加 `safe` 到模板中，表示不需要转义。

代码块如下：

```python
# 在 view.py 里使用 markdown 进行渲染

import markdown
def page(request, name):
    template = get_template('doc.html')
    docfile = get_template('doc/{}.md'.format(name))
    content = docfile.render()
    html = template.render({
        'docname' : name
        'content' : markdown.markdown(content,
                                      extensions=[
                                      'markdown.extensions.extra',
                                      'markdown.extensions.codehilite',
                                      'markdown.extensions.tox',
                                      ])
                                  })
    return HttpResponse(html)
```


Html 文件如下

```html
<!DOCTYPE>
<html lang='en'>
    <body>
    {{ content | safe }}
    </body>
</html>
```

另一个方案是使用 **django.markdown-deux** 进行渲染，首先在 *INSTALL_APPS* 中增加 **markdown-deux** , 然后模板里引用这个标签：

```html
<!DOCTYPE>
<html lang='en'>
    <body>
        {% load markdown-duex-tags %}
        {{ content | markdown }}
    </body>
</html>
```

这样也可以将 Markdown 直接渲染成网页了。
