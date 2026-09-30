---
title: LeetCode 题解-32. Longest Valid Parentheses
published: 2019-12-10T16:51:00+08:00
slug: leetcode-problem-32
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 32. Longest Valid Parentheses 题目的求解。

## Problem Description

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

## Example 1

```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

## Example 2

```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

## Analysis

题目要求我们从一串由左括号和右括号组成的字符串中找出最长的有效子串，有效指well-formed,即闭合的．我们首先想到可以使用栈来判断这个串是不是闭合的，即在扫描完所有字符之后，栈是不是空的．

但是本题要求我们找出闭合的最长子串．我们可以想象，对于一个不闭合的字符串，使用栈进行扫描之后在栈里面残留的是匹配不了的元素，而可以完成匹配的子串恰恰在他们中间．因此，我们只需要在入栈的时候记录入栈字符的index,就可以计算出成功匹配的子串的长度，进而找到最大值．

## Solution 1

```java
class Solution {
    public static int longestValidParentheses(String s) {
        Stack<Integer> st = new Stack<Integer>();
        for (int i = 0; i < s.length(); ++i){
            if (s.charAt(i) == '(') st.push(i);
            else{
                if (!st.empty()){
                    if (s.charAt(st.peek()) != '(') st.push(i);
                    else st.pop();
                }
                else st.push(i);
            }   
        }
        if (st.empty()) return s.length();
        int max = 0;
        int e = s.length();
        while (!st.empty()){
            int start = st.pop();
            max = Math.max(max, e - start - 1);
            e = start;
        }
        return Math.max(max, e);
    }
}
```

## Solution 2

如果不借助于栈，我们该怎么解决这个问题呢？联想到我们之前曾用dp的方法求解过最长连续子串的问题，与这个题目很类似，因此我们考虑如何把此题套到动态规划里面．如果我们用dp[i]来表示以s[i]结尾的最长子串的第一个字符所在位置的话，我们可以列出以下公式

- if (s[i] == '(') , dp[i] = -1;
- if (s[i] == ')')
    - if (s[i-1] == '('), dp[i] = dp[i-2]; 
    - if (s[i-1] == ')')
        - if (s[dp[i-1]-1] == '('), 说明s[i]可以成功匹配，dp[i] = dp[i-1]-1;
        - if (s[dp[i-1]-1] != '('), 那么dp[i] = -1
        
```java
class Solution{
    public static int longestValidParentheses(String s) {
        int dp[] = new int [s.length()];
        if (s.length() <= 1) return 0;
        //dp[i] denotes the start position of the valid parentheses ended at i
        dp[0] = -1;
        if (s.charAt(1) == ')' && s.charAt(0) == '(') dp[1] = 0;
        else dp[1] = -1;
        int max = 0;
        if (dp[1] >= 0) max = Math.max(0, 2 - dp[1]);

        for (int i = 2; i < s.length(); ++i){
            if (s.charAt(i) == '(') dp[i] = -1;
            else if (s.charAt(i) == ')' && s.charAt(i-1) == '('){
                if (dp[i-2] >= 0) dp[i] = dp[i-2] ;
                else dp[i] = i-1;
            }
            else {
                // ))
                if (dp[i-1] == 0) dp[i] = -1;
                else if (dp[i-1] == -1) dp[i] = -1;
                else if (s.charAt(dp[i-1] - 1) == '('){
                    if (dp[i-1] - 2 >= 0 && dp[dp[i-1]-2] == -1) dp[i] = dp[i-1]-1;
                    else if (dp[i-1] - 2 >= 0) dp[i] = dp[dp[i-1]-2];
                    else dp[i] = dp[i-1] - 1;
                }
                else dp[i] = -1;
            }
            if (dp[i] >= 0) max = Math.max(max, i - dp[i] + 1);
        }
        for (int i = 0; i < s.length(); ++i){
            System.out.printf("%d ", dp[i]);
        }
        System.out.println();
        return max;
    }
}
``