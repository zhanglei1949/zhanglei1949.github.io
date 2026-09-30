---
title: LeetCode 题解-1.two sum
published: 2019-10-20T16:31:01+08:00
slug: leetcode-problem-1
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---
对Leetcode 1. two sum 题目的求解。


## Problem Description

Given an array of integers, return indices of the two numbers such that they add up to a specific target. You may assume that each input would have exactly one solution, and you may not use the same element twice.

## Example

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## Solution

这道题题意很明确，即从一个数组中找出两个数，满足其和为`target`的要求，将这两个数的下标以数组的形式返回。首先，考虑**brute-force**穷举的方法，时间复杂度为 $\Theta(n^2)$，空间复杂度为$\Theta(1)$.

其次，考虑时间复杂度更低的方案。既然要找出两个数，我们不妨先确定一个数$a$，如果数组中存在$target-a$，那么我们就找到了答案。如果不行的话，我们再考虑其他数。至于如果确定数组包含某个数，我们考虑使用哈希表，来实现线性查询时间。
在具体实现中，我们需要对数组进行两次遍历，第一次建立哈希表，第二次进行查询。需要注意，我们通过将数组元素的index作为value存入到`hashmap`中，以避免第二次遍历的时候出现自己加自己的情况。这种算法的时间复杂度为$\Theta(n)$，空间复杂度为$\Theta(n)$.
## Code

```java
class Solution {
    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> m = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i){
            m.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; ++i){
           if (m.get(target - nums[i]) != null && m.get(target - nums[i]) != i){
               return new int [] {Math.min(i, m.get(target - nums[i])),
                                Math.max(i, m.get(target - nums[i]))};
           }
        }
        return new int [] {0,0};
    }
}
```

## Improved Solution

事实上，我们只需要一次遍历就可以解决问题。如代码所示，我们在遍历中首先确定查询当前元素的互补元素是否存在，如果存在的话，直接返回结果。然后在将当前元素加入哈希表，这样就可以避免将同一个元素考虑两次。

## Improved Code

```java
class Solution {
    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> m = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i){
            if (m.containsKey(target - nums[i])){
               return new int [] {Math.min(i, m.get(target - nums[i])),
                                Math.max(i, m.get(target - nums[i]))};
            }
            m.put(nums[i], i);
        }
        return new int [] {0,0};
    }
```

## Comments

这个问题其实也可以转换成 ***Two Pointer*** 来求解，只不过需要先排序，找到两个元素之后还需要在原始数组中找到对应的下标。