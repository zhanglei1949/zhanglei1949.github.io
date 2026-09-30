---
title: LeetCode 题解-11. Container With Most Water
published: 2019-11-18T20:01:31+08:00
slug: leetcode-problem-11
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 11. Container With Most Water 题目的求解。

## Problem Description

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note**: You may not slant the container and n is at least 2.


### Example 1

```

Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```


## Analysis

这道题虽然代码简单，但是要求我们具有足够的观察能力和分析能力得出规律．首先，最简单的方法就是暴力求解，遍历长度为1-n的所有情况，复杂度为O(n^2).

我们现在试着找些规律．假设给定的lines为[a,b,...,c,d].首先长度为n的container的最大容积只能是(a,d).对于长度为n-1的两个容器，我们观察可知，只有将较短的那根线向内移动，才有可能得到更大的容积，否则不会超过n时候的情况（木桶效应）．依次类推，我们每次移动较短的那块板，以寻求更大的容积．

但是我们好像忽略了一种情况，假如a<d,那么我们下一步会取[b,...,c,d]那么有没有可能在[a,b,...,c]中取得较高值的可能呢？答案是否定的．如果在后者有更高的值，那么肯定有一条边是a,假设另外一条边是e,如果a<e，那么最大的面积实际上应该在[a,d]取得．如果a>=e，那么e<=a<d,最大的面积也是在[a,d]取到．反证结束．

## Solution 1

```java
class Solution {
    public static int maxArea(int[] height) {
        int max = 0;
        int l = 0;
        int r = height.length - 1;
        while ( l < r){
            max = Math.max( Math.min(height[l], height[r]) * (r - l), max);
            if (height[l] < height[r]){
                l++;
            }
            else {
                r--;
            }
        }
        return max;
    }
}

```

