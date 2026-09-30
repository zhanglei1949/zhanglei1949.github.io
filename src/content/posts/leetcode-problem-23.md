---
title: LeetCode 题解-15. 3Sum
published: 2019-12-05T14:59:34+08:00
slug: leetcode-problem-23
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 23. Merge k Sorted Lists 题目的求解。

## Problem Description

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

## Example 1

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```


## Analysis

之前做过将两个linked list 合并的题目，即为归并排序的思路，这道题要求我们将k个排好序的链表做归并．有两种思路

- 将k个list的归并不断分解为两个list的归并，即采用二分的方法
- 采用最直观的思路，既然k个列表都是排好序的，那么在任何时刻，表头的元素都是当前最小的元素．可以使用priority queue来找到最小的元素，并且更换表头．


## Solution 1

```java
class Solution {
    public static ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) return null;
        return part(lists, 0, lists.length - 1);
    }
    public static ListNode part(ListNode[] lists,int s, int t) {
        if (s == t) return lists[s];
        ListNode l1 = part(lists, s, (s+t)/2);
        ListNode l2 = part(lists, (s+t)/2+1, t);
        return merge(l1, l2);
    }
    public static ListNode merge(ListNode l1, ListNode l2){
        ListNode zero = new ListNode(0);
        ListNode tmp = zero;
        while (l1 != null && l2 != null){
            if (l1.val < l2.val){ tmp.next = l1; l1 = l1.next;}
            else {tmp.next = l2; l2 = l2.next;}
            tmp = tmp.next;
        }
        while (l1 != null) {tmp.next = l1; l1 = l1.next; tmp = tmp.next;}
        while (l2 != null) {tmp.next = l2; l2 = l2.next; tmp = tmp.next;}
        return zero.next;
    }
}
```

## Solution 2

```java
class Solution{
        public static ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) return null;
        //priority queue
        PriorityQueue<ListNode> q = new PriorityQueue<ListNode>(lists.length, 
            new Comparator<ListNode>() {
                @Override
                public int compare(ListNode l1, ListNode l2){
                    if (l1.val < l2.val){
                        return -1;
                    }
                    else if (l1.val > l2.val){
                        return 1;
                    }
                    else return 0;
                }
            }
        );
        for (ListNode l : lists){
            if (l != null) q.add(l);
        }
        ListNode zero = new ListNode(0);
        ListNode tmp = zero;
        while (!q.isEmpty()){
            ListNode l = q.poll();
            tmp.next = l;
            tmp = tmp.next;
            if (l.next != null) q.add(l.next);
        }
        return zero.next;

    }
}
```

## Solution 3

```java
class Solution {
    public static ListNode mergeKLists(ListNode[] lists) {
        if (lists.length ==0) return null;
        ListNode zero = new ListNode(0);
        ListNode tmp = zero;
        while (true){
            ListNode min_node = find_min_node(lists);
            if (min_node == null) break;
            tmp.next = min_node;
            tmp = tmp.next;
        }
        return zero.next;
    }
    public static ListNode find_min_node(ListNode[] lists) {
        ListNode res = help(lists, 0, lists.length-1);
        if (res == null) return null;
        for (int i = 0; i < lists.length; ++i){
            //System.out.print(res);
            //System.out.println(lists[i]);
            if (res.equals(lists[i])){
                lists[i] = res.next;
                break;
            }
        }
        return res;
    }
    public static ListNode help(ListNode[] lists, int s, int t){
        //System.out.printf("%d %d\n", s, t);
        if (s == t) return lists[s];
        ListNode l1 = help(lists, s, (t+s)/2);
        ListNode l2 = help(lists, (t+s)/2 + 1, t);
        if (l1 == null && l2 == null ) return null;
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) return l1;
        else return l2;
    }
```