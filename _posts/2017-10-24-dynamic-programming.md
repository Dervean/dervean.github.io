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
2. 子问题重叠
 * 子问题重叠意味着子问题空间足够小，问题的递归会反复求解相同的子问题，而分治算法则会不断产生新的子问题。

---

## 切钢管

问题描述：一条长度为n的钢管，不同长度的钢管售价不同，如何切割钢管使售价最高。

如果使用自顶向下递归求解，会同简单求解斐波纳切问题一样陷入求解时间复杂度指数型增长的困境，这里使用动态规划可以有两种方式：

1. 带备忘的自顶向下递归法（top down with memoization）
 * 简单来说，就是利用一个数组保存求解过的子问题的解。
 * 这里保存**长度为i的钢管的最高价格**，保存在**数组r**中，数组r共**n+1**项，其中只使用后n项。
2. 自底向上法（bottom-up method）
 * 自底向上法不需要使用递归栈，避免了递归开销，虽然时间复杂度都为O(n^2)，但其系数更小。
 * 可以保存一个**辅助数组s**用来表示**在长度为i的钢管上切割下的第一块钢管的长度**，因为问题具有最优子结构的性质，所以只需要保存第一块的长度即可。

### 带备忘的自顶向下递归法

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

### 自底向上法

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

## 快速排序

每次可以确定一个元素的位置，注意partition函数中i，j的含义，位置于[0,i]的元素小于pivot.

~~~ python
def quich_sort(num_array,low,high):
    '''
    page 170 in Intro_to_algorithm
    :param num_array: to-be sorted array,which only contains numbers
    :param low: subscript of num_array
    :param high: subscript of num_array
    :return:
    '''
    if low >= high:
        return
    mid = partition(num_array,low,high)
    quich_sort(num_array,low,mid - 1)
    quich_sort(num_array,mid + 1,high)

def partition(num_array,low,high):
    '''
    ensure one item to be placed where it should be
    :param num_array: to-be sorted array,which only contains numbers
    :param low: subscript of num_array
    :param high: subscript of num_array
    :return:
    '''
    pivot = num_array[high]
    # the elements(subscripts less or equal than i) are smaller than pivot
    i = low - 1
    # the elements(subscripts that larger than i but smaller or equal than j) are bigger than pivot
    for j in range(low,high):
        if num_array[j] < pivot:
            i += 1
            temp = num_array[i]
            num_array[i] = num_array[j]
            num_array[j] = temp
    num_array[high] = num_array[i + 1]
    num_array[i+1] = pivot
    return (i + 1)

if __name__=='__main__':
    array = [3,2,1,4,12,9,2]
    quich_sort(array,0,len(array)-1)
    print(array)
~~~

## 堆排序

实现的是最大堆.

主要函数有：

- max_heapipy(heap,i) ： 将位于i位置的元素"下沉"(越小越"沉").

- build_max_heap(heap) : 建最大堆，逐次调用**max_heapipy**函数(从堆的最后一个双亲节点开始).

- heap_sort(heap) ： 堆排序，每次交换堆顶元素与堆底元素，数组长度减一, 并对新的堆顶元素执行**max_heapipy**函数使其归位.

~~~ python
import math

def left_child(i):
    '''
    :param i: the root node of sub-heap
    :return: left child node id
    '''
    return i*2 + 1

def right_child(i):
    '''
    :param i: the root node of sub-heap
    :return: right child node id
    '''
    return i*2+2

def parent(i):
    '''
    :param i: the child node of sub-heap
    :return: parent node id
    '''
    return math.floor((i-1)/2)

class heap(object):
    '''
    initialize a heap with an array of numbers
    '''
    def __init__(self,num_array):
        self.numarray = num_array
        self.heap_size = len(num_array)
    def size(self):
        return self.heap_size
    def array(self):
        return self.numarray
    def set_size(self,new_size):
        self.heap_size = new_size

def heap_sort(heap):
    '''
    page 151 in Intro_to_algorithm
    MAX_HEAP
    :param heap: to-be sorted
    :return:
    '''
    build_max_heap(heap)
    for i in range(0,heap.size())[::-1]:
        temp = heap.array()[0]
        heap.array()[0] = heap.array()[heap.size() - 1]
        heap.array()[heap.size() - 1] = temp
        heap.set_size(heap.size()-1)
        max_heapipy(heap,0)

def max_heapipy(heap,i):
    '''
    bubble from top to low
    :param heap: to-be sorted heap
    :param i: the root node of sub-heap
    :return:
    '''
    left = left_child(i)
    right = right_child(i)
    largest = i
    if left < heap.size() and heap.array()[i] < heap.array()[left]:
        largest = left
    if right < heap.size() and heap.array()[right] > heap.array()[largest]:
        largest = right
    if largest!=i:
        temp = heap.array()[largest]
        heap.array()[largest] = heap.array()[i]
        heap.array()[i] = temp
        return max_heapipy(heap,largest)

def build_max_heap(heap):
    '''
    :param heap: to-be sorted heap
    :return:
    '''
    bottom_right_parent = math.floor(heap.size()/2) - 1
    for i in range(0,bottom_right_parent+1)[::-1]:
        max_heapipy(heap,i)

if __name__ == '__main__':
    array = [1,2,3,4,5,6,7]
    heap1 = heap(array)
    heap_sort(heap1)
    print(heap1.array())
~~~