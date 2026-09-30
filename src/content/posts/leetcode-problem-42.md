---
title: LeetCode 题解-42. Trapping Rain Water
published: 2019-12-20T16:38:06+08:00
slug: leetcode-problem-42
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 42. Trapping Rain Water 题目的求解。

## Problem Description

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.
![avatar](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

## Example 1
```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

## Analysis

很经典的一道题目，解决这个问题的关键点在于明白怎么样才能在一个格子里面集到最多的水．不妨假设我们想要知道在第i个格子里面最多可以集聚到多少水．那么，我们很容易发现，取决与height[i+1...n]中最大数和height[0...i-1]中最大数的较小的那个．所以，我们只需要知道左边最大和右边最大即可．那么可以有两种方案．

### 基于动态规划

首先从左边遍历一次数组，dp[i] = max(dp[j]) where 0<=j<=i; 同样再从右边向左遍历一次，这样，对于我们就知道左边最大和右边最大的高度了．

### 基于two-pointer.

设定两个指针，left, right. 初始时分别设为最左和最右．在每一次迭代中，判断height[left] 和height[right]的大小．为什么要选小的呢？因为其实我们要找的是两个最大值中的最小值，要想找到更大的最小值，自然是要舍弃现在的最小值．只有这样才能找到更大的．


## Solution 1

```java
public static int trap(int[] height) {
    if (height.length == 0) return 0;
    int left = 0;
    int right = height.length - 1;
    int maxLeft = 0;
    int maxRight = 0;
    int sum = 0;
    while (left < right){
        if (height[left] < height[right]){
            if (height[left] <= maxLeft){
                sum += maxLeft - height[left];
            }
            else {
                maxLeft = height[left];
            }
            left ++;
        }
        else {
            if (height[right] <= maxRight){
                sum += maxRight - height[right];
            }
            else {
                maxRight = height[right];
            }
            right --;
        }
    }
    return sum;
}
```

## Solution 2

```java
public static int trap(int[] height) {
    int dp[] = new int [height.length];
    int sum = 0;
    int max = 0;
    for (int i = 0; i < height.length; ++i){
        if (max < height[i]) max = height[i];
        dp[i] = max;
    }
    max = 0;
    for (int i = height.length-1; i >= 0; --i){
        if (max < height[i]) max = height[i];
        sum += Math.min(max, dp[i]) - height[i];
    }
    return sum;
}
```

## Solution 3

```java
public static int trap(int [] height){
        //using stack
        if (height.length == 0) return 0;
        Stack<Integer> s = new Stack<Integer>();
        int sum = 0;
        for (int i = 0; i < height.length; ){
            if (s.isEmpty()) s.push(i++);
            else if (height[s.peek()] >= height[i]) s.push(i++);
            else {
                int top = s.pop();
                if (!s.isEmpty()){
                    sum += (Math.min(height[s.peek()], height[i]) - height[top]) * (i - s.peek() - 1);
                }
                else continue;
            }
        }
        return sum;
    }
```