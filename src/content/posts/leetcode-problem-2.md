---
title: LeetCode 题解-2.add two numbers
published: 2019-10-20T12:11:32+08:00
slug: leetcode-problem-2
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 2. two sum 题目的求解。

## Problem Description

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## Example

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## Solution

就像用链表实现大整数运算一样，算法没有什么可将的，就是要注意边界条件

1. 考虑两个n位数加起来为n+1位数的情况
2. 考虑0+一个数的情况
3. 考虑对齐相加进位的实现。

## Code

```java
class Solution {
    
    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode tmp = res;
        int last = 0;
        while (l1 != null && l2 != null){
            tmp.next = new ListNode((l1.val + l2.val + last)%10);
            last = (l1.val + l2.val +last) / 10;
            l1 = l1.next;
            l2 = l2.next;
            tmp = tmp.next;
        }
        ListNode temp = null;
        if (l1 != null){
            temp = l1;
        }
        if (l2 != null){
            temp = l2;
        }
        while (temp != null){
            tmp.next = new ListNode((temp.val + last)%10);
            last = (temp.val +last) / 10;
            temp = temp.next;
            tmp = tmp.next;
        }
        if (last > 0){
            tmp.next = new ListNode(last);
        }
        return res.next;
    }
}
```
