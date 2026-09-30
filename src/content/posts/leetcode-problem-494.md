---
title: LeetCode 题解-494. Target Sum
published: 2019-12-02T14:18:49+08:00
slug: leetcode-problem-494
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 494. Target Sum 题目的求解。

## Problem Description

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

## Example 1

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

## Note

The length of the given array is positive and will not exceed 20.
The sum of elements in the given array will not exceed 1000.
Your output answer is guaranteed to be fitted in a 32-bit integer.

## Analysis

第一反应这道题是搜索题，因为题目很明显地提到了一个target sum 和每个元素的两条分支，所以我们可以写出基于深度有限搜索的solution 1, 时间复杂度为O(2^n).

## Solution 1: bfs with recursive call

```java
class Solution {
    public static int findTargetSumWays(int[] nums, int S) {
        return help(nums, S, 0);
    }
    public static int help(int[] nums, int S, int start){
        if (start > nums.length - 1) return 0;
        if (start == nums.length - 1 && nums[start] == 0 && nums[start] == S) return 2;
        else if (start == nums.length - 1 && nums[start] == S) return 1;
        else if (start == nums.length - 1 && nums[start] == -S) return 1;
        return help(nums, S - nums[start], start + 1) + help(nums, S + nums[start], start + 1);
    }
}
```

## Analysis 2
但事实上，这道题是一道背包问题．对于每个元素我们有两种选项，+或-．
暴力破解的方法复杂度太高，我们考虑更高效的方法．我们首先可以把+,-两个符号换一种角度看待，可以看做将数组分为两半，用其中一般取减去另外一半得到target.

因此
```
sum(p) - sum(n) = S
2sum(p) = S + sum(p&n)
sum(p) = (S + sum(p&n))/2
```

所以我们只需要找到子数组，是的其和为$(S + sum(p&n))/2$即可．
由于题目中已经指出数组中所有元素之和不超过1000,所以我们可以构造dp[1-1000],dp[i]表示有多少种组合可以构成sum为i的子数组．

## Solution 2

```java
class Solution{
    public static int findTargetSumWays(int[] nums, int S){
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum < S) return 0;
        if ((sum+S) % 2 == 1) return 0;
        //because in this case it is not possible
        int target = (sum + S)/2;

        int dp[] = new int [target+1];
        dp[0] = 1;
        for (int i = 0; i < nums.length; ++i){
            for (int j = target; j >= nums[i]; --j){
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[target];
    }
}
```