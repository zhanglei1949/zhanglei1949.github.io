---
title: LeetCode 题解-3. Longest Substring Without Repeating Characters
published: 2019-10-23T17:01:45+08:00
slug: leetcode-problem-3
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 3. Longest Substring Without Repeating Characters 题目的求解。

## Problem Description

Given a string, find the length of the longest substring without repeating characters.

## Example

### Example 1

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3.
```

### Example 2

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### Example 3

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Analysis

这道题很明显是一道Two-point的题目。一个很直观的想法就是维护一个sliding-window，从头到尾扫过去，记录所有可能的结果里最长的那个子串。基于这种思想，我们可以得到时间复杂度为$\Theta(n)$，空间复杂度为$O(min(m,n))$。时间复杂度很直观，我们只需要遍历一次；空间复杂度即为我们存储哈希表和sliding-window的空间。分析可知，哈希表的大小最多为字符集的大小或string的长度，所以，我们取两者最小。

事实上，基于滑动窗口的算法可以有两种不同的具体实现，如下代码所示。两种方案的不同主要集中在哈希表key-value pair的设计的不同。在第一种方案里，我们将字符作为key，将字符在当前窗口里出现的次数作为value，这样，我们可以通过value是否为0来确定该字符是否已经出现过。在第二种方案里，我们将该字符最近一次出现的位置作为value。两种方案的复杂度相同，但是在submmit的时候结果不同：第一种方案的耗时更少一点。至于具体原因，我猜测是因为在第一种方案里，我们在遇到重复字符时手动去更新哈希表所带来的时间消耗小于在第二种方案里不不更新滑动窗口缩水对哈希表的影响所带来for 循环次数的增加。

## Solution No.1

```java
    public static int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> flag = new HashMap<Character, Integer>();
        if (s.length() <= 1) return s.length();
        int a=0,b=0;
        int max = 0;
        while (a <= b && b < s.length()){
            //System.out.printf("%d %d\n",b,  flag.get(s.charAt(b)));
            if (flag.getOrDefault(s.charAt(b), 0) == 0){
                flag.put(s.charAt(b), 1);
            }
            else {
                // pop out from front
                while (s.charAt(a) != s.charAt(b) && a < b){
                    flag.put(s.charAt(a), 0);
                    a+=1;
                }
                a++;
            }
            //System.out.printf("%d %d\n", a,b);
            max = Math.max(max, b - a + 1);
            b += 1;
        }
        return max;
    }
```

## Solution No.2

在solution 2中需要注意的是`i = Math.max(i, flag.get(s.charAt(j)) + 1);`这句代码使得我们不用去因为滑动窗口左端pointer向左移动而删除字符而对哈希表进行更新，例如可以考虑`tmsmfdut`。假设我们现在`j = s.length()-1，i= 2`，直接取计算`map.get(s.charAt(j))+1`会给我们带来错误的结果，因为我们计算了两个t。
所以通过取i和`flag.get(s.charAt(j)) + 1`两者中的最大值，我们可以确保答案是有效的。

```java
     public static int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> flag = new HashMap<Character, Integer>();
        if (s.length() <= 1) return s.length();
        int res = 0;
        for (int i = 0, j = 0; j < s.length(); ++j){
            if (flag.containsKey(s.charAt(j))){
                i = Math.max(i, flag.get(s.charAt(j)) + 1);
                //ensure the current window
            }
            flag.put(s.charAt(j), j);
            res = Math.max(res, j - i + 1);
        }
        return res;
    }
```
