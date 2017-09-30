---
layout: post
title: "Find the maximum subarray"
author: Dervean
description: "one divide-and-conqure method"
categories: [algorithm]
tags: [algorithm,python]
redirect_from:
  - /2017/09/30/
---

求解最大子数组问题.

**背景**：股市涨跌，求出某段时间T，使满足在时段T内盈利最大，并求出该时段盈利P.

每天的盈亏可以看作一个数字d(正嬴负亏)，即求出子数组使其加和最大即可.

---

## 最大子数组

假设:
- 盈亏数组为num_array
- 下标最小为low
- 下标最大为high

我们把num_array一分为二(left,right)，问题答案只能有三种：
- 解在left
- 解在right
- 解横跨left,right

前两种情况可以递归处理，第三种情况处理起来也很简单(时间复杂度为O(n)).
如果解横跨中点(mid)，那么对于left子数组来说，即找到最大子数组num_array[i,...,mid]即可，因为右侧终点已经被固定，自右而左可以很快找到解，同理可得right子数组中的解，合并即可.

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