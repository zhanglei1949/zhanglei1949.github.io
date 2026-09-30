---
title: LeetCode 题解-42. Trapping Rain Water
published: 2019-12-20T16:38:06+08:00
slug: leetcode-problem-74
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

# Problem 

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it **in-place**. 

# Example

```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

# Solution 1

这道题目特别注重对空间复杂度的控制，最优的解法应当时常数空间。空间开销最大的当属O(mn). 即，遍历一遍矩阵，
得到一个mxn的矩阵，其中每一个元素表示matrix上对应位置上的元素是不是应该被置为0.鉴于其过于简单，我们略去其
实现。

# Solution 2

在O(mxn)复杂度的基础上，我们可以将复杂度减小至O(m+n). 因为从上一种方法的实现中我们发现，其实我们并不需要记录每一个元素
，只需要记录行和列的索引。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        //O(m_n) space
        int [] rows = new int [matrix.length];
        int [] cols = new int [matrix[0].length];
        for (int i = 0; i < matrix.length; ++i){
            for (int j = 0; j < matrix[0].length; ++j){
                if (matrix[i][j] == 0){
                    rows[i] = 1;
                    cols[j] = 1;
                }
            }
        }
        for (int i = 0; i < matrix.length; ++i){
            if (rows[i] == 1){
                for (int j = 0; j < matrix[i].length; ++j){
                    matrix[i][j] = 0;
                }
            }
        }
        for (int j = 0; j < matrix[0].length; ++j){
            if (cols[j] == 1){
                for (int i = 0; i < matrix.length; ++i){
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

# Solution 3

有没有常数复杂度的实现呢？我很自然地想到了位运算。但是因为最大只能接受64x64地矩阵作为输入，所以失败了。
在看到被人地题解后恍然大悟。O(1)的复杂度自然要求我们把信息存入到矩阵中。我们不妨征用matrix[0][...]和matrix[...][0]作为记录matrix[1..m][1...n]的置零信息。至于第0列行第0行，我们只需要2个单独的变量即可。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int row = 0;
        int col = 0;
        for (int i = 0; i < matrix.length; ++i){
            for (int j = 0; j < matrix[i].length; ++j){
                if (matrix[i][j] == 0){
                    if (i == 0 && col == 0) col = 1;
                    if (j == 0 && row == 0) row = 1;
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < matrix.length; ++i){
            for (int j = 1; j < matrix[i].length; ++j){
                if (matrix[0][j] == 0 | matrix[i][0] == 0) matrix[i][j] = 0;
            }
        }
        if (row == 1){
            for (int i = 0; i < matrix.length; ++i){
                matrix[i][0] = 0;
            }
        }
        if (col == 1){
            for (int i = 0; i < matrix[0].length; ++i){
                matrix[0][i] = 0;
            }
        }
    }
}
```