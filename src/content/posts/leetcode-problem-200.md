---
title: LeetCode 题解-200. Number of Islands
published: 2019-11-26T10:39:33+08:00
slug: leetcode-problem-200
description: ''
tags:
- LeetCode
- 算法
category: 算法题解
draft: false
lang: zh_CN
---

对Leetcode 200. Number of Islands 题目的求解。

## Problem Description

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

## Example 1

```
Input:
11110
11010
11000
00000

Output: 1
```

## Example 2
```
Input:
11000
11000
00100
00011

Output: 3
```

## Analysis

这道题目的本质就是求矩阵中连续区域的个数．很简单的想法就是用图搜索的方法来探索整个矩阵，将所有访问过的位置全部标为０以免下次在访问．在bfs的实现中，我们需要借助queue，在dfs的实现中，我们可以使用递归，也可以使用stack.

## Solution 1: bfs with recursive call

```java
class Solution {
    public static int numIslands(char[][] grid) {
        if (grid.length ==0 || grid[0].length == 0) return 0;
        int cnt = 0;
        for (int i = 0; i < grid.length; ++i){
            for (int j = 0; j < grid[i].length; ++j){
                if (grid[i][j] == '1'){
                    dfs(i,j,grid);
                    cnt += 1;
                }
            }
        }
        return cnt;
    }
    public static void dfs(int i, int j, char[][] grid){
        if (i < 0 || j < 0 || i > grid.length - 1 || j > grid[i].length - 1 || grid[i][j] == '0') return ;
        grid[i][j] = '0';
        dfs(i+1, j ,grid);
        dfs(i, j+1 ,grid);
        dfs(i, j-1 ,grid);
        dfs(i-1, j ,grid);
    }
}
```

## Solution 2: dfs solution with stack

```java
class Solution{
    public static int numIslands(char[][] grid) {
        if (grid.length ==0 || grid[0].length == 0) return 0;
        int cnt = 0;
        Stack<Pair> s = new Stack<Pair>();
        for (int i = 0; i < grid.length; ++i){
            for (int j = 0; j < grid[i].length; ++j){
                if (grid[i][j] == '1'){
                    System.out.printf("%d %d\n", i, j);
                    s.push(new Pair(i,j));
                    while (!s.isEmpty()){
                        Pair p = s.pop();
                        grid[p.x][p.y] = '0';
                        if (p.x+1 < grid.length && grid[p.x+1][p.y] == '1') s.push(new Pair(p.x+1,p.y));
                        if (p.y+1 < grid[0].length && grid[p.x][p.y+1] == '1') s.push(new Pair(p.x,p.y+1));
                        if (p.x-1 >= 0 && grid[p.x-1][p.y] == '1') s.push(new Pair(p.x-1,p.y));
                        if (p.y-1 >= 0 && grid[p.x][p.y-1] == '1') s.push(new Pair(p.x,p.y-1));
                    }
                    cnt += 1;
                }
            }
        }
        return cnt;
    }
}

```

## Solution 3 bfs

```java
class Solution{
    private static class Pair{
        int x ;
        int y ;
        public Pair(int xx, int yy){this.x = xx; this.y = yy;}
        //public Pair(){this.x = -1; this.y = -1;}
    }
    public static int numIslands(char[][] grid) {
        //dfs?
        if (grid.length == 0 || grid[0].length == 0) return 0;
        int flag[][] = new int [grid.length][grid[0].length];
        Stack<Pair> s = new Stack<Pair>();
        int cur_i = 0;
        int group = 0;
        while (true){
            for (int i = cur_i; i < grid.length; ++i){
                for (int j = 0; j < grid[i].length; ++j){
                    //System.out.printf("a:%d %d\n", i, j);
                    if (grid[i][j] == '1' && flag[i][j] == 0){
                        Pair tmp = new Pair(i,j);
                        s.push(tmp);
                        flag[i][j] = 1;
                        cur_i = i;
                        break;
                    }
                }
                if (!s.isEmpty()){
                    break;
                }
            }
            if (s.isEmpty()) break;
            while (!s.isEmpty()){
                Pair p = s.pop();
                if (p.x+1 < grid.length && flag[p.x+1][p.y] == 0 && grid[p.x+1][p.y] == '1'){
                    Pair tmp = new Pair (p.x + 1, p.y);
                    s.push(tmp);
                    flag[p.x+1][p.y] = 1;
                }
                if (p.x-1 >= 0 && flag[p.x-1][p.y] == 0 && grid[p.x-1][p.y] == '1'){
                    Pair tmp = new Pair (p.x - 1, p.y);
                    s.push(tmp);
                    flag[p.x-1][p.y] = 1;
                }
                if (p.y+1 < grid[0].length && flag[p.x][p.y+1] == 0 && grid[p.x][p.y+1] == '1'){
                    Pair tmp = new Pair(p.x, p.y + 1);
                    s.push(tmp);
                    flag[p.x][p.y+1] = 1;
                }
                if (p.y-1 >= 0 && flag[p.x][p.y-1] == 0 && grid[p.x][p.y-1] == '1'){
                    Pair tmp = new Pair (p.x, p.y-1);
                    s.push(tmp);
                    flag[p.x][p.y-1] = 1;
                }
            }
            group += 1;
        }
        return group;
    }
}
```

## Solution 4: Union find
```java
class Solution{
    public static int numIslands(char[][] grid) {
        //union search
        if (grid.length == 0 || grid[0].length == 0) return 0;
        Pair[][] father = new Pair [grid.length][grid[0].length];
        for (int i = 0; i < grid.length; ++i){
            for (int j = 0; j < grid[0].length; ++j){
                if (grid[i][j] == '1'){
                    father[i][j] = new Pair(i,j);
                }
                else father[i][j] = new Pair(-1, -1);
            }
        }
        for (int i = 0; i < grid.length; ++i){
            for (int j = 0; j < grid[i].length; ++j){
                if (grid[i][j] == '0') continue;
                if (i + 1 < grid.length && grid[i+1][j] == '1' &&(findroot(i, j, father).x != findroot(i+1, j, father).x || findroot(i, j, father).y != findroot(i+1, j, father).y)){
                    join(findroot(i, j, father), findroot(i+1, j, father));
                }
                if (j + 1 < grid[i].length && grid[i][j+1] == '1' && (findroot(i, j, father).x != findroot(i, j+1, father).x || findroot(i, j, father).y != findroot(i, j+1, father).y)){
                    join(findroot(i, j, father), findroot(i, j+1, father));
                }
                if (i - 1 >= 0 && grid[i-1][j] == '1' && (findroot(i, j, father).x != findroot(i-1, j, father).x || findroot(i, j, father).y != findroot(i-1, j, father).y)){
                    join(findroot(i, j, father), findroot(i-1, j, father));
                }
                if (j - 1 >= 0 && grid[i][j-1] == '1' && (findroot(i, j, father).x != findroot(i, j-1, father).x || findroot(i, j, father).y != findroot(i, j-1, father).y)){
                    join(findroot(i, j, father), findroot(i, j-1, father));
                }
                // for (int ii = 0; ii < grid.length; ++ii){
                //     for (int jj = 0; jj < grid[0].length; ++jj){
                //         System.out.printf("[%d %d] ", father[ii][jj].x, father[ii][jj].y);
                //     }
                //     System.out.println();
                // }
                // System.out.println();
            }
        }
        int group = 0;
        for (int i = 0; i < grid.length; ++i){
            for (int j = 0; j < grid[i].length; ++j){
                if (father[i][j].x == i && father[i][j].y ==j){
                    group += 1;
                }
            }
        }
        return group;
    }
    public static Pair findroot(int i, int j, Pair[][] father){
        //System.out.printf("%d %d\n", i,j);
        while (father[i][j].x != i || father[i][j].y != j){
            int tmp = i;
            i = father[i][j].x;
            j = father[tmp][j].y;
            //System.out.printf("%d %d\n", i,j);
        }
        return father[i][j];
    }
    public static void join(Pair f1, Pair f2) {
        f2.x = f1.x;
        f2.y = f1.y;
    }
}
