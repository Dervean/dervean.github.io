---
layout: post
title: "Insertion-sort, quick-sort and heap-sort"
author: Dervean
description: "some sort method"
categories: [algorithm]
tags: [algorithm,python]
redirect_from:
  - /2017/09/30/
---

算法整理

---

## 插入排序

自小到大排列.

~~~ python
def insertion_sort(num_array):
    '''
    :param num_array: to-be sorted array,which only contains numbers
    :return:
    '''
    for i in range(1,len(num_array)):
        key = num_array[i]
        j = i - 1
        while(j>=0 and num_array[j]>key):
            num_array[j+1] = num_array[j]
            j -= 1
        num_array[j+1] = key

if __name__=='__main__':
    array = [2,3,4,1,2,3,2,5,3,6,8,5,6,7,8,96,4,7,3,4]
    insertion_sort(array)
    print(array)
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