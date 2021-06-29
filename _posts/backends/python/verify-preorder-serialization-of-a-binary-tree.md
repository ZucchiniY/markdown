---
title: 二叉树前序序列化
tags:
  - Leetcode
categories:
  - 后台技术
  - Python
description: 一般的入度出度方法和栈的替换方法。
abbrlink: 58e0c645
date: 2021-03-12 13:04:00
---

{% blockquote LeetCode https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/ 331. 验证二叉树的前序序列化 %}
序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 `#`。

```text
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
```

例如，上面的二叉树可以被序列化为字符串 "9,3,4,#,#,1,#,#,2,#,6,#,#"，其中 # 代表一个空节点。

给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。

每个以逗号分隔的字符或为一个整数或为一个表示 null 指针的 '#' 。
{% endblockquote %}

今天在力扣上看到一道二叉树的题，使用入度和出度的方法编写了实现的代码，如下：

``` python
def isValidSerialization(preorder):
        s = preorder.split(',')
        length = len(s)
        res,i  = 0,0
        for c in s:
            if c != '#':
                res += 1
            else:
                res -= 1
            i += 1
            if(res<0 and i < length):
                return False

        return res == -1
```

在看其它人的题解思路的时候，发现一个使用栈替换的实现方法，非常巧妙，具体的思路是发现「A,#,#」的串的时候，则认为是一个正确的二叉树。

然后将其替换为 「#」，利用栈的进出策略，可以简单的实现代码：

```python
class Solution:
    def isValidSerialization(self, preorder):
        stack = []
        for node in preorder.split(','):
            stack.append(node)
            while len(stack) >= 3 and stack[-1] == stack[-2] == '#' and stack[-3] != '#':
                stack.pop(), stack.pop(), stack.pop()
                stack.append('#')
        return len(stack) == 1 and stack.pop() == '#'
```
