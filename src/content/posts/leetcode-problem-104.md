---
title: LeetCode 题解-104. maximum depth of binary tree
published: 2019-10-28T16:20:09+08:00
slug: leetcode-problem-104
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 104. maximum depth of binary tree 题目的求解。

## Problem Description

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Note**: A leaf is a node with no children.

## Example

Example 1
```
Given binary tree [3,9,20,null,null,15,7], return its depth = 3.
```

## Solution 1: Iterative solution

这道题目的描述很符合递归算法的特征，而树又是非常适合递归算法的数据结构，因此我们很容易写出两行的代码。需要注意的只有对空节点的判断。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

想要知道树的最大深度，我们只需要进行一次遍历即可。事实上，深度优先和宽度优先的算法都适用与此题。

## Solution 2
深度有限遍历。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        Stack<TreeNode> s= new Stack<TreeNode>();
        Stack<Integer> ss= new Stack<Integer>();
        s.push(root);
        ss.push(1);
        int max = 0;
        while (!s.isEmpty()){
            TreeNode n = s.pop();
            int d = ss.pop();
            max = Math.max(d, max);
            if (n.left != null){
                s.push(n.left);
                ss.push(d+1);
            }
            if (n.right != null){
                s.push(n.right);
                ss.push(d+1);
            }
        }
        return max;
}
```
## Solution 3
宽度优先遍历

```java
public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        Queue<Integer> qq = new LinkedList<Integer>();
        q.add(root);
        qq.add(1);
        int max = 0;
        while (!q.isEmpty()){
            TreeNode n = q.poll();
            int d = qq.poll();
            max = Math.max(d, max);
            if (n.left != null){
                q.add(n.left);
                qq.add(d+1);
            }
            if (n.right != null){
                q.add(n.right);
                qq.add(d+1);
            }
        }
        return max;
    }
```

## Solution 4
事实上，我们还有一种更有效的遍历求解方法，即层次遍历。
```java
    public static int maxDepth(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.add(root);
        int max = 0;
        while (!q.isEmpty()){
            int size = q.size();
            for (int i = 0; i < size; ++i){
                TreeNode tmp = q.poll();
                if (tmp.left != null) q.add(tmp.left);
                if (tmp.right != null) q.add(tmp.right);
            }
            max += 1;
        }
        return max;
    }
```