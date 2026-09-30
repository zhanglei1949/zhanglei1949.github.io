---
title: LeetCode 题解-4. Median of Two Sorted Arrays
published: 2019-11-18T13:52:01+08:00
slug: leetcode-problem-4
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 4. Median of Two Sorted Arrays 题目的求解。

## Problem Description

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.


### Example 1

```

nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

### Example 2

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

## Analysis

这道题是leetcode上第一道hard题目，其困难主要在于题意的理解和边界情况的处理上。

### 题意理解

首先，我们要换一种方式来理解中位数。对于中位数，我们有两种理解

- 通过中位数可以将数组分为两半，使得前一半的元素都小于后一半的元素。
- 中位数即为第len/2个元素。

事实上，从两种不同的理解出发，我们可以得到两种不同的解法。基于第一种理解，我们可以不断进行二分查找，找到中间元素.而对于第二种理解，我们可以从两个数组的头部出发，寻找第len/2个元素．我们首先列出第二种理解对应的解法．其中index表示我们要在两个列表联合构成的集合中要找到的第index个元素．

### 边界情况分析

## Solution 1

```java

public static double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if ( (m + n)% 2 == 0){
            int l = helper(nums1, nums2, 0, m-1, 0, n-1, (m+n)/2);
            int r = helper(nums1, nums2, 0, m-1, 0, n-1, (m+n)/2+1);
            //System.out.printf("%d %d", l, r);
            return ((double)l + r)/2;
        }
        else {
            return helper(nums1, nums2, 0, m-1, 0, n-1, (m+n)/2+1);
        }
    }
    public static int helper(int[] nums1, int []nums2, int s1, int e1, int s2, int e2, int index){
        if (s1 > e1){
            return nums2[s2 + index - 1];
        }
        else if (s2 > e2){
            return nums1[s1 + index - 1];
        }
        if (index == 1){
            return Math.min(nums1[s1], nums2[s2]);
        }
        int mid1 = s1 + index / 2 - 1;
        int mid2 = s2 + index / 2 - 1;
        if (mid1 > e1 ) return helper(nums1, nums2, s1, e1, mid2+1, e2, index - index/2);
        else if (mid2 > e2) return helper(nums1, nums2, mid1+1, e1, s2, e2, index - index/2);
        if (nums1[mid1] <= nums2[mid2]){
            return helper(nums1, nums2, mid1+1, e1, s2, e2, index - index/2);
        }
        else {
            return helper(nums1, nums2, s1, e1, mid2+1, e2, index - index/2);
        }
    }

```

## Solution 2
Brute force,相当于merge sort
```java
public static double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int nums3[] = new int [nums1.length + nums2.length];
        int i = 0, j = 0, z = 0;
        for (; i < nums1.length && j < nums2.length;){
            if (nums1[i] <= nums2[j]){
                nums3[z++] = nums1[i++];
            }
            else nums3[z++] = nums2[j++];
        }
        for (;i < nums1.length; ++i){
            nums3[z++] = nums1[i];
        }
        for (;j < nums2.length; ++j){
            nums3[z++] = nums2[j];
        }

        if (nums3.length % 2 == 0){
            return (double)(nums3[nums3.length / 2 - 1] + nums3[nums3.length / 2]) / 2;
        }
        else return (double)nums3[nums3.length / 2];
    }
```

### Solution 3