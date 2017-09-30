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

** insertion sort

~~~ python
import math

def find_maximum_subarray(num_array,low,high):
    '''
    page 71 in Intro_to_algorithm
    :param num_array: to-be sorted array,which only contains numbers
    :param low: subscript of num_array
    :param high: subscript of num_array
    :return:
    '''
    if high == low:
        return(low,high,num_array[low])
    else:
        mid = math.floor((low + high)/2.0)
        (left_low,left_high,left_sum) = find_maximum_subarray(num_array,low,mid)
        (right_low,right_high,right_sum) = find_maximum_subarray(num_array,mid+1,high)
        (cross_low,cross_high,cross_sum) = find_max_cross_subarray(num_array,low,mid,high)
        if left_sum>right_sum and left_sum>cross_sum:
            return (left_low,left_high,left_sum)
        elif right_sum>left_sum and right_sum>cross_sum:
            return (right_low,right_high,right_sum)
        else:
            return (cross_low,cross_high,cross_sum)

def find_max_cross_subarray(num_array,low,mid,high):
    sum = 0
    left_sum = math.inf
    max_left = mid
    for i in range(low,mid+1)[::-1]:
        sum = sum + num_array[i]
        if left_sum == math.inf:
            left_sum = sum
        else:
            if left_sum < sum:
                left_sum = sum
                max_left = i
    sum = 0
    right_sum = math.inf
    max_right = mid + 1
    for i in range(mid+1,high):
        sum = sum + num_array[i]
        if right_sum == math.inf:
            right_sum = sum
        else:
            if right_sum < sum:
                right_sum = sum
                max_right = i
    return (max_left,max_right,left_sum+right_sum)


if __name__=='__main__':
    array = [-1,2,3,-4,-6,2,-3,-1,9,3,2,3,0,-5]
    (low,high,sum) =  find_maximum_subarray(array,0,len(array)-1)
    print(low,high,sum)
~~~