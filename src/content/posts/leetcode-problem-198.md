---
title: LeetCode 题解-198. House Robber
published: 2019-10-24T19:47:04+08:00
slug: leetcode-problem-198
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 198. House Robber 题目的求解。

## Problem Description

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

## Example

Example 1
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```
Example 2
```
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

## Analysis

这道题是一道比较简单的动态规划问题。因为所有的动态规划问题的解决都基于局部最优的推广，在这个问题中，我们想要在不违反约束的情况下拿到n个house最多的现金，所以我们考虑，如果我们已经有前n-1个房子的最优解，这个时候只需要考虑抢不抢第n的房子就行了。根据题目给的约束我们可以得到
\begin{equation}
    dp[n] = Max(dp[n-1], dp[n-2] + a[n])
\end{equation}
其中，dp[n]代表前n个房子最多的钱，a[n]表示第n的房子的现金。在实现的时候，为了方便计算，我们将dp[0],dp[1]均设置为0，dp[n]对应低n-2个房子。
## Solution 

```java
class Solution {
class Solution {
    public static int rob(int[] nums) {
        if (nums.length == 0) return 0;
        if (nums.length == 1) return nums[0];
        int dp[] = new int [nums.length + 1];
        dp[1] = nums[0];
        for (int i = 2; i < nums.length + 1; ++i){
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i-1]);
        }
        return Math.max(dp[nums.length], dp[nums.length - 1]);
    }
}
```

## Optimized Solution
仔细观察发现，空间复杂度还有优化的空间，因为我们用到的数组其实每次迭代只需要2个元素。

```java
class Solution {
    public static int  rob(int [] nums) {
        if (nums.length == 0) return 0;
            if (nums.length == 1) return nums[0];
            int a = 0, b = 0;
            int tmp;
            for (int i = 0; i < nums.length; ++i){
                tmp = Math.max(b, a + nums[i]);
                a = b;
                b = tmp;
            }
            return Math.max(a,b);
    }
}
```
