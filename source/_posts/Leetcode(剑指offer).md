---
title: 'Leetcode(剑指offer 面试)'
date: 2021/5/16 20:14:35 
tags:
	- Leetcode
---
>剑指 Offer 04 二维数组中查找
>剑指 Offer 12.矩阵中的路径（dfs）
>剑指 Offer 13.机器人的运动范围（dfs）
>剑指 Offer 14-I.剪绳子（dp）
>剑指 Offer 14-II.剪绳子 II（找规律？）
>剑指 Offer 16.数值的整数次方（位运算）
>面试题 (10.03).搜索旋转数组（二分）



<!-- more -->
# 剑指 Offer 04 二维数组中查找

## 题目
https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof
## 题解
https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/xun-zhao-dao-zui-xiao-de-ju-zhen-fan-wei-4l26/
**寻找最小矩阵的行起始位置、行终止位置；列起始位置、列终止位置：**

1.判断每一行最后一个，如果小于target 则更新最小矩阵行起始位置rowl为当前行的下一行
2.判断每一行第一个，如果大于target 则更新最小矩阵行终止位置rowr为当前行的上一行
3.判断每一列最后一个，如果小于target 则更新最小矩阵列起始位置lisl为当前列的下一列
4.判断每一列第一个，如果大于target 则更新最小矩阵列终止位置lisr为当前列的上一列


## 代码
```java
	class Solution {
	    public boolean findNumberIn2DArray(int[][] matrix, int target) {
	        int n = matrix.length;
	        if(n == 0) return false;
	        int m = matrix[0].length;
	        if(m == 0) return false;
	
	        if(target < matrix[0][0] || target > matrix[n - 1][m - 1])
	            return false;
	        else{
	            //存储矩阵的行起始位置、行终止位置；列起始位置、列终止位置
	            int rowl = 0;
	            int rowr = n - 1;
	            int lisl = 0;
	            int lisr = m - 1;
	            //1.判断每一行最后一个，如果小于target 则更新行起始位置rowl为当前行的下一行
	             for(int i = rowl; i < rowr + 1; i++){
	                if(target == matrix[i][m - 1])
	                    return true;
	                if(target > matrix[i][m - 1])
	                    rowl = i + 1;
	             }
	             //2.判断每一行第一个，如果大于target 则更新行终止位置rowr为当前行的上一行
	             for(int i = rowl; i < rowr + 1; i++){
	                if(target == matrix[i][0])
	                    return true;
	                if(target < matrix[i][0])
	                    rowr = i - 1;
	             }
	
	             //3.判断每一列最后一个，如果小于target 则更新列起始位置lisl为当前列的下一列
	             for(int i = lisl; i < lisr + 1; i++){
	                if(target == matrix[n - 1][i])
	                    return true;
	                if(target > matrix[n - 1][i])
	                    lisl = i + 1;
	             }
	             //4.判断每一列第一个，如果大于target 则更新列终止位置lisr为当前列的上一列
	             for(int i = lisl; i < lisr + 1; i++){
	                if(target == matrix[0][i])
	                    return true;
	                if(target < matrix[0][i])
	                    lisr = i - 1;
	             }
	
	
	            //遍历寻找这个矩阵，找到就输出true，找不到就最后输出false
	             for(int i = rowl; i <= rowr; i++){
	                 for(int j = lisl; j <= lisr; j++){
	                     if(target == matrix[i][j])
	                        return true;
	                 }
	             }
	             return false;
	        }
	    }
	}
```


# 剑指 Offer 12.矩阵中的路径

## 题目
https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof
## 题解
boolean[][] used 记录这个位置是否使用过
int x记录当前行
int y记录当前列
temp 记录当前匹配了几个word中的字符
dfs中： 上下左右进行遍历，注意下返回条件和 行列 范围就可以了
## 代码

```java
class Solution {
    boolean result = false;
    int m;
    int n;
    public boolean exist(char[][] board, String word) {
        m = board.length;
        n = board[0].length;
        boolean[][] used = new boolean[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(!result && board[i][j] == word.charAt(0)){
                    dfs(board, used, i, j, 0, word);
                }
            }
        }
        return result;
    }
    public void dfs(char[][] board, boolean[][] used, int x, int y, int temp, String word){
        if(temp == word.length()) {result = true; return;}
        else if(result) return;
        else if(x > -1 && x < m && y > -1 && y < n){
            if(!used[x][y] && board[x][y] == word.charAt(temp)){
                used[x][y] = true;
                dfs(board, used, x - 1, y, temp + 1, word);
                dfs(board, used, x + 1, y, temp + 1, word);
                dfs(board, used, x, y - 1, temp + 1, word);
                dfs(board, used, x, y + 1, temp + 1, word);
                used[x][y] = false;
            }
        }
        else return;
    }
}
```

# 剑指 Offer 13.机器人的运动范围

## 题目
https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof
## 题解
定义一个是否已经走过的boolean数组，dfs进去后，不满足题意的直接返回，满足的并且没走过就把boolean数组更新为走过了，然后再去dfs上下左右四个位置

## 代码

```java
class Solution {
    int result = 0;
    public int movingCount(int m, int n, int k) {
        boolean[][] used = new boolean[m][n];
        dfs(used, 0, 0, m, n, k);
        return result;
    }
    public void dfs(boolean[][] used, int x, int y, int m, int n, int k){
        if(x < 0 || y < 0 || x >= m || y >= n || x / 10 + x % 10 + y / 10 + y % 10 > k) return;
        else if(!used[x][y]){
            result++;
            used[x][y] = true;
            dfs(used, x + 1, y, m, n, k);
            dfs(used, x - 1, y, m, n, k);
            dfs(used, x , y + 1, m, n, k);
            dfs(used, x , y - 1, m, n, k); 
 
        }
    }
}
```
# 剑指 Offer 14-I.剪绳子

## 题目
https://leetcode-cn.com/problems/jian-sheng-zi-lcof
## 题解

**n米长的绳子，他截取后最长的情况只能来自于：**
1.把他截成两段后，每一段分别自己截取的最长结果相乘（举例，12米：截成两段可以是（1，11）、（2、10）、（3，9）、（4，8）、（5，7）、（6，6），这6种截法，每一种截法都有两段，每一段肯定也有会自己的截取后最长相乘结果，因此我们可以计算出来这6种最大的情况）
2.对于第一种情况其实有一些不满足，比如4米的绳子只有（1，3）、（2、2）两种截法，按照1算出最大是第二种截法的result[2] * result[2] = 1，其实应该为2 * 2 = 4,所以第二种情况就是截取成两段直接相乘的情况了

**dp公式：** dp[i] = max(max(dp[i - 1], (i - 1)) * 1, max(dp[i - 2], (i - 2)) * 2, max(dp[i - 3], (i - 3)) * 3,……,) = max(max(dp[i - j], (i - j)) * j)  j取值为（1, i / 2 + 1）

**代码过程：**
1.定义一个 n + 1长度的数组，下标代表为该长度时的最大值，初始化result[1] = 1
2.for从2开始循环到需要计算的结果n, 每一次for表示求取当前下标时的最大值，所以需要第二个for来统计上面我们得到的两种情况(因为result[0]用不到，这里有需要新创建一个变量，所以直接使用result[0]来纪录最大值)
3.for循环结束，返回结果
## 代码

```java
class Solution {
    public int cuttingRope(int n) {
        int[] result = new int[n + 1];
        result[1] = 1;
        for(int i = 2; i <= n; i++){
            for(int j = 1; j < i / 2 + 1; j++){
                result[0] = (i - j >= result[i - j] ? i - j : result[i - j]) * j ;
                result[i] = result[i] >= result[0] ? result[i] : result[0];
            }
        }
        return result[n];
    }
}
```

# 剑指 Offer 14-II.剪绳子 II

## 题目
https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof
## 题解
**找规律？**

    n     乘积     组合
    2       1       1 1
    3       2       2
    4       4       4

    5       6       2 3
    6       9       3 3
    7       12      4 3

    8       18      2 3 3
    9       27      3 3 3
    10      36      4 3 3

    11      54      2 3 3 3
    12      81      3 3 3 3
    13      108     4 3 3 3

    14      162     2 3 3 3 3
    15      243     3 3 3 3 3
    16      324     4 3 3 3 3

    17      486     2 3 3 3 3 3
    18      729     3 3 3 3 3 3
    19      972     4 3 3 3 3 3

    20      1458    2 3 3 3 3 3 3
    21      2187    3 3 3 3 3 3 3
    22      2916    4 3 3 3 3 3 3

    23      4374    2 3 3 3 3 3 3 3
    24      6561    3 3 3 3 3 3 3 3
    25      8748    4 3 3 3 3 3 3 3

    26      13122   2 3 3 3 3 3 3 3 3
    27      19683   3 3 3 3 3 3 3 3 3
    28      26244   4 3 3 3 3 3 3 3 3

    29      39366   2 3 3 3 3 3 3 3 3 3
    30      59049   3 3 3 3 3 3 3 3 3 3
    ……

## 代码

```java
class Solution {
    public int cuttingRope(int n) {
        if(n == 2) return 1;
        if(n == 3) return 2;
        long result = 1;
        while(n > 4){
            n -= 3;
            result *= 3;
            result %= 1000000007;
        }
        return (int)(n * result % 1000000007);
    }
}

```

# 剑指 Offer 16.数值的整数次方

## 题目
https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof
## 题解
1.自己定义一个 pow数组，存的是 x的1次方，2次方，4次方，8次方，16次方……这样可以简便最后的运算
2.通过while求取pow
3.再来一个while 通过位运算计算当前位是否需要×上对应pow数组
4.返回结果
## 代码

```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 1 || n == 0 || (x == -1 && n % 2 == 0)) return 1;
        if(x == -1 && n % 2 == 1) return -1;
        if(x == 0 || n == Integer.MIN_VALUE) return 0;

        if(n < 0){n = -n; x = 1.0 / x;}

        int temp = 1;
        double result = 1;
        double[] pow = new double[32];
        pow[1] = x;

        while(temp < 31 && pow[temp] >= -10000 && pow[temp] <= 10000){
            temp++;
            pow[temp] = pow[temp - 1] * pow[temp - 1];
        }
        int flag = temp;
        temp = 1;
        while(temp < flag){
            if((n & 1) == 1) result *= pow[temp];
            n = n >> 1;   
            temp++;
        }

        return result;

    }
}
```

# 面试题 (10.03).搜索旋转数组

## 题目
https://leetcode-cn.com/problems/search-rotate-array-lcci
## 题解
**共四种情况（请综合我下面给的例子来看后三种情况）**：
**1.左 == target：** 直接返回 左
**2.左 == 中：** 此时target**可能在[左，mid]中**，**也可能在[mid + 1, r]中**，左 = 左 + 1
**3.左 < 中：** 此时需要分情况讨论：
- （1）target比左小**或者**target比中大时（比小的都小**或者**比大的都大）：此时target只可能在[mid, r]中，所以l = mid;
- （2）其他，即target比左大**并且**target比中小时（大小在左和中之间）：此时target只可能在[左 + 1, mid]中，所以 l = l + 1; r = mid;

**4.左 > 中：** 此时需要分情况讨论：
- （1）target比左小**并且**target比中大时（大小在左和中之间）：此时target只可能在[mid, r]中，所以l = mid;
- （2）其他，即target比左大**或者**比中小时（比大的都大**或者**比小的都小）：此时target只可能在[左 + 1, mid]中，所以 l = l + 1; r = mid;

**后三种情况即2.3.4情况例子：**
**2.** [3，1，3，3，3] 或 [3，3，3，3，1]，3 == 3(nums[0] == nums[2]，**左 == 中**)，目标target 1 在[左，mid]中 **或** 在[mid + 1, r]中
**3.** [1，2，3，4，5] 或 [1，2，3，4，0]，1 < 3(nums[0] < nums[2]，**左 < 中**)
- （1）[1，2，3，4，**0**]中目标target 0 或者 [1，2，3，4，**5**]中目标target 5 都在[mid, r]中
- （2）[1，**2**，3，4，0]中目标target 2 只在[左 + 1, mid]中

**4.** [5，6，2，3，4] 或 [5，1，2，3，4]，5 > 2(nums[0] > nums[2]，**左 > 中**)
- （1）[5，6，2，**3**，4]中目标target 3 只在[mid, r]中
- （2）[5，**6**，2，3，4]中目标target 6 或者 [5，**1**，2，3，4]中目标target 1 都只在[左 + 1, mid]中

## 代码

```java
class Solution {
    public int search(int[] arr, int target) {
        int n = arr.length;
        int result = -1;
        int l = 0, r = n - 1;
        while(l <= r){
            int mid = (l + r + 1) >> 1;
            if(arr[l] == target) return l;
            else if(arr[l] == arr[mid]) l++;
            else if(arr[l] < arr[mid]){
                if(arr[l] > target || arr[mid] < target) l = mid;
                else{
                    l = l + 1;
                    r = mid;
                }
            }else{
                if(arr[l] > target && arr[mid] < target) l = mid;
                else{
                    l = l + 1;
                    r = mid;
                }
            }
        }
        return result;
    }
}
```