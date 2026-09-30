---
title: LeetCode 题解-18. 4Sum
published: 2019-12-03T17:05:50+08:00
slug: leetcode-problem-18
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 18. 4Sum 题目的求解。

## Problem Description

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

The solution set must not contain duplicate triplets.

## Example 1

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```


## Analysis

这是一道two-pointer的题目．通过O(n)的遍历将自由度为3的搜索题目降到自由度为2.由此我们可以转化为2sum的问题．
至于如何解决2sum的问题，我们首先进行排序，再使用two pointer进行搜索，总的复杂度为O(n^2).


## Solution 

```java
class Solution {
    public static List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new LinkedList<>();
        int s = 0;
        int t = 0;
        for (int i = 0; i < nums.length - 2; ++i){
            if (i > 0 && nums[i] == nums[i-1]) continue;
            s = i + 1;
            t = nums.length - 1;
            while ( s < t){
                if (s > i+1 && t < nums.length - 1 && nums[s] == nums[s-1] && nums[t] == nums[t+1]){
                    s ++;
                    t --;
                    continue;
                }
                if (nums[s] + nums[t] == - nums[i]){
                    LinkedList<Integer> l = new LinkedList<>();
                    l.add(nums[i]);
                    l.add(nums[s]);
                    l.add(nums[t]);
                    res.add(l);
                    s ++;
                    t --;
                }
                else if (nums[s] + nums[t] > - nums[i]) t --;
                else s ++;
            }
        }
        return res;
    }
}
```
