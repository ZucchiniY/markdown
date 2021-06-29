---
title: iframe 滚动条
tags:
  - 滚动条
  - JavaScript
  - css
categories:
  - 前端技术
  - JavaScript
description: 显示和隐藏 iframe 内的滚动条的方法。
abbrlink: 1c5bba53
date: 2018-06-21 06:43:00
---

## 滚动条重复 

在调用框架或者多级 **iframe** 的时候，经常会出现多个滚动条或者左右都有的情况，需要我们进行调整，现就设置方法记录如下。


## 去掉全部滚动条 


### 设置scrolling属性 

```javascript
scrolling: auto // 在需要的时候显示滚动条
scrolling: yes // 始终显示滚动条
scrolling: no //始终隐藏滚动条
```

### 设置 body 

```css
body {overflow: hidden}
```

可以去看滚动条，也可以用来去看某一个滚动条时的方案。

## 有选择的去掉滚动条 

```css
body {overflow-x: auto; overflow-y: hidden;} /* 去看右边的滚动条，保留下面的 */
body {overflow-x: hidden; overflow-y: auto;} /* 去掉底下的滚动条，保留右边 */
```

## 代码优先级 

如果 `scrolling: auto` 或者 `scrolling:yes` 会依据 **body** 的值显示或者隐藏；如果 `scrolling:no` 无论什么设置都不会再显示滚动条。
