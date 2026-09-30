---
title: LeetCode 题解-10. Regular Expression Matching
published: 2019-12-02T14:18:49+08:00
slug: leetcode-problem-10
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 10. Regular Expression Matching 题目的求解。

## Problem Description

Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '*'.

- '.' Matches any single character.
- '*' Matches zero or more of the preceding element.
The matching should cover the entire input string (not partial).

Note:

+ `s` could be empty and contains only lowercase letters `a-z`.
+ `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

## Example 1

```
Input :
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

## Example 2

```
Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

## Example 3

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

## Example 4

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```

## Example 5

```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
``` 

## Analysis

这道题目要求我们实现一个正则匹配的引擎，包括`*`和`.`．我刚开始想用贪心匹配解决，发现不能解决全串匹配的问题，也就是说，贪心匹配不保证我们考虑到所有可能的情况．根据leetcode discussion提供的思路，我们使用动态规划的方法解决此问题．

我们开辟二维数组dp[i][j]来表示我们是否可以用p[0...j]的正则表达式成功匹配s[0...i]的字符串．递归公式我们可以这样考虑．假设我们在dp[i][j]之前已经计算好了dp[0...i-1][0..j-1]. 至于dp[i][j],我们考虑三种不同的情况，

- 1. s[i] == p[j]. 那么如果我们就可以继承dp[i-1][j-1]. 

- 2. p[j] == '.',  同样，我们可以继承dp[i-1][j-1].

- 3. p[j] == '*'. 这意味着p[j-1] 是[a-z]的字符．所以我们需要将讨论p[j-1]与s[i]的关系．
    - p[j-1] == '.' 或 p[j-1] == s[i]. 在这种情况下，我们可以有不同的可能，只要有一种可能能够成功匹配，我们就能成功匹配．　如果我们不适用`[a-z|.]*'进行匹配，结果是dp[i][j] = dp[i][j-2]. 如果我们只匹配一次，dp[i][j] = dp[i][j-1]. (相当于不用\*); 如果我们匹配多次，dp[i][j] = dp[i-1][j].
    - p[j-1] 不能和s[i]进行匹配，即匹配失败．所以，dp[i][j] = dp[i][j-2];

最终返回dp[i][j]即为结果．

## Solution 

需要注意的是在初始化的时候需要初始化dp[0][0..j].

```java
class Solution {
    public static boolean isMatch(String s, String p){
        int dp[][] = new int [s.length()+1][p.length()+1];
        dp[0][0] = 1;
        for (int i = 1; i <= p.length(); ++i){
            if (p.charAt(i-1) == '*') dp[0][i] = dp[0][i-2];
        }
        for (int i = 1; i <= s.length(); ++i){
            for (int j = 1; j <= p.length(); ++j){
                if (p.charAt(j-1) == s.charAt(i-1)){
                    dp[i][j] = dp[i-1][j-1];
                }
                else if (p.charAt(j-1) == '.'){
                    dp[i][j] = dp[i-1][j-1];
                }
                else if (p.charAt(j-1) == '*'){
                    //in this condition, we have to check dp[i-1][j-1] and dp[i][j-1] and dp[i-1][j]
                    if (p.charAt(j-2) == s.charAt(i-1) || p.charAt(j-2) == '.') {
                        dp[i][j] = dp[i][j-2] | dp[i-1][j] | dp[i][j-1];
                                // no matches | multiple matches | single match
                    }
                    else dp[i][j] = dp[i][j-2];
                }
                //default 0
            }
        }
        // for (int i = 0; i < s.length(); ++i){
        //     for (int j = 0; j < p.length(); ++j){
        //         System.out.printf("%d ", dp[i][j]);
        //     }
        //     System.out.println();
        // }
        if (dp[s.length()][p.length()] == 1) return true;
        return false;
    }
}
```
