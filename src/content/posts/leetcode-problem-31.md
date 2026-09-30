---
title: LeetCode 题解-31. Next Permutation
published: 2019-12-05T13:59:11+08:00
slug: leetcode-problem-31
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 31. Next Permutation 题目的求解。

## Problem Description

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

## Example 1

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```


## Analysis

对n个元素的集合我们进行排列，叫permutation. 对于排列的集合，我们可以按照字母序(lexicographically)进行排序．而这道题目就要求我们在给定一个排列的情况下，找到它下一个排列．

首先我们需要认清一个排列的规律．
- 字母序排序的第一个排列是n个元素的升序排列，最后一个排列是n个元素的降序排列．
- 所以我们可以把每个排列分成两半，前一半不一定．而后一半一定是降序的，．

那么我们如果得到字典序的下一个排列呢．首先，找出这两个半相交的地方，即index最大的出现顺序的地方．在此之后是降序列．我们要使得排列变得更大，就要排列出更大的降序列，所以要将这个顺序对变成降序．那么要变成怎么样的降序呢？当然是比当前大的排列里最小的．那么，我们就需要找出他之后的元素里比他大的最小的，进行swap,之后将后面的元素进行排序，得到新子集合的最小排列．


## Solution 1

```java
class Solution {
    //1. first find the smallest ascending order(wrong)
    //2, we should find adjacent elements in ascending order.
    for (int i = nums.length-1;  i >= 1; --i){
        if (nums[i] > nums[i-1]){
            for (int j = nums.length - 1; j >= i; --j){
                if (nums[j] > nums[i-1]){
                    int tmp = nums[i-1];
                    nums[i-1] = nums[j];
                    nums[j] = tmp;
                    Arrays.sort(nums, i, nums.length);
                    return ;
                }
            }
        }
    }
    int tmp = 0;
    for (int i = 0; i < nums.length/2; ++i){
        tmp = nums[i];
        nums[i] = nums[nums.length - i - 1];
        nums[nums.length - i - 1] = tmp;
    }
}
```