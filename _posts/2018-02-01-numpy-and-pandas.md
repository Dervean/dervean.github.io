---
layout: post
title: "numpy and pandas"
author: Dervean
description: "scientific computational tools"
categories: [python]
tags: [python]
redirect_from:
  - /2018/02/01/
---

科学计算工具 numpy 和 pandas 的基本操作。

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

### arange，reshape

~~~ python
import numpy as np

# 步长固定行矩阵，0到20，步长为2，共10个元素
array = np.arange(0, 20, 2)
# reshape矩阵，以上10个元素变成2行5列的新矩阵
array = array.reshape((2, 5))
~~~

### inspace

~~~ python
import numpy as np

# 生成固定的起始元素（0）、最终元素（10）以及元素个数（8）的行矩阵
array = np.linspace(0, 10, 8)
~~~













