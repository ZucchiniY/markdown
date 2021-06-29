---
title: Selenium 操作下拉框
tags:
  - Selenium
  - 下拉框
categories:
  - 后台技术
  - Java
description: 使用 Selenium 操作下拉框。
abbrlink: 9b20002f
date: 2019-02-27 09:05:00
author: zucchini
---

使用 Selenium 进行 Web 端的自动化测试的时候，不能通过 findElement 进行自动选择，后来发现，需要先声明一个 Select 类型，再进行选择，实现方法如下：

```java
Select dropdown = new Select(driver.findElement(By.id("selectId")));
dropdown.selectByValue("optionValue");
// 或者使用 Index
dropdown.selectByIndex(0);
// 或者使用下拉框中的内容
dropdown.selectByVisibleText("content");
```

这样就可以操作下拉框了。
