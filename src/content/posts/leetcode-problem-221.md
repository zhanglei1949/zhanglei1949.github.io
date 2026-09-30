---
title: LeetCode 题解-221. Maximal Square
published: 2019-11-13T20:50:12+08:00
slug: leetcode-problem-221
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 221. Maximal Square 题目的求解。

## Problem Description

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

## Example

```
Input:
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output: 4
```

## Analysis

看到网格类题目，首先想到动态规划。既然要用动态规划，我们必须要找到递归规律。我们很自然的想到，用dp[i][j]表示以(i,j)为右下角的最大的正方形。那么

- 如果matrix[i][j]为0,则dp[i][j]必为0
- 如果matrix[i][j]为1,则有两种情况
    - dp[i-1][j-1] > max(dp[i-1][j], dp[i][j-1]) 这种情况下，我们最大能得到min(dp[i-1][j], dp[i][j-1])+1的正方形。
    - dp[i-1][j-1] < max(dp[i-1][j], dp[i][j-1]) 这种情况下，我们最大能得到dp[i-1][j-1]+1的正方形。

所以`dp[i][j] = Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1]) + 1;`
## Solution 1

```java

public static int maximalSquare(char[][] matrix) {
    //right down corner
    if (matrix.length < 1 || matrix[0].length < 1) return 0;
    int dp[][] = new int[matrix.length + 1][matrix[0].length + 1];
    int max = 0;
    for (int i = 1; i < dp.length; ++i){
        for (int j = 1; j < dp[i].length; ++j){
            if (matrix[i-1][j-1] == '0') dp[i][j] = 0;
            else dp[i][j] = Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1]) + 1;
            max = Math.max(dp[i][j], max);
        }
    }
    return max*max;
}

```

## Solution 2
事实上，我们并不需要mxn的矩阵来储存所有的dp元素，2xn的矩阵即可，由此我们可以将空间复杂度降低至O(n),达到理论上最优。
```java
public static int maximalSquare(char[][] matrix) {
    //right down corner
    if (matrix.length < 1 || matrix[0].length < 1) return 0;
    int dp[][] = new int[2][matrix[0].length + 1];
    int max = 0;
    for (int i = 1; i < matrix.length + 1; ++i){
        for (int j = 1; j < matrix[0].length + 1; ++j){
            int cur = (i-1) % 2;
            int prev = i % 2;
            if (matrix[i-1][j-1] == '0') dp[cur][j] = 0;
            else dp[cur][j] = Math.min(Math.min(dp[prev][j], dp[cur][j-1]), dp[prev][j-1]) + 1;
            max = Math.max(dp[cur][j], max);
        }
    }

    return max*max;
}
```

## Solution 3
那么，时间复杂度还有没有优化的空间呢？好像没有了，但是我的代码只能击败5%的人，为什么呢？