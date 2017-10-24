---
layout: post
title: "Dynamic programming - Cut Rod and Matrix Chain Multiplication"
author: Dervean
description: "two problems solving by dp"
categories: [algorithm,dynamic programming]
tags: [algorithm,python]
redirect_from:
  - /2017/10/24/
---

动态规划

问题需要满足以下两个条件：

1. 最优子结构
 * 如果一个问题的最优解包含子问题的最优解，则称该问题具有最优子结构。
 * 最优子结构要求子问题无关。
 * 作为构成原问题最优解的组成部分，每个子问题的解就是它的最优解。
2. 子问题重叠
 * 子问题重叠意味着子问题空间足够小，问题的递归会反复求解相同的子问题，而分治算法则会不断产生新的子问题。

---

## 切钢管

问题描述：一条长度为n的钢管，不同长度的钢管售价不同，如何切割钢管使售价最高。

如果使用自顶向下递归求解，会同简单求解斐波纳切问题一样陷入求解时间复杂度指数型增长的困境，这里使用动态规划可以有两种方式：

1. 带备忘的自顶向下递归法（top down with memoization）
 * 简单来说，就是利用一个数组保存求解过的子问题的解。
 * 这里保存**长度为i的钢管的最高价格**，保存在**数组r**中，数组r共**n+1**项，其中只使用后n项(第一项为0)。
2. 自底向上法（bottom-up method）
 * 自底向上法不需要使用递归栈，避免了递归开销，虽然时间复杂度都为O(n^2)，但其系数更小。
 * 可以保存一个**辅助数组s**用来表示**在长度为i的钢管上切割下的第一块钢管的长度**，因为问题具有最优子结构的性质，所以只需要保存第一块的长度即可。
 * 自底向上法的思想是：每个子问题只需要求解一次，且它的所有前提子子问题已经被求解完成。

 切钢管的时间复杂度：O(n^2)，可以简单理解为n个子问题，n个选择。

#### 带备忘的自顶向下递归法


~~~ python
def cut_rod_R(price_array,n):
    '''
    :param price_array: the price array of rod
    :param n: the length of rod
    :return: store the max cost of given rod length
    '''
    # 长度为i的钢管最高卖价
    r = [None for i in range(0,n+1)]
    cut_rod_R_partition(price_array,n,r)
    return r

def cut_rod_R_partition(price_array,n,r):
    '''
    :param price_array: the price array of rod
    :param n: the length of rod
    :param r: store the max cost of given rod length
    :return:
    '''
    # 子问题已经被解得
    if r[n] is not None and r[n] >= 0:
        return r[n]
    # q：长度为n的钢管最高售价
    if n == 0:
        q = 0
    else:
        q = None
        # i：切下来的第一段钢管长度
        for i in range(1,n+1):
            newq = price_array[i] + cut_rod_R_partition(price_array,n-i,r)
            if q is None:
                q = newq
            else:
                if q < newq:
                    q = newq
    r[n] = q
    return q
~~~

#### 自底向上法


~~~ python
def cut_rod_bottom_up(price_array,n):
    '''
    :param price_array: the price array of rod
    :param n: the length of rod
    :return:
    '''
    # 长度为i的钢管最高卖价
    r = [None for i in range(0, n + 1)]
    # 长度为n的钢管，如果想要卖出最高价，切下的第一段钢管长度
    s = [None for i in range(0, n + 1)]
    r[0] = 0
    # j：钢管长度
    for j in range(1,n+1):
        # q：长度为j的钢管最高售价
        q = None
        # i：切下来的第一段钢管长度
        for i in range(1,j+1):
            newq = price_array[i] + r[j-i]
            if q is None or q < newq:
                q = newq
                s[j] = i
            r[j] = q
    return r,s

# 钢管如何切割
def print_solution(price_array,n):
    (r,s) = cut_rod_bottom_up(price_array,n)
    while n > 0:
        print("%d\t"%s[n],end='')
        n = n - s[n]
~~~

## 矩阵链式乘法

问题描述：n个矩阵相乘，A1A2...An，如何添加括号使得矩阵相乘所用的总乘法次数最少。

1. 带备忘的自顶向下递归法（top down with memoization）
 * 这里使用一个**二维数组m**，保存**第i个矩阵到第j个矩阵相乘需要的总次数**，数组m共**（n+1)^2**项。
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
 * 自底向上法不需要使用递归栈，避免了递归开销，虽然时间复杂度都为O(n^2)，但其系数更小。
 * 可以保存一个**辅助数组s**用来表示**在长度为i的钢管上切割下的第一块钢管的长度**，因为问题具有最优子结构的性质，所以只需要保存第一块的长度即可。
 
    |---+---+---+---+---+---|
    | 0 | 0 | 0 | 0 | 0 | 0 |
    |---|:--|--:|--:|--:|--:|
    | 0 | * | * | * | * | * |
    | 0 | 0 | * | * | * | * |
    | 0 | 0 | 0 | * | * | * |
    | 0 | 0 | 0 | 0 | * | * |
    | 0 | 0 | 0 | 0 | 0 | * |
    |---+---+---+---+---+---|

#### 带备忘的自顶向下递归法


~~~ python
def cut_rod_R(price_array,n):
    '''
    :param price_array: the price array of rod
    :param n: the length of rod
    :return: store the max cost of given rod length
    '''
    # 长度为i的钢管最高卖价
    r = [None for i in range(0,n+1)]
    cut_rod_R_partition(price_array,n,r)
    return r

def cut_rod_R_partition(price_array,n,r):
    '''
    :param price_array: the price array of rod
    :param n: the length of rod
    :param r: store the max cost of given rod length
    :return:
    '''
    # 子问题已经被解得
    if r[n] is not None and r[n] >= 0:
        return r[n]
    # q：长度为n的钢管最高售价
    if n == 0:
        q = 0
    else:
        q = None
        # i：切下来的第一段钢管长度
        for i in range(1,n+1):
            newq = price_array[i] + cut_rod_R_partition(price_array,n-i,r)
            if q is None:
                q = newq
            else:
                if q < newq:
                    q = newq
    r[n] = q
    return q
~~~

#### 自底向上法


~~~ python
def cut_rod_bottom_up(price_array,n):
    '''
    :param price_array: the price array of rod
    :param n: the length of rod
    :return:
    '''
    # 长度为i的钢管最高卖价
    r = [None for i in range(0, n + 1)]
    # 长度为n的钢管，如果想要卖出最高价，切下的第一段钢管长度
    s = [None for i in range(0, n + 1)]
    r[0] = 0
    # j：钢管长度
    for j in range(1,n+1):
        # q：长度为j的钢管最高售价
        q = None
        # i：切下来的第一段钢管长度
        for i in range(1,j+1):
            newq = price_array[i] + r[j-i]
            if q is None or q < newq:
                q = newq
                s[j] = i
            r[j] = q
    return r,s

# 钢管如何切割
def print_solution(price_array,n):
    (r,s) = cut_rod_bottom_up(price_array,n)
    while n > 0:
        print("%d\t"%s[n],end='')
        n = n - s[n]
~~~
