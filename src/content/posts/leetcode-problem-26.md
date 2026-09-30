---
title: LeetCode 题解-26. Remove duplicates from sorted array
published: 2019-10-28T16:20:09+08:00
slug: leetcode-problem-26
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 26. Remove duplicates from sorted array 题目的求解。

## Problem Description

Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.

## Example

Example 1
```
Given nums = [1,1,2], Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the returned length.
```
Example 2
```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length
```

## Analysis

这道题目标签是*easy*，但是还是很巧妙的。由于题目要求我们只能使用O(1)的额外空间，所以我们不能新开数组。再由题意可知，我们只需要保证数组前半段的正确性，不用考虑后半段的内容，所以我们可以将数组后半段的空间利用起来。由于数组已经被排好序了，所以我们可以预见数组将会符合这样的格式：`A(+)…B(+)`。一个简单的想法是，对于重复出现的元素，只保留一个，将剩下的用后续元素覆盖掉。

具体实现方法即为快慢指针法。我们让慢指针`p1`指向去掉重复元素序列的最后一个元素，让快指针`p2`指向下一个要加入序列的元素。
1. 初始时`p1`和`p2`都指向起始元素。
2. 如果`p1`和`p2`指向的元素相同，保持`p1`不动，`p2`指向下一个元素。所以`p2`最终会指向大于第一个大于`p1`的元素。
3. 如果`p1`和`p2`指向的元素不同，我们将`p2`处的元素swap到`p1+1`的位置。
## Solution 

```java
class Solution {
        public static int removeDuplicates(int[] nums) {
        int point1 = 0;
        int point2 = 0;
        int tmp;
        // find the number point2 greater than point1,
        // and put it at point1+1
        if (nums.length <= 1){
            return nums.length;
        }
        while (point2 < nums.length){
            if (nums[point1] == nums[point2]){
                point2 += 1;
            }
            else {
                point1 += 1;
                point2 += 1;
                //swap point1 with point2-1
                tmp = nums[point1];
                nums[point1] = nums[point2-1];
                nums[point2-1] = tmp;
            }
        }
        return point1 + 1;
    }
}
```

## Another Solution
下面这种带有loop的算法起始是快慢指针的一种变形了，其本质是相同的。

```java
class Solution {
    public static boolean removeDuplicates(String s) {
        if (nums.length <= 1){
            return nums.length;
        }
        int cur = 1;
        for (int i = 1; i < nums.length; ++i){
            if (nums[i] > nums[cur]){
                nums[cur++] = nums[i];
            }
        }
        return cur;
    }
}
```
