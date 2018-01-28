---
layout: post
title: "Dynamic topic model"
author: Dervean
description: "apply dtm on topic analysis"
categories: [dtm,python]
tags: [dtm,python]
redirect_from:
  - /2017/11/16/
---

这一段时间一直在用到 dynamic topic model(DTM)，也遇到很多坑，有一些坑需要标记一下以防以后再陷进去。

[维基传送门](https://en.wikipedia.org/wiki/Dynamic_topic_model)

[D. Blei and J. Lafferty. Dynamic topic models. In Proceedings of the 23rd International Conference on Machine Learning, 2006.](http://www.cs.columbia.edu/~blei/papers/BleiLafferty2006a.pdf)

[S. Gerrish and D. Blei. A Language-based Approach to Measuring Scholarly Impact. In Proceedings of the 27th International Conference on Machine Learning, 2010.](http://www.cs.columbia.edu/~blei/papers/GerrishBlei2010.pdf)

dynamic topic model，顾名思义，是一个进行主题分析的工具，应用于文本的主题动态分析，由David Blei and John Lafferty 提出，可以看成是文档主题生成模型（LDA,Latent Dirichlet Allocation）的进阶版。

[github](https://github.com/blei-lab/dtm)

---



## Python

### 递归遍历目录下所有文件

python版本：3

参考[Python递归遍历目录下所有文件](http://www.cnblogs.com/dreamer-fish/p/3820625.html)

可以选择使用os.listdir和os.walk两种方法，值得注意的是这两个方法遍历目录下的文件的时候并不是根据文件名顺序遍历，所以即使目录下的所有文件是根据文件名排列有序的

~~~python

~~~


## Linux

### 统计文件夹下文件、目录个数

参考[Linux下统计当前文件夹下的文件个数、目录个数](http://www.jb51.net/article/56474.htm)

1. 统计当前文件夹下文件的个数:

~~~~~~~~~~~~
~~~~~~~
ls -l |grep "^-"|wc -l
~~~~~~~~
~~~~~~~~~~~~

2. 统计当前文件夹下目录的个数

~~~~~~~~~~~~
~~~~~~~
ls -l |grep "^d"|wc -l
~~~~~~~~
~~~~~~~~~~~~

3. 统计当前文件夹下文件的个数，包括子文件夹里的

~~~~~~~~~~~~
~~~~~~~
ls -lR |grep "^-"|wc -l
~~~~~~~~
~~~~~~~~~~~~

4. 统计文件夹下目录的个数，包括子文件夹里的

~~~~~~~~~~~~
~~~~~~~
ls -lR |grep "^-d"|wc -l
~~~~~~~~
~~~~~~~~~~~~

## 矩阵链式乘法

问题描述：n个矩阵相乘，A1A2...An，如何添加括号使得矩阵相乘所用的总乘法次数最少。

1. 带备忘的自顶向下递归法（top down with memoization）
 * AiA2...Aj 可以递归求解：(Ai...Ak)(Ak+1...Aj)，问题归为求解 **Ai...Ak的乘法次数** 加上 **Ak+1...Aj的乘法次数** 加上 **两个结果矩阵的乘法次数**。
 * 这里使用一个**二维数组m**，保存**第i个矩阵到第j个矩阵相乘需要的总次数**，数组m共**（n+1)^2**项，因为矩阵下标递增，所以i一定小于j，意味着我们只使用二维数组的不到一半的空间。
 * 如下表，矩阵个数为5，则需要6✖️6个二维空间，其中标星号是指需要使用的空间。

    |---+---+---+---+---+---|
    | 0 | 0 | 0 | 0 | 0 | 0 |
    |---|:--|--:|--:|--:|--:|
    | 0 | * | * | * | * | * |
    | 0 | 0 | * | * | * | * |
    | 0 | 0 | 0 | * | * | * |
    | 0 | 0 | 0 | 0 | * | * |
    | 0 | 0 | 0 | 0 | 0 | * |
    |---+---+---+---+---+---|

2. 自底向上法（bottom-up method）
 * 额外保存一个**辅助二维数组s**用来表示**对于第i个矩阵到第j个矩阵相乘的分割点k**，数组m共**（n+1)^2**项，对于n个矩阵，只存在(n-1)个分割点。
 * 如下表，矩阵个数为5，则需要6✖️6个二维空间，其中标星号是指需要使用的空间。

    |---+---+---+---+---+---|
    | 0 | 0 | 0 | 0 | 0 | 0 |
    |---|:--|--:|--:|--:|--:|
    | 0 | 0 | * | * | * | * |
    | 0 | 0 | 0 | * | * | * |
    | 0 | 0 | 0 | 0 | * | * |
    | 0 | 0 | 0 | 0 | 0 | * |
    | 0 | 0 | 0 | 0 | 0 | 0 |
    |---+---+---+---+---+---|

#### 带备忘的自顶向下递归法


~~~ python
import math
def matrix_chain_R(size_matrix):
    '''
    :param size_matrix: the size of matrix set, like: 1,3,5,10 --> (1,3),(3,5),(5,10)
    :return:
    '''
    # 矩阵的个数
    n_matrix = len(size_matrix) - 1
    # m 是一个(n+1)^2的二维数组，只使用其中的部分空间，存储从第i个矩阵到第j个矩阵相乘所需要的最少乘法次数
    m = [[] for i in range(0,n_matrix + 1)]
    for i in range(0,n_matrix+1):
        m[i] = [math.inf for j in range(0,n_matrix+1)]
    # 调用lookup_chain函数，其实是用来计算m中的各项值，虽然我们只需要m[1][n]的值
    # m[1][n]就是从第1个矩阵到第n个矩阵相乘所需要的最少乘法次数
    lookup_chain(size_matrix,m,1,n_matrix)
    return m

# 查找（计算）从第i个矩阵到第j个矩阵相乘所需要的最少乘法次数
def lookup_chain(size_matrix,m,i,j):
    if m[i][j] is not math.inf:
        return m[i][j]
    # 如果只有一个矩阵
    if i == j:
        m[i][j] = 0
    else:
        for k in range(i,j):
            # AiA2...Aj 可以递归求解：(Ai...Ak)(Ak+1...Aj)，问题归为求解 Ai...Ak的乘法次数 加上 Ak+1...Aj的乘法次数 加上 两个结果矩阵的乘法次数
            q = lookup_chain(size_matrix,m,i,k) + lookup_chain(size_matrix,m,k+1,j) + size_matrix[i-1]*size_matrix[k]*size_matrix[j]
            if q < m[i][j]:
                m[i][j] = q
    return m[i][j]
~~~

#### 自底向上法


~~~ python
def mcm(size_matrix):
    '''
    :param size_matrix: the size of matrix set, like: 1,3,5,10 --> (1,3),(3,5),(5,10)
    :return:
    '''
    # number of matrix, index of matrix chain begins from 1
    n_matrix = len(size_matrix) - 1
    # optimal cost array (only use index from 1)
    m = [[] for i in range(n_matrix + 1)]
    for i in range(0,n_matrix + 1):
        m[i] = [0 for j in range(n_matrix + 1)]
    # records the index (only use index from 1)
    s = [[] for i in range(n_matrix + 1)]
    for i in range(0, n_matrix + 1):
        s[i] = [0 for j in range(n_matrix + 1)]
    # l: the length of sub chain
    for l in range(2,n_matrix + 1):
        # i: the begin index of sub chain
        for i in range(1,n_matrix-l+2):
            # j: the end index of sub chain
            j = l + i - 1
            # useful index of m start from 1
            m[i][j] = math.inf
            for k in range(i,j):
                # calculate the cost
                c = m[i][k] + m[k+1][j]+size_matrix[i - 1]*size_matrix[k]*size_matrix[j]
                if c < m[i][j]:
                    m[i][j] = c
                    s[i][j] = k
    return (m,s)

# 打印分割结果
def print_solution(s,i,j):
    if i == j:
        print("A"+str(i),end='')
    else:
        print("(",end='')
        print_solution(s,i,s[i][j])
        print_solution(s,s[i][j]+1,j)
        print(")",end='')
~~~
