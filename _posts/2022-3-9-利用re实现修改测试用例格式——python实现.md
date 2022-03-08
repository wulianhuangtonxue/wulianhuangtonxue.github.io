---
layout: post
title: 利用re实现修改测试用例格式
subtitle:  python实现
date:   2022-3-9
author:  物联黄同学
header-img: img/blogimg/20220309.jpg
catalog:   true
tags:
    - python开发
    - 测试用例
    - 正则表达式
---



# 利用re实现修改测试用例格式——python实现

## 前言

> 还记得我上一篇关于leetcode的那道蜡烛之间盘子计数的blog嘛，不知道的可以去了解一下，附上blog地址。
>
> [Leetcode每日一题（2022.3.8） - 物联黄同学的博客 | HI Blog (wulianhuangtonxue.github.io)](https://wulianhuangtonxue.github.io/2022/03/08/Leetcode每日一题-2022.3.8-2005-蜡烛之间的盘子-前缀和/)
>
> [(2条消息) Leetcode每日一题（2022.3.8）—— 2005 蜡烛之间的盘子（前缀和）_物联黄同学的博客-CSDN博客](https://blog.csdn.net/weixin_54891898/article/details/123354539)
>
> 而我平时会将这些题目录下来，放到学校的oj服务器上，在这个的过程中，除了需要复制题目内容，编写完整的code外，还要录入一些测试用例，这些测试用例可以自己测试，我曾经写过一篇关于使用洗牌算法生成测试用例的方法，有兴趣的也可以去看看。
>
> [(2条消息) 随机洗牌算法构造测试用例_物联黄同学的博客-CSDN博客](https://blog.csdn.net/weixin_54891898/article/details/122157095)
>
> [使用随机洗牌算法构造测试用例 - 物联黄同学的博客 | HI Blog (wulianhuangtonxue.github.io)](https://wulianhuangtonxue.github.io/2022/01/01/使用随机洗牌算法构造测试用例/)
>
> 除了自己设计测试用例，leetcode也会提供题目的一些用例，首先是题目本身就会显示的几个样例，然后在你提交代码不通过时，也会返回一些用例，我们可以直接使用这些用例，相对来说会比自己设计要方便很多。但是，leetcode提供的测试用例，往往会有一些格式的问题，这些格式其实可以手动修改，但是我比较懒，拒绝这样做，于是便有了这篇blog。



## 问题简述

我们还是那道蜡烛盘子问题为例。

先看下它的一个测试用例（leetcode原生）

> "***|**|*****|**||**|*"
> [[1,17],[4,5],[14,17],[5,11],[15,16]]

然后我在oj上对这道题的输入和输出要求是这样的

![L0EBIT.png](https://s6.jpg.cm/2022/03/10/L0EBIT.png)



我们当然可以自己修改输入或者输出的要求，但是录题是为了帮助其他同学练习，难道要为难他们要去专门输入像leetcode格式那样的字符串带双引号，数组带方括号？

显然，作为录题人，我觉得应该自己处理这些用例的格式，正好之前包括最近都在做一个使用python的re模块的项目，所以我便想到了，是不是可以编写一个程序实现对测试样例的内容的修改。

当然很多人可能会想这其实可以手动修改格式，去除一些符号，然后换行罢了，没必要专门写一个程序吧？那么，还记得这道题，使用暴力模拟为什么过不了，因为超时，为什么会超时，因为leetcode给了一个非常大的测试用例，可以通过下面的图片感受一下它的庞大与恐怖

![L0EfUE.png](https://s6.jpg.cm/2022/03/10/L0EfUE.png)



这只是字符串的部分，数组的更多，看到这个测试用例的时候，我是崩溃的，我只能说leetcode算你狠，难怪模拟过不了。所以，面对这种数量非常庞大的数据，我们手动操作的话可能会直接寄。作为一个热爱编程的程序员，我们的任务就应该是将化繁为简，所以我就写了一个程序demo来实现。



## 方法介绍

### re模块

我在代码中主要使用了re模块，这是正则表达式模块，很多语言都有该模块的库，而在python中该模块的库就叫re。

正则表达式是一种用于识别字符串的方法，通过正则表达式，我们可以更加高效地识别检测字符串的内容。

而关于python re库的更多内容，大家可以直接搜索了解，有机会的话我也会整理一下它的知识点。

我们在这个demo中主要使用了下列的三个函数

complie（str）：将str字符串编译成正则表达式对象

match（表达式，str）：对str字符串进行正则表达式匹配，匹配到第一个内容为止，如果匹配不到，则会返回None

对象.sub（str1，str2）: 利用正则表达式对象对str2进行检测，将检测到的部分替换为str1，默认检测到str2的所有。



### os模块和文件操作

除了使用到re模块，我们在该程序中还使用了python的文件操作和OS模块，这里不细写了，只写一个用到的函数。

os.listdir（path）：将path下的文件以列表的形式返回



## 需要的材料

我们可以使用记事本将leetcode上的用例复制粘贴然后保存到相应的路径下，比如下面这样。

![L0Ezxh.png](https://s6.jpg.cm/2022/03/10/L0Ezxh.png)



## code（python）

```python
import re
import os

def ModifyContent(path):
    """
    修改文件的内容
    面向txt文件
    主要事leetcode的测试样例往往有双引号和方括号
    该篇主要用于处理这些变成我们熟悉的样例
    :param path: 文件地址
    :return:
    """
    # 打开文件
    file = open(path, 'r+')
    # 读取文件内容
    content = file.read()
    lines = content.split('\n')

    # 四个正则表达式对象
    # 用于去除引号
    pattern1 = re.compile(r'\"([^"]*)\"')
    # 识别最左边和最右边的两个方括号
    pattern2 = re.compile(r'\[\[|]]')
    # 识别一维数组
    pattern3 = re.compile(r'],\[')
    # 识别一维数组的元素之间的分隔
    pattern4 = re.compile(r',')

    # 匹配字符串内容
    res = pattern1.match(content)
    # 获取字符串内的内容
    s1 = res.group(1)
    # 计算一维数组的数量
    n = len(pattern3.findall(content)) + 1
    s2 = lines[1]
    # 去除最左边或者最右边的两个分括号
    s2 = pattern2.sub('', s2)
    # 将一维数组之间使用换行分隔
    s2 = pattern3.sub('\n', s2)
    # 将元素之间使用空格分隔
    s2 = pattern4.sub(' ', s2)
    # 将内容连接起来
    content = s1 + '\n' + str(n) + '\n' + s2 + '\n'
    print(content)
    # 先关闭文件操作符
    file.close()

    # 再使用w+模式打开，利用该特性将文件原本的内容删除
    file = open(path, 'w+')
    # 将内容输出到文件中，实现对内容的修改
    print(content, file=file)


def batch(path):
    """
    主要是文件路径下的txt文件全部修改为指定内容
    通过循环调用ModifyContent函数即可
    :param path:    路径
    :return:
    """
    # 获取路径下的文件
    files = os.listdir(path)
    for file in files:
        # 文件路径
        filePath = path + "\\" + file
        ModifyContent(filePath)

path = "F:\\刷\\leetcode\\22年3月\\blog\\testdemo"
batch(path)
```



这里的path，朋友们如果需要，需要根据自己的实际情况进行修改。



## 后话

> 欢迎大家找我交流学习，共勉！

