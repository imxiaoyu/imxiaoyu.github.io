---
title: LeetCode刷题总结
date: 2022-03-11 23:31:05
tags:
	- JAVA
	- Leetcode
---

LeetCode上刷题知识点

<!-- more -->

# 常用方法
## 自定义排序

```java
//二维数组，根据数组nums[i][0]位置,从小到大排序
int[][] nums;
Arrays.sort(nums, new Comparator<int[]>(){
    @Override
    public int compare(int[] o1, int[] o2){
        return o1[0] - o2[0];
    }
});
```


## 优先队列

```java
//小顶堆，默认容量为11
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
//大顶堆，容量11
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(new Comparator<Integer>(){
    @Override
    public int compare(Integer o1, Integer o2){
        return o1 - o2;
    }
});
```

# 数据结构

# 二叉树

# 递归

# 动态规划

# dfs

# bfs

# 图论