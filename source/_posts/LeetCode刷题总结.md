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
//默认小顶堆，默认容量为11
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

## [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

### 思路

分治思想，此题是给你二叉树的前序和中序，让你重建二叉树，其实对于给你后序和中序，也是一样的：

- 1.首先是先用**map字典dict把二叉树中序**的每个点对应的位置通过**<key:节点的val, value:节点的位置>**存下来

- 2.因为前序是二叉树的根节点在最前面，所以我们**通过前序得到二叉树根节点前序的位置**
- 3.得到根节点前序位置，从而得到根节点的value，用**字典dict找到根节点中序的位置**(此时也就得到了**根节点左子树的范围和右子树的范围**，也就得到了**左子树和右子树的根节点前序位置)**，所以又能进行3

**注意：**

recur(int root, int left, int right)

node.left = recur(root + 1, left, posi - 1) 左孩子根节点前序位置：root + 1

 node.right = recur(root + posi - left + 1, posi + 1, right) **右孩子根节点前序位置**：root + posi - left  + 1，这里其实是root + 左子树长度(posi - left) + 1

### 代码

```java
//前序 + 中序 -> 重建二叉树
class Solution {
    int[] pre;
    Map<Integer, Integer> dict = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        this.pre = preorder;
        for(int i = 0; i < inorder.length; i++){//1.通过字典存下中序节点以及其中序数组对应位置 
            dict.put(inorder[i], i);
        }
        return recur(0, 0, inorder.length - 1);//2.最初根节点肯定是前序0位置，然后对应的中序数组是[0, inorder.length - 1]
    }
    public TreeNode recur(int root, int left, int right){
        if(left > right) return null;//3.上一步节点没有这个孩子，返回null
        TreeNode node = new TreeNode(pre[root]);//创建这个节点
        int posi = dict.get(pre[root]);//得到根节点对应的中序位置
        node.left = recur(root + 1, left, posi - 1);//根节点左孩子(左孩子根节点前序位置root + 1,左孩子对应中序位置[left, posi - 1])
        node.right = recur(root + posi - left + 1, posi + 1, right);//根节点右孩子(右孩子根节点前序位置root + posi + 1 - left,右孩子对应中序位置[posi + 1, right])
        return node;
    }
}
```

```java
//后序 + 中序 -> 重建二叉树
class Solution {
    int[] post;
    Map<Integer, Integer> dict = new HashMap<>();
    public TreeNode buildTree(int[] postorder, int[] inorder) {
        this.post = postorder;
        for(int i = 0; i < inorder.length; i++){//1.通过字典存下中序节点以及其中序数组对应位置 
            dict.put(inorder[i], i);
        }
        return recur(postorder.length - 1, 0, inorder.length - 1);//2.最初根节点肯定是后序最后位置，然后对应的中序数组是[0, inorder.length - 1]
    }
    public TreeNode recur(int root, int left, int right){
        if(left > right) return null;//3.上一步节点没有这个孩子，返回null
        TreeNode node = new TreeNode(post[root]);//创建这个节点
        int posi = dict.get(pre[root]);//得到根节点对应的中序位置
        node.left = recur(root - right + posi - 1, left, posi - 1);//根节点左孩子
        node.right = recur(root - 1, posi + 1, right);//根节点右孩子
        return node;
    }
}
```

# 分治

## 排序

### 快排

```java
class Solution {
    public void sort(int[] nums) {
        qSort(0, 0, nums.length - 1);
    }
    public void qSort(int[] nums, int start, int end){
        if(left < right){
            int base = nums[start];
            int left = start;
            int right = end + 1;
            while(true){
                while(left < end && nums[++left] <= base) ;
                while(right > start && nums[--right] >= base) ;
                if(left < right) swap(nums, left, right);
                else break;
            }
            swap(nums, start, right);
            qSort(nums, start, right - 1);
            qSort(nums, right + 1, end);
        }
    }
    public void swap(int[] nums, int a, int b){
        int c = nums[a];
        nums[a] = nums[b];
        nums[b] = c;
    }
}
```



### 归并排序

# 字典树

## [720. 词典中最长的单词](https://leetcode-cn.com/problems/longest-word-in-dictionary/)



代码

```java
class Solution {
    static int N = 30010, M = 26;
    static int[][] tr = new int[N][M];
    static boolean[] isEnd = new boolean[N];
    static int idx = 0;
    void add(String s) {
        int p = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            int u = s.charAt(i) - 'a';
            if (tr[p][u] == 0) tr[p][u] = ++idx;
            p = tr[p][u];
        }
        isEnd[p] = true;
    }
    boolean query(String s) {
        int p = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            int u = s.charAt(i) - 'a';
            p = tr[p][u];
            if (!isEnd[p]) return false;
        }
        return true;
    }
    public String longestWord(String[] words) {
        Arrays.fill(isEnd, false);
        for (int i = 0; i <= idx; i++) Arrays.fill(tr[i], 0);
        idx = 0;

        String ans = "";
        for (String s : words) add(s);
        for (String s : words) {
            int n = s.length(), m = ans.length();
            if (n < m) continue;
            if (n == m && s.compareTo(ans) > 0) continue;
            if (query(s)) ans = s;
        }
        return ans;
    }
}
```



# 递归

# 图论

## 邻接表

### [2039. 网络空闲的时刻](https://leetcode-cn.com/problems/the-time-when-the-network-becomes-idle/)

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

# 动态规划

# dfs

# bfs
