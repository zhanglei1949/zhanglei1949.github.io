---
title: LeetCode 题解-25. Reverse Nodes in k-Group
published: 2019-10-28T16:20:09+08:00
slug: leetcode-problem-25
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 25. Reverse Nodes in k-Group 题目的求解。

## Problem Description

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

## Example

```
Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5
```

### Note

1. Only constant extra memory is allowed.
2. You may not alter the values in the list's nodes, only nodes itself may be changed.

## Analysis

这道题和206题一样，也是一道链表题目，核心是指针的交换操作。关键点总结如下

- 在链表最前端设置zero节点。
- 每次选中k个节点，不停地将cur的next节点挪到start的后面

事实上，代码还可以更简练，通过recursive的方法。
事实上，代码还可以更简练，通过recursive的方法。

## Solution 

```java
class Solution {
    public static ListNode reverseKGroup(ListNode head, int k) {
        ListNode zero = new ListNode(0);
        zero.next = head;
        ListNode start = zero;
        ListNode tail = zero;
        while (true){
            for (int i = 0; i < k ; ++i){
                tail = tail.next;
                if (tail == null) break;
            }
            if (tail == null) break;
            start = reverseK(start, k);
            tail = start;
        }
        return zero.next;
    }
    public static ListNode reverseK(ListNode start, int k){
        ListNode cur = start.next;
        for (int i = 0; i < k - 1; ++i){
            ListNode tmp = cur.next.next;
            cur.next.next = start.next;
            start.next = cur.next;
            cur.next =  tmp;
        }

        return cur;
    }
}

```
