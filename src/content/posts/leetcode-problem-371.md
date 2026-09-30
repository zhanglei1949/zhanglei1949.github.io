---
title: LeetCode 题解-371. Sum of Two Integers
published: 2019-11-28T16:15:40+08:00
slug: leetcode-problem-371
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 371. Sum of Two Integers 题目的求解。

## Problem Description

Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

## Example 1

```
Input: a = 1, b = 2
Output: 3
```

## Example 2
```
Input: a = -2, b = 3
Output: 1
```

## Analysis

在不使用内置加法运算的情况下求两个数的和，我们需要依赖位运算．在位运算中我们需要关注两种运算

- 异或．两个数异或的结果恰恰是不进位加法的结果．
- 与．两个数与运算后的结果恰恰是进位的结果．

所以对于a+b,我们求得异或和与的结果之后，再将两个结果相加，长此以往，直到其中一个为零：没有进位了，或者加法结果恰好为2^n．

## Solution 1: bfs with recursive call

```java
class Solution{
    public static int getSum(int a, int b) {
        while (a != 0 && b != 0){
            int tmp = a ^ b;
            b = (a & b) << 1;
            a = tmp;
            //System.out.printf("%d %d\n", a, b);
        }
        if (a == 0) return b;
        else return a;
    }
｝
```
