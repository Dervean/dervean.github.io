---
layout: post
title: "Numpy基本操作"
author: Dervean
description: "scientific computational tools"
categories: [python]
tags: [python]
redirect_from:
  - /2018/02/01/
---

科学计算工具 numpy 的基本操作。

---

* Kramdown table of contents
{:toc .toc}

# Numpy

### 基本属性

~~~ python
import numpy as np

# 创建一个矩阵
array = np.array([
    [1,2,3],
    [4,5,6]
],dtype=np.int)

# 数组维数目
print("dimension: ",array.ndim)

# 形状
print("shape: ",array.shape)

# 总共多少个元素
print("size:",array.size)

# 元素数据格式
print("number type: ",array.dtype)
~~~

### 全0矩阵

~~~ python
import numpy as np

# 全0矩阵，3行4列
array = np.zeros((3, 4))
~~~

### 全1矩阵

~~~ python
import numpy as np

# 全1矩阵，3行4列
array = np.ones((3, 4))
~~~

### arange, reshape

~~~ python
import numpy as np

# 步长固定行矩阵，0到20，步长为2，共10个元素
array = np.arange(0, 20, 2)

# reshape矩阵，以上10个元素变成2行5列的新矩阵
array = array.reshape((2, 5))
~~~

### linspace

~~~ python
import numpy as np

# 生成固定的起始元素（0）、最终元素（10）以及元素个数（8）的行矩阵
array = np.linspace(0, 10, 8)
~~~

### sin函数和判断矩阵中每个元素大小

~~~ python
import numpy as np

array = np.arange(4)

# 应用sin函数
# [ 0.          0.84147098  0.90929743  0.14112001]
array = np.sin(array)

# 返回真值矩阵，判断元素是否大于0.5，每个元素都是boolean类型
# [False  True  True False]
compare = array > 0.5
~~~

### 矩阵点乘

~~~ python
import numpy as np

a = np.array([
    [1,2],
    [3,4]
])

b = np.array([
    [1,0],
    [1,1]
])

# 矩阵点乘
array = a.dot(b)

# 矩阵元素对应相乘
array = a * b
~~~

### random

~~~ python
import numpy as np

# 随机生成2行4列的矩阵，每个元素[0,1]
array = np.random.random((2,4))
~~~


### max, argmax, min, argmin, sum, median, mean, axis取0为列、取1为行

~~~ python
import numpy as np

array = np.array([
    [1,2],
    [3,4]
])

# axis = 1代表对行元素操作；axis = 0代表对列元素操作

# [2 4] 表示：第一行最大为2，第二行最大为4
print(array.max(axis=1))

# 返回值最大的元素的索引
# [1 1] 表示：第一行的第2个，第二行的第2个
print(array.argmax(axis=1))

# [1 2] 表示：第一列最小为1，第二列最小为2
print(array.min(axis=0))

# 返回值最小的元素的索引
# [0 0] 表示：第一列的第1个，第二列的第1个
print(array.argmin(axis=0))

# 10 表示：所有元素之和为10
print(array.sum(axis=None))

# [ 1.5  3.5] 表示：中位数 第一行的为1.5，第二行的为3.5
print(np.median(array,axis=1))

# [ 1.5  3.5] 表示：平均值 第一行的为1.5，第二行的为3.5
print(array.mean(axis=1))
~~~

### cumsum

~~~ python
import numpy as np

array = np.array([
    [1,2],
    [3,4]
])

# 返回累计之和
"""
[
    [1 3]
    [3 7]
]
"""
print(array.cumsum(axis=1))

"""
[
    [1 2]
    [4 6]
]
"""
print(array.cumsum(axis=0))
~~~

### diff

~~~ python
import numpy as np

array = np.array([
    [1, 2, 3],
    [3, 4, 5],
    [4, 5, 6]
])

# 返回相邻两个元素之差
"""
[
    [1 1]
    [1 1]
    [1 1]
]
"""
print(np.diff(array))
~~~

### nonzero

~~~ python
import numpy as np

array = np.array([
    [1, 0],
    [0, 1]
])

# 返回非零元素的下标，分别返回两个数组，第一个数组是非零元素的行标，第二个数组是非零元素的列标，一一对应看即可
(row_index, column_index) = array.nonzero()

# [0 1] 表示第一行、第二行
print(row_index)

# [0 1] 表示第一列、第二列
print(column_index)
~~~

### transpose

~~~ python
import numpy as np

array = np.array([
    [1,2],
    [3,4]
])

# 转置矩阵
"""
[
    [1 3]
    [2 4]
]
"""
print(array.transpose())
~~~

### clip

~~~ python
import numpy as np

array = np.array([
    [1, 3, 10],
    [2, 4, 8]
])

"""
[
    [3 3 6]
    [3 4 6]
]
"""
# 让所有小于3的元素等于3；让所有大于6的元素等于6
print(array.clip(3,6))
~~~

### 索引，取一行，取一列

~~~ python
import numpy as np

array = np.array([
    [1, 3, 10],
    [2, 4, 8]
])

# 8 第二行第三列
print(array[1][2])

# 8 第二行第三列
print(array[1, 2])

# [3 4]
print(array[:, 1])

# [2 4 8]
print(array[1, :])

# [4 8]
print(array[1, 1:3])
~~~

### 迭代

~~~ python
import numpy as np

array = np.array([
    [1, 3, 10],
    [2, 4, 8]
])

# 迭代每一行
for row in array:
    print(row)

# 迭代每一列
for column in array.transpose():
    print(column)

# 迭代每一个元素
for item in array.flat:
    print(item)
~~~

### 合并：vstack, hstack, concatenate

~~~ python
import numpy as np

a = np.array([1, 1, 1])
b = np.array([2, 2, 2])

# vstack : vertical stack
"""
[
    [1 1 1]
    [2 2 2]
    [1 1 1]
]
"""
print(np.vstack((a,b,a)))

# hstack : horizontal stack
"""
[1 1 1 2 2 2 1 1 1]
"""
print(np.hstack((a,b,a)))

"""
[1 1 1 2 2 2 1 1 1]
"""
print(np.concatenate((a,b,a),axis=0))

"""
[
    [1 2 1]
    [1 2 1]
    [1 2 1]
]
"""
print(np.concatenate((a.reshape(3,1),b.reshape(3,1),a.reshape(3,1)),axis=1))
~~~ 

### 分割：vsplit, hsplit, array_split

~~~ python
import numpy as np

array = np.array([
    [1, 3, 10],
    [2, 4, 8]
])

# vsplit, hsplit可以对矩阵进行行分、列分，但只能进行相等分割（即每个子部分都应该size相同）
vparts = np.vsplit(array,2)
hparts = np.hsplit(array,3)

# array_split可以对矩阵进行不等分割
(hpart1, hpart2) = np.array_split(array,2,axis=1)
"""
[[1 3]
 [2 4]]
"""
print(hpart1)
"""
[[10]
 [ 8]]
"""
print(hpart2)
~~~

### copy

~~~ python
import numpy as np

array = np.array([
    [1, 2],
    [3, 4]
])

# 深复制
array_temp = array.copy()
~~~













