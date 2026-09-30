---
title: LeetCode 题解-47. Permutation II
published: 2019-10-31T21:42:12+08:00
slug: leetcode-problem-47
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 47. Permutation II 题目的求解。

## Problem Description

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

## Example

Example 1
```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## Analysis

由题意可知，这道题是一道排列题，而且题目要求返回所有的符合的排列结果，这给我们的求解带来了很大的难度。题意很简单，就是将若干个数字的全部排列返回，并剔除重复的元素。

我们不放回想我们自己是怎么得到一串数列的全排列的。对于n个数的序列，我们假设得到了后n-1个数的所有排列，那么我们只需要将第一个数插入到这些排列的不同的位置即可得到想要n个数的全排列。由此思路我们可以写出递归法的代码。在java具体实现的时候要注意深拷贝的问题。通过已有list复制初始化并不能另开辟空间。

## Solution 

```java
    public static List<List<Integer> > permuteUnique(int[] nums) {
        return my(nums, 0)；
    }
    public static List<List<Integer> > my(int [] nums, int s){
        List<List<Integer>> newres = new ArrayList<List<Integer>>  ();
        if (s >= nums.length) return newres;
        else if (s == nums.length - 1){
            List<Integer> a = new ArrayList<Integer>();
            a.add(nums[s]);
            newres.add(a);
            return newres;
        }
        List<List<Integer>> res = my(nums, s+1);
        // create new permutation
        Iterator<List<Integer>> iter = res.iterator();
        while (iter.hasNext()){
            List<Integer> l = iter.next();
            for (int i = 0; i <= l.size(); ++i){
                List<Integer> tmp = new ArrayList<Integer>();
                for (int j : l){
                    tmp.add(j);
                }
                tmp.add(i, nums[s]);
                if (newres.contains(tmp)){
                    continue;
                }
                newres.add(tmp);
            }
            iter.remove();
        }
        return newres;
    }
```

## Optimized Solution
回溯法
回溯法
