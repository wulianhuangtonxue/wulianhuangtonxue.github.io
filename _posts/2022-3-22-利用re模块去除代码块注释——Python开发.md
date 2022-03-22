---
layout: post
title: 利用re模块去除代码块注释
subtitle: Python开发
datae: 2022-3-22
author: 物联黄同学
header-img: img/blogimg/20220321.jpg
catalog:	true
tags:
    - Python
    - 正则表达式
---

# 利用re模块去除代码块注释——Python开发

[TOC]

## 前言

> 上次做了用于输入样例格式修改，相当于测试用例的过滤器，这次我们使用类似的思路来做一个对于像C++代码中块注释的过滤器。
>
> ```c++
> /**
>  * Definition for a binary tree node.
>  * struct TreeNode {
>  *     int val;
>  *     TreeNode *left;
>  *     TreeNode *right;
>  *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
>  *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
>  *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
>  * };
>  */
> ```
>
> 观察上述代码，这是我从一道leetocde题目给的代码复制下来的，这是一个结构体的定义，语言是c++，对于这种块注释我们把它放在visual studio 2022 中，使用取消注释时，这些*还会保留，有时候一个个清除过于麻烦。所以，我们可以利用python的re模块实现对这种讨厌注释块的过滤，保留像结构体这种有意义的信息。



## 知识点

基本上和上次差不多，这里再简单回顾一下。

### re

re模块主要是python 中集成正则表达式的模块，功能主要是字符串的匹配。

这里用到了三个re函数

re.complie(): 生成正则表达式对象

re.sub()：将指定内容替换

re.search(): 查找函数，在字符串中查找第一个符合正则表达式对象的子串。



### 文件

除了re外，还使用到文件的一些操作，之前我们对于清空文件内容采取的方法是使用先以只读模式读取内容，然后关闭文件，再以写的方式打开，由于只写的方式会自动将内容清空的特性，自动实现该特点后再将内容写入。这次对于清空，我们采取一个新的操作。

我们可以以 r+ 模式打开文件，并在读取完后，使用truncate（）函数实现对文件内容清空。



## 核心代码

正则表达式对象，第一行是块注释的一些特征

```python
pattern = re.compile(r'/\*{0,2}| \* | \*/')
white = re.compile(r'\S')
```



识别开头的/* 或者*并替换，以及跳过多余的空白行。

```python
# 由于只针对开头，只能使用一次匹配
line = pattern.sub("", line, 1)
# 忽略空白行
if white.search(line):
    ans += line + "\n"
```



## 操作流程

我们先将内容用记事本保存。

![L3y2ty.png](https://s6.jpg.cm/2022/03/22/L3y2ty.png)



然后在pycharm 中运行我们程序，当然要先传入文件地址。

然后再打开文件，我们就会发现文件内容方式了更改。

![L39ck8.png](https://s6.jpg.cm/2022/03/22/L39ck8.png)

这里不知道什么原因，居然把那个Defintion去掉了，看了代码也没懂，有机会再研究一下。

其实是再第一行末位了哈哈哈。



## code（Python）完整

```python
import re
# 去除块注释
def rem_block_ann(filepath):
    """
    该函数用于去除讨厌的块注释
    :param filepath: 文件路径，txt文件
    :return:
    """
    pattern = re.compile(r'/\*{0,2}| \* | \*/')
    white = re.compile(r'\S')
    # 打开文件
    file = open(path, 'r+')
    # 先将内容分行存入列表lines
    lines = file.read().split('\n')
    file.truncate(0)
    ans = ""
    for line in lines:
        # 由于只针对开头，只能使用一次匹配
        line = pattern.sub("", line, 1)
        # 忽略空白行
        if white.search(line):
            ans += line + "\n"
    print(ans, file=file)
    file.close()
    
path = "F:\\刷\\leetcode\\22年3月\\blog\\testdemo\\226structcode.txt"
rem_block_ann(path)

```



> 不摆了。