---
title: LeetCode 题解-206. Reverse Linked List
published: 2019-10-28T16:20:09+08:00
slug: leetcode-problem-206
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 206. Reverse Linked List 题目的求解。

## Problem Description

Reverse a singly linked list.

A linked list can be reversed either iteratively or recursively. Could you implement both?

## Example

Example 1
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

## Analysis

这道题的核心其实是指针的交换。方便起见，我们引入多余的head指针，指向第一个node。迭代实现和递归实现如下。

## Iterative Solution 

```java
class Solution {
    ListNode zero = new ListNode(0);
    zero.next = head;
    ListNode tmp = new ListNode(0);
    while (head != null){
        tmp = head.next;
        if (tmp == null) break;
        head.next = head.next.next;
        tmp.next = zero.next;
        zero.next = tmp;
    }
    return zero.next;
}
```

## Recursive Solution

递归方法的核心是先reverse剩余的n-1个指针，再将第一个指针接到最后。

```java
class Solution {
    public static ListNode reverseList(ListNode head) {
        if (head == null) return head;
        ListNode tail = head;
        while (tail.next != null){
            tail = tail.next;
        }
        myreverse(head, tail);
        head.next = null;
        return tail;
    }
    public static void myreverse(ListNode head, ListNode tail){
        if (head == tail){
            return ;
        }
        myreverse(head.next, tail);
        head.next.next = head;
    }
}
```
