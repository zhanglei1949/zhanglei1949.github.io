---
title: LeetCode 题解-136. Single Number
published: 2019-11-28T13:39:12+08:00
slug: leetcode-problem-136
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 136. Single Number 题目的求解。

## Problem Description

Given a **non-empty** array of integers, every element appears twice except for one. Find that single one.

## Example 1

```
Input: [2,2,1]
Output: 1
```

## Example 2
```
Input: [4,1,2,1,2]
Output: 4
```




## Solution 1: brute force
时间复杂度为线性．
```java
class Solution {
    public static int singleNumber(int[] nums) {
        Map<Integer,Integer> m = new HashMap<Integer, Integer>();
        for (int num : nums){
            m.put(num, m.getOrDefault(num, 0)+1);
        }
        for (int num : m.keySet()){
            if (m.get(num) == 2) continue;
            if (m.get(num) == 1) return num;
        }
        return 0;
    }
}
```

## Solution 2: bit operation

时间复杂度为线性，利用了相同的数异或为0的特点．
```java
class Solution {
    public static int singleNumber(int[] nums) {
        int res = 0;
        for (int num : nums){
            res = res ^ num;
        }
        return res;
    }
}
```

## 拓展
有一个 n 个元素的数组，除了两个数只出现一次外，其余元素都出现两次，让你找出这两个只出现一次的数分别是几，要求时间复杂度为 O(n) 且再开辟的内存空间固定(与 n 无关)。

## Solution
如果我们按照前面提到的方法对所有元素求异或的话，我们是得不到最后答案的，因为我们得到的结果是这两个数的异或，那么我们想到，再遍历一次数组，对每个元素单独再异或，只可能有三种结果，其中只会出现一次的两个结果即为我们想要的答案．
```java
class Solution{
    public static int singleNumber(int [] nums){
        int res = 0;
        for (int num : nums){
            res = res ^ num;
        }
        Map<Integer,Integer> m = new HashMap<Integer, Integer>();
        for (int num : nums){
            int tmp = res ^ num;
            m.put(num, m.getOrDefault(num, 0) + 1);
        }
        for (int num : m.keySet()){
            if (m.get(num) == 1) System.out.printf("%d \n", num);
        }
        return 0;
    }
}
```
