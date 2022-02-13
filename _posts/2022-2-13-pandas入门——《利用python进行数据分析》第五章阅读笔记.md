---
layout:     post   				    # 使用的布局（不需要改）
title:      pandas 入门				# 标题
subtitle:   《利用python进行数据分析》第五章 阅读笔记 #副标题
date:       2022-2-13 				# 时间
author:     物联黄同学 						# 作者
header-img: img/20220212.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - python
    - pandas
    - 数据分析
    - 阅读笔记
---
# pandas入门——《利用python进行数据分析》第五章 阅读笔记

## 前言

> 来了来了，摆了几天，终于开始更新第五章了。学它！
>
> 这次带来的是pandas，也就是第五章
>
> 环境：pycharm2020， anconda3（别问我为什么pycharm不更新到2021，孩子家里没装宽带，流量太贵了555）
>
> pandas 采用了很多NumPy的代码风格，但和NumPy最大的不同之处在于 pandas 是用来处理表格型或异质性数据的。而NumPy则相反，更适合处理同质型的数值类型数组数据。



## 一 pandas 数据结构介绍

> 入门pandas需要熟悉的两个常用的工具数据结构： Series 和 DataFrame。

### 1. Series

> Series 是一种一维的数组型对象，包含了一个值序列（与NumPy中的类型相似），并且包含了数据标签，即索引。
>
> 最简单的Series可以是用一个数组创建，也可以在创建时使用标签作为索引，如果没有则会自动使用0到N-1作为标签索引，N是数组长度。
>
> 与NumPy相比，可以从数据中选择数据时使用标签进行索引
>
> 基本可以对其使用Numpy函数或者NumPy风格的操作
>
> 从另一个角度看 Series 其实是一个长度固定且有序的字典
>
> isnull 和 notnull 是两个用来检测数据是否缺失的函数，也是Series实例化的方法
>
> Series 对象自身的和其索引的 name 属性
>
> 索引可以通过按位置赋值所改变

```python
import pandas as pd
import numpy as np
from pandas import Series,DataFrame

# 最简单的序列
obj = pd.Series([4, 7, -5, 3])
print(obj)
print(obj.values)
print(obj.index)

print("---------------------------------")
# 自己定义索引
obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
print(obj2)
print(obj2.index)
print(obj2['a'])
obj2['d'] = 6
print(obj2[['c', 'a', 'd']])

print("---------------------------------")
# 和Numpy一致
print(obj2[obj2 > 0])
print(obj2 * 2)
print(np.exp(obj2))
print('b' in obj2)
print("---------------------------------")

# 使用 字典 直接生成 Seires
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = pd.Series(sdata)
print(obj3)
print("---------------------------------")
# 使用一个列表对Series的索引进行排序，按照列表的顺序
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = pd.Series(sdata, index=states)
# 用于检查缺失数据， 缺失——True
print(pd.isnull(obj4))
# 缺失——False
print(pd.notnull(obj4))
# 数据对齐特性
print(obj3 + obj4)
print("---------------------------------")
# Series 对象自身的和其索引的 name 属性
obj4.name = 'population'
obj4.index.name = 'state'
print(obj4)

# 所引可以通过按位置赋值所改变
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
print(obj)
```



### 2. DataFrame

> DataFrame 表示的是 矩阵的数据表，它包含已排序的列集合，每一列可以是不同的值类型（数值、字符串、布尔值等）。DataFrame 既有行索引也有列索引，它可以被视为一个共享相同索引的Series字典。在DataFrame 中，数据被存储为一个以上的二维块，而不是列表，字典或其他一维数组的集合。
>
> 有多种方式构建DataFrame， 最常用的是 利用等长度列表或者NumPy数组的字典来形成
>
> head方法会选出头五行
>
> 创建时可以指定列的顺序，如果传入的列不在字典中，则会出现缺失值
>
> DataFrame 中的一列可以按字典型标记或属性那样检索为Series
>
> 行也可以是通过位置或特殊属性loc进行选取
>
> 列的引用可修改
>
> 当使用列表或数组赋值给一个列时，值的长度必须和DataFrame 的长度匹配。如果将Series赋值给一列时，Series的索引会按照DataFrame的索引重新排列
>
> del 关键字可以像在字典中那样对DataFrame 删除列
>
> 另一种常用的数据形式是包含字典的嵌套字典
>
> 可以使用类似NumPy语法对DataFrame 进行转置
>
> 包含Series的字典也可以用于构造
>
> 索引和列也拥有name属性
>
> values 属性会将 其数据以二维 ndarray 形式返回
>
> values的 dtype 会自动选择适合所有列的类型

```python
import pandas as pd
import numpy as np
from pandas import Series, DataFrame

data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
# 产生的DataFrame 会自动为 Series 分配索引，并且列会按照排序的顺序排列
frame = pd.DataFrame(data)
print(frame)
# 对于大型的 DataFrame，head方法会只选出头部的五行
print(frame.head())
print('\n------------------------------------\n')

# 指定列的顺序
print(pd.DataFrame(data, columns=['year', 'state', 'pop']))
# 如果传入的列不在字典中，会出现缺失值
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'],
                   index=['one', 'two', 'three', 'four', 'five', 'six'])
print(frame2)
print(frame2.columns)
# 其中的一列检索为Series
print(frame2['state'])
print(frame2.year)
print('\n------------------------------------\n')

# 行索引
print(frame2.loc['three'])
# 修改列的引用
frame2['debt'] = 16.5
print(frame2)
frame2['debt'] = np.arange(6.)
print(frame2)
print('\n------------------------------------\n')

# 使用Series赋值给DataFrame
val = pd.Series([-1.2, -1.5, 1.7], index=['two', 'four', 'five'])
frame2['debt'] = val
print(frame2)
frame2['eastern'] = frame2.state == 'Ohio'
print(frame2)
# del 方法可以用于移除之前新建的列
del frame2['eastern']
print(frame2.columns)
print('\n------------------------------------\n')

pop = {'Nevada': {2001: 2.4, 2002: 2.9},
       'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
frame3 = pd.DataFrame(pop)
print(frame3)
print(frame3.T)
# 使用包含 Series 的字典构造
pdata = {'Ohio': frame3['Ohio'][:-1],
         'Nevada': frame3['Nevada'][:2]}
print(DataFrame(pdata))
print('\n------------------------------------\n')

# name属性
frame3.index.name = 'year'; frame3.columns.name = 'state'
print(frame3)
# values 属性
print(frame3.values)
# 会自动选择适合所有列的类型
print(frame2.values.dtype)
```

> 更多的关于Dataframe 构造函数的有效输入请参考表 5-1



### 3. 索引对象

> pandas中的索引对象是用于存储轴标签和其他元数据，在构造Series 和 DataFrame 时，你所使用的数组或者序列都可以在内部转换为索引对象。
>
> 索引对象不可以改变
>
> 一些用户并不经常利用索引对象提供的功能
>
> 既类似数组，也像一个固定大小的集合
>
> pandas索引对象 可以包含重复标签

```python
import numpy as np
import pandas as pd

obj = pd.Series(range(3), index=['a', 'b', 'c'])
index = obj.index
print(index[1:])
# index[1] = 'd'    # 索引对象不可以改变
labels = pd.Index(np.arange(3))
print(labels)
obj2 = pd.Series([1.5, -2.5, 0], index=labels)
print(obj2)
print(obj2.index is labels)

print('----------------------')
pop = {'Nevada': {2001: 2.4, 2002: 2.9},
       'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
frame3 = pd.DataFrame(pop)
print(frame3.columns)
print('Ohio' in frame3.columns)
print(2003 in frame3.index)
# 索引对象可以包含重复标签
dup_lables = pd.Index(['foo', 'foo', 'bar', 'bar'])
print(dup_lables)
```

> 更多的索引对象的方法和属性请参考表5-2



## 二 基本功能

> 继续了解Series 和 DataFrame 中数据交互的基础机制

### 1. 重建索引

> 主要用到了reindex，其实对索引重新排列，如果赋新的索引有原来没有的，就取缺失值NaN
>
> 对于时间序列，如果需要插值或者填值，reindex的method参数的选择就可以在索引时完成这一操作
>
> 在DataFrame中，reindex可以改变行、列索引，一个序列时默认重建行索引，列可以用columns关键字指定
>
> reindex 会返回一个新的pandas对象

```python
import numpy as np
import pandas as pd

obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
print(obj)
obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
print(obj2)
print('----------------------')
obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
print(obj3)
print('----------------------')
print(obj3.reindex(range(6), method='ffill'))   # ffill 表示值向前填充
print('----------------------')
frame = pd.DataFrame(np.arange(9).reshape((3, 3)),
                     index=['a', 'c', 'd'],
                     columns=['Ohio', 'Texas', 'California'])
print(frame)
frame2 = frame.reindex(['a', 'b', 'c', 'd'])
print('----------------------')
print(frame2)
states = ['Texas', 'Utah', 'California']
print(frame.reindex(columns=states))
```

> 关于reindex方法的参数请参考表5-3



### 2. 轴向上删除条目

> 主要用 drop方法，其可以返回一个删除指示值的新对象，如果需要操作原对象，需要将inplace置为True
>
> 对于DataFrame可以使用axis = 1或 ‘colums’ 来从列删除值，如果不指定，默认行标签

```python
import numpy as np
import pandas as pd

obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])
print(obj)
new_obj = obj.drop('c')
print(new_obj)
print('----------------------')
print(obj.drop(['d', 'c']))
print('----------------------')
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                    index=['Ohio', 'Colorado', 'Utah', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
print(data)
print('----------------------')
print(data.drop(['Colorado', 'Ohio']))
print('----------------------')
print(data.drop('two', axis=1))
print('----------------------')
print(data.drop(['two', 'four'], axis='columns'))
print('----------------------')
# 使用inplace 属性
obj.drop('c', inplace=True)
print(obj)
```



### 3. 索引、选择与过滤

> Series 索引 与 NumPy 数组索引的功能类似，只不过 Series 的索引值可以不仅仅是整数。且切片可以包含尾部
>
> 单个值或序列，DataFrame 索引出一个列或多个列，切片或者布尔值索引为行选择， 且不包含尾部。

```python
import numpy as np
import pandas as pd

obj = pd.Series(np.arange(4), index=['a', 'b', 'c', 'd'])
print(obj)
print('----------------------')
print(obj['b'])
print(obj[1])
print(obj[['b', 'a', 'd']])
print('----------------------')
# Series 切片包含尾部
print(obj['b':'c'])
obj['b':'c'] = 5
print('----------------------')
print(obj)
print('----------------------')

data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                    index=['Ohio', 'Colorado', 'Utah', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
print(data)
print('----------------------')
print(data['two'])
print(data[['three', 'one']])
print('----------------------')
# 切片、布尔 为行
print(data[:2])
print(data[data['three'] > 5])
```



#### 3.1 使用loc 和 iloc 选择数据

> 针对DataFrame在行的索引， 有 loc（轴标签）和 iloc 整数标签 以NumPy 风格的语法从DataFrame中选择数组和行和列的子集，同时兼容切片

```python
import numpy as np
import pandas as pd

data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                    index=['Ohio', 'Colorado', 'Utah', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
print(data)
print('----------------------')
print(data.loc['Colorado', ['two', 'three']])
print(data.iloc[2, [3, 0, 1]])
print('----------------------')
print(data.iloc[2])
print(data.iloc[[1, 2], [3, 0, 1]])
print('----------------------')

# 兼容切片
print(data.loc[:'Utah', 'two'])
print('----------------------')
print(data.iloc[:, :3][data.three > 5])
```

> 表 5-4 介绍了 DataFrame 选择数据的一些方法



### 4. 整数索引

> 对pandas 使用整数索引若其带来歧义，这是因为它和python内建数据结构不同，所以数据选择时请尽量使用的标签索引，如果有一个包含整数的数轴时。
>
> 为了更精确的处理，可以使用之前提到的loc 或者 iloc

```python
import numpy as np
import pandas as pd

ser = pd.Series(np.arange(5.))
print(ser)
# print(ser[-1])    会报错
ser2 = pd.Series(np.arange(3.), index=['a', 'b', 'c'])
print(ser2[-1])     # 不会报错
print('----------------------')

# 使用 loc 或者 iloc 更精确地操作 整数索引
print(ser[:1])
print('----------------------')
print(ser.loc[:1])
print('----------------------')
print(ser.iloc[:1])
```



### 5. 算术和数据对齐

> 不同索引的对象之间的算术行为是pandas 的一大重要特性，对象相加时，如果某个索引对不同时，会返回索引对并集，且会在没有交叠的标签位置上补上缺失值，对于DataFrame中，行列都会自动对齐
>
> 如果将行列完全不同的对象相加，结果将全部为空

```python
import numpy as np
import pandas as pd

s1 = pd.Series([7.3, -2.5, 3.4, 1.5], index=['a', 'c', 'd', 'e'])
s2 = pd.Series([-2.1, 3.6, -1.5, 4, 3.1],
               index=['a', 'c', 'e', 'f', 'g'])
print(s1)
print('----------------------')
print(s2)
print('----------------------')
print(s1 + s2)
print('----------------------')
df1 = pd.DataFrame(np.arange(9.).reshape((3, 3)), columns=list('bcd'),
                   index=['Ohio', 'Texas', 'Colorado'])
df2 = pd.DataFrame(np.arange(12.).reshape((4, 3)), columns=list('bde'),
                   index=['Utah', 'Ohio', 'Texas', 'Oregon'])
print(df1)
print('----------------------')
print(df2)
print('----------------------')
print(df1 + df2)
print('----------------------')
df1 = pd.DataFrame({'A': [1, 2]})
df2 = pd.DataFrame({'B': [3, 4]})
print(df1 - df2)
```



#### 5.1 使用填充值的算术方法

> 针对不同的索引对象，可以对缺失值进行填充
>
> 对一个对象使用add方法，将另一个对象和一个 fill_value 作为参数传入

```python
import numpy as np
import pandas as pd

df1 = pd.DataFrame(np.arange(12.).reshape((3, 4)),
                   columns=list('abcd'))
df2 = pd.DataFrame(np.arange(20.).reshape((4, 5)),
                   columns=list('abcde'))
df2.loc[1, 'b'] = np.nan
print(df1)
print('----------------------')
print(df2)
print('----------------------')
print(df1 + df2)
print('----------------------')
# 对一个对象使用add方法，将另一个对象和一个 fill_value 作为参数传入
print(df1.add(df2, fill_value = 0))
print('----------------------')
print(df2.add(df1, fill_value = 0))
print('----------------------')
print(1 / df1)
print('----------------------')
print(df1.rdiv(1))
# 也可以使用reindex 在重建索引时，指定值填充
print('----------------------')
print(df1.reindex(columns = df2.columns, fill_value=0))
```

> 更多算术方法参考5-5， r表示参数反转



#### 5.2 DataFrame 和 Series 间的操作

> 广播机制的应用，默认Series 的所引和DataFrame 的列进行匹配，并广播到各行
>
> 如果一个索引值 不在DataFrame 中，也不在Series 的索引中，则对象会重建索引并形成联合
>
> 如果想要匹配行，广播列，需要使用算术方法，并对其的axis 属性操作

```python
import numpy as np
import pandas as pd

arr = np.arange(12.).reshape((3, 4))
print(arr)
print('----------------------')
print(arr[0])
print('----------------------')
# 减法在每一行都进行了操作，广播机制
print(arr - arr[0])
print('----------------------')

# DataFrame 和 Series 间也类似
frame = pd.DataFrame(np.arange(12.).reshape((4, 3)),
                     columns=list('bde'),
                     index=['Utah', 'Ohio', 'Texas', 'Oregon'])
series = frame.iloc[0]
print(frame)
print('----------------------')
print(series)
print('----------------------')
print(frame - series)
print('----------------------')
# 如果一个索引值 不在DataFrame 中，也不在Series 的索引中，则对象会重建索引并形成联合
series2 = pd.Series(range(3), index=['b', 'e', 'f'])
print(frame + series2)
print('----------------------')
# 如果想要匹配行，广播列，需要使用算术方法，并对其的axis 属性操作
series3 = frame['d']
print(frame)
print('----------------------')
print(series3)
print('----------------------')
print(frame.sub(series3, axis='index'))
print('----------------------')
print(frame.sub(series3, axis=0))
```



### 6. 函数应用和映射

> numpy 的通用函数（逐元素操作）对pandas对象也有效
>
> 对于DataFrame 的 有另一个常用操作将函数逐行或逐列作用，apply方法，默认逐列，传递axis参数为 'columns' 则为逐行。
>
> 传递给apply 函数 并不一定要返回标量值，也可以返回带有多个值的Series
>
> 逐元素的 Python 函数也可以使用，假设根据 DataFrame 中的每个浮点数计算一个格式化字符串，可以使用applymap方法， 使用 applymap 作为函数名是因为Series 有map方法， 可以将一个逐元素的函数应用到Series上

```python
import numpy as np
import pandas as pd

frame = pd.DataFrame(np.random.randn(4,3), columns=list('bde'),
                     index=['Utah', 'Ohio', 'Texas', 'Oregon'])
print(frame)
print('----------------------------')
print(np.abs(frame))
print('----------------------------')

f = lambda x: x.max() - x.min()
print(frame.apply(f))
print('----------------------------')
print(frame.apply(f, axis='columns'))
print('----------------------------')

def f(x):
    return pd.Series([x.min(), x.max()], index=['min', 'max'])

print(frame.apply(f))
print('----------------------------')

format = lambda x: '%.2f' % x

print(frame.applymap(format))
print('----------------------------')
print(frame['e'].map(format))
```



### 7. 排序和排名

> 根据某些准则对数据集进行排序是另一个重要的内建操作。对于按行或列索引进行字典型排序，可以使用sort_index方法， 会返回一个新的、排序好的对象
>
> 在DataFrame中，可以在各个轴上按索引排序， 默认升序
>
> 值排序可以使用sort_values方法
>
> 默认情况下，所有缺失值会被排序至Series 尾部
>
> 对DataFrame排序，可以选用一列或多列作为排序键，需要使用sort_values方法的参数by来接收列名，传递多列时，会按传递的顺序来指定优先级
>
> 排名是指数组从1到有效数据点总数分配名次的操作。Series和DataFrame的rank方法可以用来实现排名，通过将平均排名分配到每个组来打破平级关系， 也可以根据数据的观察顺序进行分配， 默认升序
>
> DataFrame 可以对行或列计算排名

```python
import numpy as np
import pandas as pd

obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
# 对于按行或列索引进行字典型排序，可以使用sort_index方法， 会返回一个新的、排序好的对象
print(obj.sort_index())
print('---------------------------')
# 在DataFrame中，可以在各个轴上按索引排序
frame = pd.DataFrame(np.arange(8).reshape((2, 4)),
                     index=['three', 'one'],
                     columns=['d', 'a', 'b', 'c'])
print(frame.sort_index())
print('---------------------------')
print(frame.sort_index(axis=1))
print('---------------------------')
print(frame.sort_index(axis=1, ascending=False))
print('---------------------------')
obj = pd.Series([4, 7, -3, 2])
# 值排序
print(obj.sort_values())
print('---------------------------')
# 缺失值被排序至尾部
obj = pd.Series([4, np.nan, 7, np.nan, -3, 2])
print(obj.sort_values())
print('---------------------------')

frame = pd.DataFrame({'b': [4, 7, -3, 2], 'a': [0, 1, 0, 1]})
print(frame)
print('---------------------------')
# 可以使用一列或多列作为排序键
print(frame.sort_values(by='b'))
print('---------------------------')
print(frame.sort_values(by=['a', 'b']))
print('---------------------------')

obj = pd.Series([7, -5, 7, 4, 2, 0, 4])
print(obj.rank())
print('---------------------------')
# 根据观察顺序排名
print(obj.rank(method='first'))
print('---------------------------')

# 将值分配给组中的最大排名
print(obj.rank(ascending=False, method='max'))
print('---------------------------')

frame = pd.DataFrame({'b': [4.3, 7, -3, 2], 'a': [0, 1, 0, 1],
                      'c': [-2, 5, 8, -2.5]})
print(frame)
print('---------------------------')
# 以列为轴，其实就是每行进行排序
print(frame.rank(axis='columns'))
```

> 更多排名的平级打破方式请参考表 5-6



### 8. 含有重复标签的轴索引

> 尽管很多pandas函数如reindex需要标签唯一，但对pandas对象并不是强制性的
>
> 索引的is_unique属性可以用来表示标签是否唯一
>
> 含有重复标签的数据选择会比较不一样，根据标签索引多个条目会返回一个序列，单个条目返回标量
>
> 在DataFrame中拓展为行行索引

```python
import numpy as np
import pandas as pd

obj = pd.Series(range(5), index=['a', 'a', 'b', 'b', 'c'])
print(obj)
print('---------------------------')
print(obj.index.is_unique)
print('---------------------------')
print(obj['a'])
print(obj['c'])

# 拓展到DataFrame，进行行行索引
df = pd.DataFrame(np.random.randn(4, 3), index=['a', 'a', 'b', 'b'])
print(df)
print('---------------------------')
print(df.loc['b'])
```



## 三 描述性统计的概述与计算

> pandas对象装配了一个常用数学、统计学方法的集合，其中大部分属于归约或汇总统计的类别，从DataFrame的行或列中抽取一个Series或一个系列值的单个值（如总和或平均值）。与NumPy数组中的类似方法相比，它们内建了处理缺失值的功能，忽视缺失值
>
> sum方法会返回一个包含列上加和的Series，可以使用axis=1或者‘columns’，会将一行上各列的值相加
>
> 除非整个切片上都是NA，否则NA值会被自动排除，可以通过禁用skipna来实现不排除NA值
>
> 关于归约方法的可选参数可以参考表5-7
>
> 一些如idxmax和idxmin，返回的是间接信息，即索引
>
> 除了归约方法，有的方法是积累型方法
>
> 一些既不是归约，也不是积累的方法，如describe，一次性产生多个汇总统计，对于非数值数据，describe会产生另一种汇总统计

```python
import numpy as np
import pandas as pd

df = pd.DataFrame([[1.4, np.nan], [7.1, -4.5],
                   [np.nan, np.nan], [0.75, -1.3]],
                  index=['a', 'b', 'c', 'd'],
                  columns=['one', 'two'])
print(df)
print('---------------------------')
print(df.sum())
print('---------------------------')
print(df.sum(axis='columns'))
print('---------------------------')
print(df.mean(axis='columns', skipna=False))
print('---------------------------')
print(df.idxmax())
print('---------------------------')
print(df.cumsum())
print('---------------------------')
print(df.describe())
print('---------------------------')
obj = pd.Series(['a', 'a', 'b', 'c'] * 4)
print(obj.describe())
```

> 更多描述性统计和汇总统计参考表 5-8

#### 1. 相关性和协方差

> 一些汇总统计，比如相关性和协方差，由多个参数计算。考虑某些使用附加pandas-datareader库从Yahoo！Finance上获取的包含股价和交易量的DataFrame

一般来说，anaconda本身并没有集成改包，所以一般需要先安装

打开anaconda的命令行 即 Anaconda Prompt，然后输入下列命令

```
conda install pandas-datareader
```

> Yahoo! Finace 可能没了，其在2017被Verizon收购。
>
> 噢实际上目前还能用，不过需要更新api，且需要翻墙连外网，但是我还是报了SSL错误
>
> 这里我采用了随机生成，有机会在学一下爬虫
>
> Series 的corr方法计算的事两个Series 中重叠的、非NA的、按索引对齐的值的相关性，cov则是计算协方差
>
> 使用DataFrame 的 corrwith 方法，你可以计算出DataFrame中的行或列与另一个序列或DataFrame的相关性，该方法传入一个Series时，会返回一个含有为每列计算相关性的Series。传入一个DataFrame时，会计算匹配到列名的相关性数值。

```python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.random.randn(5, 4),
                  columns=['AAPL', 'GOOG', 'IBM', 'MSFT'],
                  index=['2016-10-17', '2016-10-18', '2016-10-19', '2016-10-20', '2016-10-21'])
print(df)
print('-------------------')
print(df['MSFT'].corr(df['IBM']))
print(df['MSFT'].cov(df['IBM']))
print(df.MSFT.corr(df.IBM))
print('-------------------')
print(df.corr())
print('-------------------')
print(df.cov())
print('-------------------')
print(df.corrwith(df.IBM))
print('-------------------')
df1 = pd.DataFrame(np.random.randn(5, 4),
                  columns=['AAPL', 'GOOG', 'IBM', 'MSFT'],
                  index=['2016-10-17', '2016-10-18', '2016-10-19', '2016-10-20', '2016-10-21'])
print(df1)
print('-------------------')
print(df.corrwith(df1))
```



### 2. 唯一值、计数和成员属性

> 另一类相关的方法可以从一维Series包含的数值中提取信息。
>
> 唯一值——unique， 并不一定按照排序好的顺序，如果需要，可以使用sort
>
> 计数——value_counts，默认返回的Series会按降序排序，value_counts 也是有效的 pandas 顶层方法，可用于任意数组或序列
>
> isin 执行向量化的成员属性的检查， 与isin 相关的 Index.get_indexer 方法，可以提供一个索引数组，这个索引数组可以将可能非唯一值数组转换为了一个唯一值数组
>
> 关于唯一值、计数和集合成员属性方法可以参考 表5-9
>
> 将pandas.value_counts 传入 DataFrame 的apply 函数可以得到



```python
import pandas as pd
import numpy as np

obj = pd.Series(['c', 'a', 'd', 'a', 'a', 'b', 'b', 'c', 'c'])
uniques = obj.unique()
print(uniques)
print('-------------------')
uniques.sort()
print(uniques)
print('-------------------')
print(obj.value_counts())
print('-------------------')
print(pd.value_counts(obj.values, sort=False))
print('-------------------')
print(obj)
print('-------------------')
mask = obj.isin(['b', 'c'])
print(mask)
print('-------------------')
print(obj[mask])
print('-------------------')
to_match = pd.Series(['c', 'a', 'b', 'b', 'c', 'a'])
unique_vals = pd.Series(['c', 'b', 'a'])
print(pd.Index(unique_vals).get_indexer(to_match))
print('-------------------')

data = pd.DataFrame({'Qu1': [1, 3, 4, 3, 4, 6],
                     'Qu2': [2, 3, 1, 2, 3, 7],
                     'Qu3': [1, 5, 2, 4, 4, 9]})
print(data)
print('-------------------')
# 计数，如果出现 缺失值 填充为 0
result = data.apply(pd.value_counts).fillna(0)
print(result)
```





## 后话

> 效率无限低，麻了，还碰上文件丢失，现在才做完。。。。

