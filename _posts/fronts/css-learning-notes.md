---
title: CSS  语法入门
tags:
  - css
categories:
  - 前端技术
  - css
description: CSS 入门语法知识
abbrlink: 1e4744b6
date: 2016-01-06 06:45:00
---

CSS 是前端开发的基础。CSS 規則由兩個主要的部分構成:選擇器，以及一條或者多條聲明。
CSS 是前端开发的基础，主要由两个部分构成：
1. 选择器
2. 声明，可以是单条，也可以是多条

```CSS
selector { declaration1; declaration2; ... declarationN;}
```

选择器通常是 HTML 中的元素。每条声明都是由一个属性和一个值构成，属性是希望设置的样式、属性，每个属性都有一个值，并用冒号分开。

```CSS
selector {property: value}
```

下面代码的作用是将 `h1` 元素内的文字的颜色定义为红色，同时将字体的大小设置为 14 像素。

在这个例子中，`h1` 是选择器，_color_ 和 _font-size_ 是属性，*red* 和 *14px* 是值。

```CSS
h1 {color : red; font-size: 14px;}
```


如果值是多个词组，可以给值增加引号。

```CSS
p { font-family: "sans serif";}
```

如果林定义不止一个声明，则需要用分号将每个声明隔开。

下面的示例是将段落的字体定义为红色且居中。

虽然最后一个属性是不需要增加分号的，但是为了以后修改，最好在每条属性后面都增加分号分隔符。

```CSS
p {
  text-align: center;
  color: black;
  font-family: arial;
}
```

子元素总是继承你元素的属性。

```CSS
body {
  font-family: Vrdana, sans-serif;
}
```
这样，在 body 属性下的元素：p, td, ul, ol, li, dl, dt, dd 等都会继承 body 中定义的字体，同样继承来的值也可以进行重写。

```CSS
body { font-family: Vrdana;}
p { font-family: Times;}
    ```

选择器、派生选择器：通过依据元素位置的上下文件关系来定义的样式。

```CSS
li strong {
  font-style: italic;
  font-weight: normal;
}
```

ID 选择器：可以为标有特定 ID 的元素指定样式。

```CSS
#red { color: red;}
```

ID 选择器也可以和派生选择器一起使用。

```CSS
# sidebar p{
font-style: italic;
text-align: right;
margin-top: 0.5em;
}
```

单独选择器：可以单独发挥作用的选择器。

```CSS
#sidebar {
border: 1px dotted #000;
padding: 10px;
}
```

类选择器：以一个点号作为开头。

```CSS
.center { text-align: center;}
```

也可以用作派生选择器上。

```CSS
.fancy td {
  color: #f60;
  background: #666;
}
```

元素也可以基于它们的类而被选择。

```CSS
td.fancy {
  color: #f60;
  background: #666;
}
```

上面的两个示例中，第一个是类名为 fancy 的元素内容属性设置，下面的则是指 `<td class='fancy'>` 的元素的属性设置。


| 选择器                 | 描述                                                     |
|------------------------|----------------------------------------------------------|
| [attribut]             | 用于选取带有指定属性的元素                               |
| [attribut=value]       | 用于选取带有指定属性和值的元素                           |
| [attribut~=value]      | 用于选取属性值中包含指定词条的元素                       |
| [attribut&vert;=value] | 用于选取带有以指定开头的属性值的元素，该值必须是整个单词 |
| [attribut^=value]      | 匹配属性值以指定值开头的每个元素                         |
| [attribut$=value]      | 匹配属性值以指定值结尾的每个元素                         |
| [attribut\*=value]     | 匹配属性值中包含指定值的每个元素                         |

CSS 允许应用纯色做为背景，也允许使用背景图片创建一个繁杂的效果。

可以使用 **background-color** 指定背景色，这个属性接受任务合法的颜色值。可以利用这个将背景色配置为灰色。**background-color** 是不能被继承的。

```CSS
p { background-color: gray;}
p { background-color: gray; padding:20px;}
```
