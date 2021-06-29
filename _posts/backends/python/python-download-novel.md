---
title: 用 Python 爬取小说
tags:
  - beautifusoup
  - 下载小说
categories:
  - 后台技术
  - Python
keywords:
  - beautifusoup
  - download
description: 利用 Python 爬取全本小说。
abbrlink: 6dd2e3ae
date: 2018-08-15 02:41:00
---

Python 爬取网络的内容是非常方便的，但是在使用之前，要有一些前端的知识，比如： HTML、 CSS、XPath 等知识，再会一点点 Python 的内容就可以了。

因为使用的是 Anaconda ，所以大多数的包都已经有了，但是在使用过程中也有一些小问题，但是最终程序是实现了的。

-   BeautifulSoup 是一个可以从HTML或XML文件中提取数据的Python库。非常好用，具体的 [文档可以从这里跳转](https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html) ，利用这篇文章可以让你轻松的进行网页的解析。可以把仅有的一点前端知识也略去了。
-   requests 适合正常人类使用的一个 HTTP 解析工具
-   time 让网站以为你不是电脑
-   sys 显示和刷新

代码：

```python
# _*_ coding:UTF-8 _*_
from bs4 import BeautifulSoup
import requests
import time
import sys
"""
download 《武动乾坤》 from www.biqukan.com
Parameters:
    None
Returns:
    None
"""


class downloader(object):
    def __init__(self):
        self.server = "http://www.biqukan.com"
        self.target = "http://www.biqukan.com/3_3012"
        self.names = []
        self.urls = []
        self.nums = 0
        """
        To get Urls for download
        """

    def get_download_url(self):
        req = requests.get(url=self.target)
        html = req.text
        div_bf = BeautifulSoup(html, "html.parser")
        div = div_bf.find_all("div", class_="listmain")
        a_bf = BeautifulSoup(str(div[0]), "html.parser")
        a = a_bf.find_all("a")
        self.nums = len(a[12:])
        with open('武动乾坤目录.txt', 'a', encoding='utf-8') as f:
            for each in a[12:]:
                f.write(each.string)
                self.names.append(each.string)
                f.write(self.server + each.get("href"))
                self.urls.append(self.server + each.get("href"))
                """
    To get content
    Parameters:
        target - 下载链接
    Returns:
        content - 章节内容
    """

    def get_contents(self, target):
        headers = requests.utils.default_headers()
        headers['User-Agent'] = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:61.0) Gecko/20100101 Firefox/61.0'
        req = requests.get(url=target, headers=headers)
        html = req.text
        bf = BeautifulSoup(html, "html.parser")
        texts = bf.find_all("div", class_="showtxt")
        content = texts[0].text.replace('\xa0' * 8, '\n\n')
        return content
    """
    To save to text
    Parameters:
        name - 章节名称
        path - 当前路径 + 小说名
        text - 章节内容
    Returns:
        None
    """

    def writer(self, name, path, text):
        # writer_flag = True
        with open(path, 'a', encoding='utf-8') as f:
            f.write(name + '\n')
            f.writelines(text)
            f.write('\n\n')


if __name__ == "__main__":
    dl = downloader()
    dl.get_download_url()
    print("第", dl.nums, "章")
    print("开始下载:")
    for i in range(dl.nums):
        time.sleep(1)
        dl.writer(dl.names[i], '武动乾坤.txt', dl.get_contents(dl.urls[i]))
        sys.stdout.write("  已下载:%.3f%%" % float(i/dl.nums*100) + '\r')
        sys.stdout.flush()
        print("下载完成")
```

几个小点需要注意：

1.  不能访问的过快，所以在循环中进行一次等待，我这里用的是 `time.sleep(1)`
2.  为了不被反爬虫识别为爬虫，需要在访问的时候，增加一个 **header** ，利用 `headers = requests.utils.default_headers()` 和 `headers['User-Agent'] = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.13; rv:61.0) Gecko/20100101 Firefox/61.0'` 两行，就可以不被识别了
3.  解析的时候，出现了一个问题，就是一开始从目录页获取的时候，只能读取 193 篇文章，经过排查，发现是在使用 BeautifulSoup 的时候解析的有点问题，将原本的 `"lxml"` 方式修改为`"html.parser"` 方式就可以了

不过因为这本小说字数真的有点多，所以下载过程有点慢，不过整体来说还是可以使用的。
