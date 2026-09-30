---
title: LeetCode 题解-5. Longest Palindromic Substring
published: 2019-11-26T11:37:02+08:00
slug: leetcode-problem-5
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 5. Longest Palindromic Substring 题目的求解。

## Problem Description

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

## Example 1

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

## Example 2
```
Input: "cbbd"
Output: "bb"s
```

## Analysis

这道题的题意为从字符串的子串中找出最长的回文串．而回文可以递归定义，所以我们可以靠动态规划来解决这个问题．复杂度为O(n^2).

## Solution 1: dp

```java
class Solution {
    public static String longestPalindrome(String s) {
        int l = s.length();
        if (l == 0) return s;
        int max = 0;
        int max_i = 0;
        int max_j = 0;
        int dp[][] = new int [l][l];
        for (int i = 0; i < l; ++i){
            dp[i][i] = 1;
        }
        for (int i = 1; i <= l - 1; ++i){
            for (int j = 0; j < l - i; ++j){
                if (s.charAt(j) == s.charAt(j+i)){
                    if (i == 1) dp[j][j+i] = 1;
                    else dp[j][j+i] = dp[j+1][j+i-1];
                }
                if (dp[j][j+i] == 1){
                    if (i + 1 > max){
                        max = i + 1;
                        max_i = j;
                        max_j = j+i;
                    } 
                }
            }
        }
        return s.substring(max_i, max_j+1);
    }
}
```

## Solution 2: dfs solution with stack

还有一种思路就是先选定回文的中心，在进行扩展，遍历所有可能的中心得到最长的结果．
```java
public static String longestPalindrome(String s) {
    //each palindrome is centered
    if (s.length() == 0) return s;
    int l = 0;
    int r = 0;
    int max_l = 0; int max_r = 0; int max_ = 0;
    for (int i = 0; i < s.length() - 1; ++i){
        //extend(i, i, )
        l = i;r = i;
        while ( l >= 0 && r <= s.length() - 1 && s.charAt(l) == s.charAt(r)) {
            l --; r ++;
        }
        if (max_ < r - l - 1){
            max_ = r - l - 1;
            max_l = l+1; max_r = r-1;
        }
        //extent(i,i+1)
        l = i;r = i+1;
        while ( l >= 0 && r <= s.length() - 1 && s.charAt(l) == s.charAt(r)) {
            l --; r ++;
        }
        if (max_ < r - l - 1){
            max_ = r - l - 1;
            max_l = l+1; max_r = r-1;
        }
    }
    return s.substring(max_l, max_r+1);
}
```
