---
title: LeetCode 题解-152. Maximum Product Subarray
published: 2019-11-28T16:24:51+08:00
slug: leetcode-problem-152
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 152. Maximum Product Subarray 题目的求解。

## Problem Description

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

## Example 1

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

## Example 2

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

## Solution 1: brute force

计算出每个可能的subarray的product,选择最大的．时间复杂度为O(n^2).需要注意的是只需要开O(n)的数组即可．
```java
class Solution {
    public static int maxProduct(int[] nums) {
        int dp[] = new int [nums.length];
        int max = -10000;
        for (int i = nums.length - 1; i >= 0; --i){
            for (int j = i; j < nums.length; ++j){
                if (j == i) dp[j] = nums[j];
                else dp[j] = dp[j-1] * nums[j];
                max= Math.max(max, dp[j]);
            }
        }
        return max;
    }
}
```

## Solution 2 : optimized

我们想想有没有可能在线性时间内解决问题呢？如果我们使用dp[i]来表示以nums[i]结尾的最大的product呢？然而这样简单的做法是不行的．因为我们的数组中会出现负数．例如[2,-2,-4].最大的乘积是16但是按照简单的dp只能得到8.　深入观察可以发现，我们之所以得不到最大解是因为在中间结果中存在很小的负数再乘以负数变得很大的情况．既然这样，我们就维护两个数组，分别存储最大的数和最小的数．

```java
class Solution {
    public static int maxProduct(int[] nums) {
        if (nums.length == 1) return nums[0];
        int dp_max[] = new int [nums.length+1];
        int dp_min[] = new int [nums.length+1];
        int max = -10000;
        for (int i = 1; i <= nums.length; ++i){
            dp_max[i] = Math.max(Math.max(nums[i-1], dp_max[i-1] * nums[i-1]), dp_min[i-1] * nums[i-1]);
            dp_min[i] = Math.min(Math.min(nums[i-1], dp_max[i-1] * nums[i-1]), dp_min[i-1] * nums[i-1]);
            max=Math.max(max, dp_max[i]);
        }
        return max;
    }
}
```
