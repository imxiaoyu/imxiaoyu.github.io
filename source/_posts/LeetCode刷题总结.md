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

**注意：**

`Comparator<T>()`中T必须为类，也就是T必须父类继承自Object，所以:

```java
int[] nums
Arrays.sort(nums, new Comparator<int>(){……});错误，泛型不能是基本数据类型
Arrays.sort(nums, new Comparator<Integer>(){……});错误，nums是int数组
*******************************************************************************
Integer[] nums
Arrays.sort(nums, new Comparator<Integer>(){……});正确，nums格式和泛型匹配
```



## PriorityQueue优先队列

```java
//小顶堆，默认容量为11
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
//大顶堆，容量11
//写法一
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(new Comparator<Integer>(){
    @Override
    public int compare(Integer o1, Integer o2){
        return o2 - o1;
    }
});
//写法二：
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder());
```

# 数据结构

# 二叉树

# 递归

# 动态规划

# dfs

# bfs

# 图论

## 邻接表
	输入：
	1 4 9
	4 3 8
	1 2 5
	2 4 6
	1 3 7

编号|u|v|w|…|first|next
:-:|:-:|:-:|:-:|:-:|:-:|:-:
1|1|4|9|…|5|-1
2|4|3|8|…|4|-1
3|1|2|5|…|-1|1
4|2|4|6|…|2|-1
5|1|3|7|…||3

u[i] - v[i]表示边i，w[i]表示边i的权重，first[u[i]]表示边为u[i] - (…)的最新位置，next[i]表示边为u[i] - (…)的下一位置

```java
//传入坐标i，边a-b，以及权重w，更新邻接表
void add(i, a, b, w){
    u[i] = a;
    v[i] = b;
    w[i] = w;
	next[i] = first[a]; 或 next[i] = first[u[i]];
	first[a] = i; 或 first[u[i]] = i;
    
}    
```

