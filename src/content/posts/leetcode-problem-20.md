---
title: LeetCode 题解-20. Valid Parentheses
published: 2019-10-24T18:01:32+08:00
slug: leetcode-problem-20
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 20. Valid Parentheses 题目的求解。

## Problem Description

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

## Example

Example 1
```
Input: "()"
Output: true
```
Example 2
```
Input: "()[]{}"
Output: true
```
Example 3
```
Input: "(]"
Output: false
```
Example 3
```
Input: "([)]"
Output: false
```

## Analysis

这道题比较简单，只需要使用栈这种数据结构就可以轻松解决。遇到左括号，我们push进栈，遇到右括号，如果此时栈为空或者栈顶不是对应的左括号，我们即可判断匹配失败。如果是对应的左括号，那么pop出栈顶。然而我发现使用java自带的栈的代码只能在leetcode上超过60%的时间复杂度，说明我们代码还有提升空间。进一步考虑使用数组来代替java数据结构，我们只需要创建一个足够大的数组（不需要考虑扩张以及banlance factor），并且记录栈顶的位置即可。

更进一步的观察我们发现，由于需要考虑三种不同的括号，我们不得不对三个括号的匹配进行列举，使得代码冗长。如果我们在push进栈时将我们期待的右括号push进去，那么我们在后续匹配时就不用遍历三种可能的情况，而是直接比较当前括号和栈顶括号是否相同即可。

## Solution 

```java
class Solution {
    public static boolean isValid(String s) {
        if (s.length() == 0) return true;
        if (s.length() % 2 == 1) return false;
        Stack<Integer> st = new Stack<Integer>();
        for (int i = 0; i < s.length(); ++i){
            if (s.charAt(i) == '('){
                st.push(0);
            }
            else if(s.charAt(i) == '{'){
                st.push(2);
            }
            else if (s.charAt(i) == '['){
                st.push(4);
            }
            else if (s.charAt(i) == ')'){
                if (! st.isEmpty() && st.peek() == 0){
                    st.pop();
                }
                else return false;
            }
            else if (s.charAt(i) == '}'){
                if (! st.isEmpty() && st.peek() == 2){
                    st.pop();
                }
                else return false;
            }
            else {
                if (! st.isEmpty() && st.peek() == 4){
                    st.pop();
                }
                else return false；
            }
        }
        return st.isEmpty();
    }
}
```

## Optimized Solution


```java
class Solution {
    public static boolean isValid(String s) {
        if (s.length() == 0) return true;
        if (s.length() % 2 == 1) return false;
        //Stack<Character> st = new Stack<Character>();
        char arr[] = new char[s.length()];
        int ind = -1;
        // store what we expected
        for (int i = 0; i < s.length(); ++i){
            if (s.charAt(i) == '('){
                arr[++ind] = ')';
            }
            else if (s.charAt(i) == '{'){
                arr[++ind] = '}';
            }
            else if (s.charAt(i) == '['){
                arr[++ind] = ']';
            }
            else if (ind >= 0 && arr[ind] == s.charAt(i)){
                ind --;
            }
            else {
                return false;
            }
        }
        return ind == -1;
    }
}
```
