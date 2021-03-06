---
title: 'Leetcode(mid100-)'
date: 2021/7/2 22:52:51 
tags:
	- Leetcode
---
>**题目列表：**
>102.二叉树的层序遍历(BFS)
>113.路径总和 II(DFS)
>151.翻转字符串里的单词(StringBuffer模拟)
>153.寻找旋转排序数组中的最小值（二分）
>198.打家劫舍（dp）
>213.打家劫舍II（dp）
>236.二叉树的最近公共祖先（二叉树、递归）
>240.搜索二维矩阵 II（优化搜索）
>264.丑数 II(数学规律推导)
>274.H 指数（规律+数组）
>275.H 指数 II（规律+数组）
>279.完全平方数(dp)
>322.零钱兑换(dp)
>337.打家劫舍III(需要多写几次**)
>343.整数拆分（dp）
>400.第 N 位数字（找规律）
>421.数组中两个数的最大异或值(字典树)
>474.一和零（dp）
>477.汉明距离总和（异或运算）
>494.目标和(dp或dfs)
>518.零钱兑换 II(dp)
>523.连续的子数组和(前缀和)
>525.连续数组（前缀和）
>692.前K个高频单词（暴力排序，或者Comparator接口）
>740.删除并获得点数（dp）
>752.打开转盘锁（bfs）
>877.石子游戏（dp或脑筋急转弯？）
>946.验证栈序列（栈）
>990.等式方程的可满足性（并查集）
>1035.不相交的线(最长公共子序列 经典dp 模板)
>1049.最后一块石头的重量 II（dp背包）
>1190.反转每对括号间的子串（栈，反转字符串）
>1239.串联字符串的最大长度（dfs）
>1310.子数组异或查询（异或运用）
>1442.形成两个异或相等数组的三元组数目（优化+异或+前缀和）
>1482.制作 m 束花所需的最少天数（二分）
>1600.皇位继承顺序（模拟）
>1679.K 和数对的最大数目（HashMap）
>1734.解码异或后的排列（异或规律）
>1738.找出第 K 大的异或坐标值（二维前缀和）
>1744.你能在你最喜欢的那天吃到你最喜欢的糖果吗？（前缀和）
>1828.统计一个圆中点的数目（暴力？）
>1833.雪糕的最大数量（排序？）
>1860.增长的内存泄露（模拟）
>1861.旋转盒子（模拟）
>1864.构成交替字符串需要的最小交换次数（统计+规律）
>1865.找出和为指定值的下标对（HashMap运用）
>1877.数组中最大数对和的最小值（数组）
>1878.矩阵中最大的三个菱形和(模拟)
>1881.插入后的最大值（模拟）
>1882.使用服务器处理任务(优先队列)
>1894.找到需要补充粉笔的学生编号（前缀和）
>1895.最大的幻方(前缀和)
>1910.删除一个字符串中所有出现的给定子字符串（模拟）
>1911.最大子序列交替和（模拟，本质就是dp）



909.蛇梯棋 + 
53道题目
<!-- more -->
# 102.二叉树的层序遍历
## 题目
https://leetcode-cn.com/problems/binary-tree-level-order-traversal/
## 题解
- 1.定义一个**当前层的List tempDeep存放非空的根节点root**
- 2.tempDeep非空时**进入第一层while**，创建一个下一层的List nextDeep 和 当前层的val结果List
- 3.tempDeep非空时**进入第二层while**，把当前层tempDeep中每一个节点的val放入一个Integer的List中，然后他的非空左右孩子按顺序放到下一层nextDeep中，删除遍历完的节点
- 4.当tempDeep判断完一遍后也就是tempDeep变为空**退出第二层while**，然后更新当前层为下一层即tempDeep = nextDeep，把存放val的List放入结果result中
- 5.tempDeep非空时又**进入第二层while（也就是第3步）**
- 6.**tempDeep为空**了，第一层while也结束，返回结果result

## 代码

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if(root == null) return result; 
        List<TreeNode> tempDeep = new ArrayList<>();
        tempDeep.add(root);
        while(tempDeep.size() != 0){
            List<TreeNode> nextDeep = new ArrayList<>();
            List<Integer> ans = new ArrayList<>();
            while(tempDeep.size() != 0){
                TreeNode tempNode = tempDeep.get(0);
                tempDeep.remove(0);
                ans.add(tempNode.val);
                if(tempNode.left != null) nextDeep.add(tempNode.left);
                if(tempNode.right != null) nextDeep.add(tempNode.right);          
            }
            result.add(ans);
            tempDeep = nextDeep; 
        }
        return result;  
    }
}
```

# 113.路径总和 II
## 题目
https://leetcode-cn.com/problems/path-sum-ii/
## 题解
搜索左右子树：直到叶子节点而且当前的和等于目标target时把当前路径加到结果中

## 代码

```java
class Solution {
     List<List<Integer>> result = new ArrayList<List<Integer>>();
    public List<List<Integer>> pathSum(TreeNode root, int target) {
        dfs(root, target, 0, new ArrayList<Integer>());
        return result;
    }
    public void dfs(TreeNode root, int target, int sum, List<Integer> tempAns){
        if(root == null) return;
        else {
            tempAns.add(root.val);
            if(root.left == null && root.right == null && sum + root.val == target)
                result.add(new ArrayList<Integer>(tempAns));
            else {
                dfs(root.left, target, sum + root.val, tempAns);
                dfs(root.right, target, sum + root.val, tempAns);
            }
            tempAns.remove(tempAns.size() - 1); 
        }
    }
}
```

# 151.翻转字符串里的单词
## 题目
https://leetcode-cn.com/problems/reverse-words-in-a-string/
## 题解
定义两个String Buffer，一个存放结果，一个存放当前的单词，然后倒叙判断，遇到空格就把当前单词放在结果里，最终要判断一下当前的单词是否为空（因为String s开头可能没有空格）

## 代码

```java
class Solution {
    public String reverseWords(String s) {
        boolean flag = false;
        StringBuffer result = new StringBuffer();
        StringBuffer tempWord = new StringBuffer();
        int tempPosi = s.length() - 1;
        while(tempPosi >= 0){
            char c = s.charAt(tempPosi);
            if(tempWord.length() != 0 && c == ' '){
                if(!flag) {result.append(tempWord); flag = true;}
                else result.append(" " + tempWord);
                tempWord.delete(0, tempWord.length());
            }
            else if(c != ' ') tempWord.insert(0, c);
            tempPosi--;
        }
        if(tempWord.length() != 0){
            if(!flag) result.append(tempWord);
            else result.append(" " + tempWord);
        }
        return result.toString();
        
    }
}
```

# 153.寻找旋转排序数组中的最小值
## 题目
https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/
## 题解
每次对比都是**左侧元素**和**中间元素**对比，**共两种情况**：
**1.左 < 中：**此时最小值**仅可能**是**左或在[mid + 1，r]中**，更新一下结果 ，左 = mid + 1
**2.左 > 中：**此时最小值**仅可能**是**中或在[l，mid - 1]中**，更新一下结果 ，右 = mid - 1

**两种情况例子：**
**1.**[**1**，2，3，4，5]或[1，2，3，4，**0**]，1 < 3(nums[0] < nums[2])，最小值1是左 **或** 最小值0在[mid + 1，r]中
**2.**[3，4，**2**，3，3]或[3，**1**，2，3，3]，3 > 2(nums[0] > nums[2])，最小值2是中 **或** 最小值1在在[l，mid - 1]中

## 代码

```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int result = nums[0];
        int l = 0, r = n - 1;
        while(l <= r){
            int mid = (l + r + 1) >> 1;
            if(nums[l] < nums[mid]){
                result = Math.min(result, nums[l]); 
                l = mid  + 1;
            }else{
                result = Math.min(result, nums[mid]);
                r = mid  - 1;
            }
        }
        return result;
    }
}
```

# 198.打家劫舍

## 题目
https://leetcode-cn.com/problems/house-robber


## 题解

>跟740类似

**最优子结构的公式：**
>dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);

**注意：**
>1.当数组长度为1时:返回nums[0];
>2.当数组长度为2时:返回Math.max(nums[0],nums[1]);
>3.当数组长度大于3时:结构为上式。

## 代码

```java
	class Solution {
	    public int rob(int[] nums) {
	        int numLength = nums.length;
	        int[] dp = new int[numLength];

	        if(numLength == 1){return nums[0];}

	        else if(numLength == 2){return Math.max(nums[0],nums[1]);}

	        else{
	            dp[0] = nums[0];
	            dp[1] = Math.max(nums[0],nums[1]);

				//dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
	            for(int i = 2; i < numLength; i++) {
	                dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
	            }
	            return dp[numLength - 1];
	        }
	    }
	}
```

# 213.打家劫舍II

## 题目
https://leetcode-cn.com/problems/house-robber-ii

## 题解

>**跟上题类似，但是可以分别去掉首部和尾部，然后判断两个的大小，返回大的。**


**最优子结构的公式：**
>dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);

**注意：**
>1.当数组长度为1时:返回nums[0];
>2.当数组长度为2时:返回Math.max(nums[0], nums[1]);
>3.当数组长度为3时:返回Math.max(Math.max(nums[0], nums[1]), nums[2]);
>4.当数组长度大于4时:去掉首部和尾部分为两部分进行求解。

## 代码

```java
	class Solution2 {
	    public int rob(int[] nums) {
	        int numLength = nums.length;
	        int[] dp = new int[numLength + 1];
	        if(numLength == 1){return nums[0];}
	        else if(numLength == 2){return Math.max(nums[0],nums[1]);}
	        else if(numLength == 3){return Math.max(Math.max(nums[0],nums[1]),nums[2]);}
	        else{
	
	            //去掉第一个房间，判断剩下的
	            dp[2] = nums[1];
	            dp[3] = Math.max(nums[1],nums[2]);
	            //dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
	            for(int i = 4; i < numLength + 1; i++) {
	                dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i - 1]);
	            }
	
	            //去掉最后一个房间，判断剩下的
	            dp[1] = nums[0];
	            dp[2] = Math.max(nums[0],nums[1]);
	            for(int i = 3; i < numLength; i++) {
	                dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i - 1]);
	            }
	
	            return Math.max(dp[numLength - 1], dp[numLength]);
	        }
	    }
	}
```

# 236.二叉树的最近公共祖先
## 题目
https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/
## 题解
对于每一个节点root，去寻找他的左孩子和右孩子：
- 1.如果找到了p和q说明本节点就是最近的公共祖先
- 2.如果找不到则说明p和q在root的左子树或者右子树里，则去找左子树或右子树
## 代码

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        if (root == p || root == q) return root;
        
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if (left != null && right != null) return root;
        if (left != null) return left;
        if (right != null) return right;

        return null;
    }
}
```

# 240.搜索二维矩阵 II
## 题目
https://leetcode-cn.com/problems/search-a-2d-matrix-ii/
## 题解
**寻找最小矩阵的行起始位置、行终止位置；列起始位置、列终止位置：**
- 1.判断每一行最后一个，如果小于target 则更新最小矩阵行起始位置rowl为当前行的下一行
- 2.判断每一行第一个，如果大于target 则更新最小矩阵行终止位置rowr为当前行的上一行
- 3.判断每一列最后一个，如果小于target 则更新最小矩阵列起始位置lisl为当前列的下一列
- 4.判断每一列第一个，如果大于target 则更新最小矩阵列终止位置lisr为当前列的上一列

## 代码

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
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

# 264.丑数 II
## 题目
https://leetcode-cn.com/problems/ugly-number-ii/
## 题解
因为后面的丑数是前面丑数×2、×3、×5得到的，所以记录一下，接下来要去×的数，遍历一遍即可

## 代码

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] result = new int[n];
        result[0] = 1;
        int two = 0, three = 0, five = 0;
        for (int i = 1; i < n; i++) {
            result[i] = Math.min(2 * result[two], Math.min(3 * result[three], 5 * result[five]));
 
            if (2 * result[two] == result[i]) two++;
            if (3 * result[three] == result[i]) three++;
            if (5 * result[five] == result[i]) five++;      
        }
        return result[n - 1];
    }
}
```

# 274.H 指数

## 题目
https://leetcode-cn.com/problems/h-index/
## 题解
从小到大排序一下，然后数组倒着遍历，遍历的同时有一个结果值`result`每循环一次`＋1`，直到结果值大于等于当前位置的数组值结束，也就是`result >= citations[i]`

## 代码

```java
class Solution {
    public int hIndex(int[] citations) {
        Arrays.sort(citations);
        int result = 0;
        for(int i = citations.length - 1; i >= 0; i--){
            if(result >= citations[i]) break;
            else result++;
        }
        return result;
    }
}
```

# 275.H 指数 II

## 题目
https://leetcode-cn.com/problems/h-index-ii/
## 题解
数组倒着遍历，遍历的同时有一个结果值`result`每循环一次`＋1`，直到结果值大于等于当前位置的数组值结束，也就是`result >= citations[i]`

## 代码

```java
class Solution {
    public int hIndex(int[] citations) {
        int result = 0;
        for(int i = citations.length - 1; i >= 0; i--){
            if(result >= citations[i]) break;
            else result++;
        }
        return result;
    }
}
```

# 279.完全平方数

## 题目
https://leetcode-cn.com/problems/perfect-squares/
## 题解
dp[i]表示 正整数i 需要完全平方数的最少的个数
两层for：一层遍历正整数i,另一层遍历各种小于i的完全平方数
## 代码

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            dp[i] = i;
            for (int j = 2; i - j * j >= 0; j++) { 
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
}
```
**有点麻烦的dp：**先把n里面的完全平方存下来，然后遍历小于i的每一个数字
```java
class Solution {
    public int numSquares(int n) {
        int len = (int)Math.sqrt(n);
        if(len * len == n) return 1;
        else if(Math.abs(len * len - n) == 1 || Math.abs(len * len - n) == 4) return 2;
        int[] dp = new int[n + 1];
        for(int i = 1; i <= len; i++) dp[i * i] = 1; 

        for(int i = 1; i <= n; i++){
            for(int j = 1; j < i; j++){
                if(dp[i] == 0)  dp[i] = dp[i - j] + dp[j];
                else if(dp[j] == 0) ;
                else dp[i] = Math.min(dp[i], dp[i - j] + dp[j]);
            }
        }
        return dp[n];
    }
}
```

# 322.零钱兑换

## 题目
https://leetcode-cn.com/problems/coin-change/solution/dp-by-rain-ru-xlm6/
## 题解
dp[i]：表示金额i时凑成金额i所需的最少的硬币个数
两层for循环：一重遍历不同面额的硬币、一重计算截止当前面额的硬币时所需的最少的硬币个数

## 代码

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(amount == 0) return 0;
        int[] dp = new int[amount + 1];
        for(int i = 0; i < coins.length; i++){
            if(coins[i] > amount) continue;
            else dp[coins[i]] = 1;
            for(int j = coins[i] + 1; j <= amount; j++){
                if(dp[coins[i]] != 0 && dp[j - coins[i]] != 0){
                    if(dp[j] == 0) dp[j] = dp[coins[i]] + dp[j - coins[i]];
                    else dp[j] = Math.min(dp[j], dp[coins[i]] + dp[j - coins[i]]);
                }
            }
        }
        return dp[amount] == 0 ? -1 : dp[amount];
    }
}
```

# 337.打家劫舍III(需要多写几次) **

## 题目
https://leetcode-cn.com/problems/house-robber-iii
二叉树状房间
## 题解

>**简化一下这个问题：**一棵二叉树，树上的每个点都有对应的权值，每个点有两种状态（选中和不选中），问在不能同时选中有父子关系的点的情况下，能选中的点的最大权值和是多少。

>我们可以用 f(o)f(o) 表示选择 o 节点的情况下，o 节点的子树上被选择的节点的最大权值和；g(o)g(o) 表示不选择 o 节点的情况下，o 节点的子树上被选择的节点的最大权值和；l 和 r 代表 o 的左右孩子。

>**1.**当 o 被选中时，o 的左右孩子都不能被选中，故 o 被选中情况下子树上被选中点的最大权值和为 l 和 r 不被选中的最大权值和相加，即 f(o) = g(l) + g(r)f(o)=g(l)+g(r)。
>**2.**当 o 不被选中时，o 的左右孩子可以被选中，也可以不被选中。对于 o 的某个具体的孩子 xx，它对 o 的贡献是 xx 被选中和不被选中情况下权值和的较大值。故 g(o) = \max \{ f(l) , g(l)\}+\max\{ f(r) , g(r) \}g(o)=max{f(l),g(l)}+max{f(r),g(r)}。


>至此，我们可以用哈希表来存 f 和 g 的函数值，用深度优先搜索的办法后序遍历这棵二叉树，我们就可以得到每一个节点的 f 和 g。根节点的 f 和 g 的最大值就是我们要找的答案。


## 代码

```java
	class Solution {
    Map<TreeNode, Integer> select = new HashMap<>();
    Map<TreeNode, Integer> refuse = new HashMap<>();
    public int rob(TreeNode root) {
        dfs(root);
        return Math.max(select.get(root),refuse.get(root));
    }
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        dfs(root.right);
        select.put(root, root.val + refuse.getOrDefault(root.left, 0) + refuse.getOrDefault(root.right, 0));
        refuse.put(root, Math.max(select.getOrDefault(root.left, 0), refuse.getOrDefault(root.left, 0)) + Math.max(select.getOrDefault(root.right, 0), refuse.getOrDefault(root.right, 0)));
    }
}
```
用数组优化后：
```java
	class Solution {
	    public int rob(TreeNode root) {
	    int[] result = dp(root);
	    return Math.max(result[0], result[1]);
	    }
	
	    public int[] dp(TreeNode root){
	        if(root == null) return new int[]{0,0};
	    
	        int[] left  = dp(root.left);
	        int[] right = dp(root.right);
	
	        int rob     = root.val + left[0] + right[0];
	        int not_rob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]) ;
	
	        return new int[]{not_rob, rob};
	    }
```


# 343.整数拆分
## 题目
https://leetcode-cn.com/problems/integer-break/
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
    public int integerBreak(int n) {
        int[] nums = new int[n + 1];
        nums[1] = 1;
        for(int i = 2; i <= n; i++){
            for(int j = 1; j < i / 2 + 1; j++){
                nums[0] = (i - j >= nums[i - j] ? i - j : nums[i - j]) * j ;
                nums[i] = nums[i] >= nums[0] ? nums[i] : nums[0];
            }
        }
        return nums[n];
    }
}
```

# 400.第 N 位数字
## 题目
https://leetcode-cn.com/problems/nth-digit/
## 题解
数字位数|数字范围|字符序列范围
:-:|:-:|:-:
1|0 - 9|0 - 9
2|10 - 99|10 - 189
3|100 - 999|190 - 2889
4|1000 - 9999|2890 - 38889
5|10000 - 99999|38890 - 488889
6|100000 - 999999|488890 - 5888889
7|1000000 - 9999999|5888890 - 68888889
8|10000000 - 99999999|68888890 - 788888889
9|100000000 - 999999999|788888890 - 8888888889

根据上表我们就能求出题目给的n是哪个数字的第几位，那就结束了……
**举例：** n = 13  (01234567891011) 答案按说是1，其实就是数字11的个位数
- 1.先求n对应的是哪个数字：`num =  (13 - 10) / 2 + 10 = 11`
- 2.再求是这个数字的哪一位 `posi = (13 - 10) % 2 = 1` (结果为0就是最高位，1是次一位，对于11这个两位数来说1表示的就是个位)
- 3.求出这个数字的这一位并返回

## 代码

```java
class Solution {
    public int findNthDigit(int n) {
        int[] ant = new int[]{0, 10, 190, 2890, 38890, 488890, 5888890, 68888890, 788888890};
        int[] numBegin = new int[]{0, 10, 100, 1000, 10000, 100000, 1000000, 10000000, 100000000};
        for(int i = ant.length - 1; i >= 0; i--){
            if(n >= ant[i]){
                int num = (n - ant[i]) / (i + 1) + numBegin[i];
                int posi = (n - ant[i]) % (i + 1);
                return num % (int)Math.pow(10, i + 1 - posi) / (int)Math.pow(10,  i - posi);
            }
        }
        return 0;
    }
}
```

# 421.数组中两个数的最大异或值
## 题目
https://leetcode-cn.com/problems/maximum-xor-of-two-numbers-in-an-array

## 题解
暴力属于钻了漏洞，还是说下**字典树的思路**吧：
**第一步：** 把每个数的二进制取出来，存在32深度的树当中（比如15，对应二进制0000 0000 0000 1111，那么32深度的树就会从15的二进制从左到右存，即先存0，再0，再0……一直到1），每个数存完了，现在我们的字典树其实就可以还原出每一个数字了，
**第二步：** 我们去找每个数字异或的最大值，也就是从高位开始，如果和这个数字的当前位二进制不同的字典树存在我们就进去并加上此位置异或的结果，因为此时异或肯定比相同的时候大，一直这样下去，就可以找到最大异或值了。


**例子：** 数组中为 15，1，2
**第一步：** 存三个数
**第二步：** 
- 1.找15（二进制后四位1111）的异或最大，从高位开始，
- 2.最高位0，字典树中最高位1的树不存在,所以继续15的下一位，和字典树下一位；
- 3.第二位还是0，字典树中还不存在，继续下一位……；
- 4.到了1这一位了，字典树中这一位时0存在，进入0，结果ans += 8（此时位数对应的数） = 8，继续还是1，字典树中这一位时0存在，进入0，结果ans += 4（此时位数对应的数） = 12；
- 5.继续，此时还是1，字典树中这一位呢（0，1都存在，因为2的二进制最后四位0010，1的二进制最后四位0001，所以倒数第二位的字典树中0和1均存在），进入0的（肯定不同时异或结果最大），结果ans += 2（此时位数对应的数） = 14；
- 6.继续，此时还是1，字典树中这一位呢？只有1存在，（你可能问数字2的最后一位不是0吗？因为上一步中我们选择了进入0的字典树，所以就进入了数字1这个支路，此时这个支路中没有数字2）所以最终结果 14；


## 代码
**java暴力：**
```java
class Solution {
    public int findMaximumXOR(int[] nums) {
        int result = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i++)
            for(int j = i; j < nums.length; j++)
                result = Math.max(nums[i] ^ nums[j], result);
        return result;
    }
}
```

**字典树：**
```java
class Solution {
    int tempBit;
    int result = 0;
    Tree start = new Tree();
    public class Tree{
        public Tree[] next = new Tree[2];
    }
    public void init(int num){
        Tree treeBits = start;
        for(int i = 31 ; i >= 0; i--){
            tempBit = num >> i & 1;
            if(treeBits.next[tempBit] == null){
                treeBits.next[tempBit] = new Tree();
            }
            treeBits = treeBits.next[tempBit];
        }
    }
    public void searchMax(int num){
        int sum = 0;
        Tree treeBits = start;
        for(int i = 31 ; i >= 0; i--){
            tempBit = num >> i & 1;
            if(treeBits.next[1 - tempBit] != null){
                sum += 1 << i;
                treeBits = treeBits.next[1 - tempBit];
            }
            else{     
                treeBits = treeBits.next[tempBit];
            }
        }
        result = Math.max(result, sum);
    }
    public int findMaximumXOR(int[] nums) {
        for(int num : nums){
            init(num);
            searchMax(num);
        } 
        return result;
    }
}
```

# 474.一和零
## 题目
https://leetcode-cn.com/problems/ones-and-zeroes/
## 题解
dp[i][j] 表示i个0、j个1的时候最多可以有几个字符串：
遍历一下字符串数组，然后倒着判断截止到当前字符串的时候dp[0 - m][0 - n]最多可以放几个字符串，遍历结束后输出一下dp[m][n]（倒着遍历是为了避免重复计算，因为计算dp数组较大位置时需要用上一个字符串计算出的小位置结果，如果先算小位置，再算大位置就可能造成大位置用的是本轮字符串计算得到的结果）

## 代码

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for(String s : strs){
            int zeroNum = 0, oneNum = 0;
            for(int pos = s.length() - 1; pos >= 0 ; pos--){
                if(s.charAt(pos) == '0') zeroNum++;
                else oneNum++;
            }
            for(int zero = m; zero >= zeroNum; zero--){
                for(int one = n; one >= oneNum; one--){
                    dp[zero][one] = Math.max(dp[zero][one], dp[zero - zeroNum][one - oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
}
```

# 477.汉明距离总和
## 题目
https://leetcode-cn.com/problems/total-hamming-distance/
## 题解
统计一下 **数组中每一个数的 同一位的二进制0或1的个数** ，这样**对于这一位来说他的汉明距离和就是0的个数 × 1的个数**，一共32位那么循环一下加和就好了。

## 代码

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int result = 0;
        int zero = 0;
        for(int i = 0; i < 32; i++){
            zero = 0;
            for(int j = 0; j < nums.length; j++) if((nums[j] >> i & 1) == 1) zero++;
            result += (nums.length - zero) * zero;
        }
        return result;
    }
}
```

# 494.目标和
## 题目
https://leetcode-cn.com/problems/target-sum/
## 题解
**dfs:**
dfs就对于数组每一位置就两种状态，加或者减，最后判断到了数组长度位置的时候和是不是等于target就好了
**dp:**
定义一个结果HashMap,key存放的是和，value存放到这个和一共有多少种不同的表达式：
遍历一遍数组长度，创建一个临时temp的HashMap，复制result到temp，然后计算截止到当前数组长度时，所有不同的和一共有多少表达式，最后返回result.getOrDefault(target, 0);

## 代码
**dfs:**
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        return dfs(nums, target, 0, 0);
    }
    int dfs(int[] nums, int target, int posi, int sum) {
        if (posi == nums.length) return sum == target ? 1 : 0;
        return dfs(nums, target, posi + 1, sum + nums[posi]) + dfs(nums, target, posi + 1, sum - nums[posi]);
    }
}
```
**dp:**
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        Map<Integer, Integer> result = new HashMap<>();
        result.put(nums[0], 1);
        result.put(-nums[0], result.getOrDefault(-nums[0], 0) + 1);
        for(int i = 1; i < nums.length; i++){
            Map<Integer, Integer> temp = new HashMap<>(result);
            result.clear();
            for (Map.Entry<Integer, Integer> entry : temp.entrySet()) {
                result.put(entry.getKey() + nums[i], entry.getValue() + result.getOrDefault(entry.getKey() + nums[i], 0));
                result.put(entry.getKey() - nums[i], entry.getValue() + result.getOrDefault(entry.getKey() - nums[i], 0));  
            }
        }
        return result.getOrDefault(target, 0);
    }
}
```


# 518.零钱兑换 II
## 题目
https://leetcode-cn.com/problems/continuous-subarray-sum/

## 题解
dp[i] 表示总金额为i时可以有多少组合数
两个for：一重遍历不同面额的硬币、一重计算截止当前面额的硬币时不同金额可以有多少组合数
## 代码

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for(int i = 0; i < coins.length; i++){
            if(coins[i] > amount) continue;
            for(int j = coins[i]; j <= amount; j++){
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
}
```

# 523.连续的子数组和
## 题目
https://leetcode-cn.com/problems/continuous-subarray-sum/

## 题解
用原数组存一下前缀和，然后两层for判断一下是否是k的倍数：
1.第一层for：
- 如果前i(i > 0)个数的和是k的倍数**或者**第i(i > 1)个数与第i - 1个数的和为0（因为0也是k的倍数）直接返回true

2.第二层for：
- 如果[j,i]区间内原数组元素的和比k都小了，直接结束第二个for（因为原数组是为非负数，所以第二个for里面后面的[j,i]的和肯定也比k小，即不是k的倍数，没必要继续在第二层for里面判断了）
- 如果[j,i]区间内原数组元素的和是k的倍数，直接返回true

## 代码

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        if(nums.length < 2) return false;
        for(int i = 1; i < nums.length; i++) nums[i] += nums[i - 1];
        for(int i = 1; i < nums.length; i++){
            if(nums[i] % k == 0 || (i > 1 && nums[i] - nums[i - 2] == 0)) return true;
            for(int j = 0; j < i - 1; j++){   
                if(nums[i] - nums[j] < k) break; 
                if((nums[i] - nums[j]) % k == 0) return true;  
            }
        }
        return false;
    }
}
```

# 525.连续数组
## 题目
https://leetcode-cn.com/problems/contiguous-array/

## 题解
创建一个哈希表ant，key存放[0,i]的前缀和，value存放下标i（对于重复的前缀和只存放第一个前缀和的下标，**注意**求前缀和时我们要把0变为-1，这样能够和1相加变为0，便于运算）,遍历一遍数组进行前缀和计算与判断，过程为：
**1.先求前缀和：0变为-1，1还是1**
**2.判断截止到i为止的最长连续子数组，共三种情况：**
- **①当前的前缀和 nums[i]== 0：** 说明当前最长连续子数组为[0,i],长度为i + 1
- **②当前的前缀和在哈希表里存在：**（也就是与之前在哈希表存的前缀和相同了），那说明左开右闭区间`(ant.get(nums[i]),i]` 内0和1数量也是相同的（只有这样前缀和才会相同），所以我们让`result = Math.max(result, i - ant.get(nums[i]));`
- **③当前的前缀和在哈希表里不存在：** 那么把它存放到哈希表里

最后返回result
## 代码

```java
class Solution {
    public int findMaxLength(int[] nums) {
        int result = 0;
        Map<Integer,Integer> ant = new HashMap<>();
        nums[0] = nums[0] == 1 ? 1 : -1;
        ant.put(nums[0], 0);
        for(int i = 1; i < nums.length; i++) {
            if(nums[i] == 1) nums[i] += nums[i - 1];
            else nums[i] = nums[i - 1] - 1;
            if(nums[i] == 0) result = i + 1;
            else if(ant.containsKey(nums[i])) result = Math.max(result, i - ant.get(nums[i]));
            else ant.put(nums[i], i);
        }
        return result;
    }
}
```

# 692.前K个高频单词
## 题目
https://leetcode-cn.com/problems/top-k-frequent-words/


## 题解
**一、暴力求法：**
- 1、创建一个HashMap用来存取每个单词出现的次数
- 2、遍历HashMap k次找寻出现次数最多的单词（如果多个一样就取字母顺序排序），找到单词后存在result队列中
- 3、返回结果

**二、Comparator接口求法：**

- 1、创建一个HashMap用来存取每个单词出现的次数
- 2、直接把HashMap中求的单词存在result集合中
- 3、使用comparator接口完成排序
- 4、返回结果

## 代码
**一、暴力求解：**
```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String,Integer> word = new HashMap<>();
        for (String s : words) word.put(s, word.getOrDefault(s, 0) + 1);   
        List<String> result = new ArrayList<>();
        StringBuffer tempMaxString = new StringBuffer();
        int tempMaxNum = 0;
        for(int i = 0; i < k; i++){
            tempMaxNum = 0;
            for(Map.Entry<String, Integer> entry : word.entrySet()){
                if(tempMaxNum < entry.getValue()){
                    tempMaxNum = entry.getValue();
                    tempMaxString.delete(0, tempMaxString.length());
                    tempMaxString.append(entry.getKey());
                }
                else if(tempMaxNum == entry.getValue()){
                    if(tempMaxString.toString().compareTo(entry.getKey()) > 0){
                        tempMaxNum = entry.getValue();
                        tempMaxString.delete(0, tempMaxString.length());
                        tempMaxString.append(entry.getKey());
                    }
                }
            }
            word.remove(tempMaxString.toString());
            result.add(tempMaxString.toString());
        }
        return result;
    }
}
```
**二、Comparator接口：**
```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> word = new HashMap<String, Integer>();
        for (String s : words) word.put(s, word.getOrDefault(s, 0) + 1);

        List<String> result = new ArrayList<String>();
        for (String s : word.keySet()) result.add(s);
            
        Collections.sort(result, new Comparator<String>() {
            public int compare(String word1, String word2) {
                return word.get(word1) == word.get(word2) ? word1.compareTo(word2) : word.get(word2) - word.get(word1);
            }
        });
        return result.subList(0, k);
    }
}
```


# 740.删除并获得点数
## 题目
https://leetcode-cn.com/problems/delete-and-earn


## 题解

借鉴的一个leetcode大神（陆艰步走）的解释：

>首先，我们先明确一个概念，就是每个位置上的数字是可以在两种前结果之上进行选择的：
>- 1.如果你不删除当前位置的数字，那么你得到就是前一个数字的位置的最优结果。
>- 2.如果你觉得当前的位置数字i需要被删，那么你就会得到i - 2位置的那个最优结果加上当前位置的数字乘以个数。
>
>以上两个结果，你每次取最大的，记录下来，然后答案就是最后那个数字了。


**先把数字进行整理一下。**
>我们在原来的 nums 的基础上构造一个临时的数组 all，这个数组，以元素的值来做下标，下标对应的元素是原来的元素的个数。
>**举个例子：**
>nums = [2, 2, 3, 3, 3, 4]
>**构造后：**
>all=[0, 0, 2, 3, 1];
>就是代表着 22 的个数有两个，33 的个数有 33 个，44 的个数有 11 个。

其实这样就可以变成打家劫舍(我还没做过，马上做，下面会加上)的问题了呗。

**打家劫舍的最优子结构的公式：**
>dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);


**再来看看现在对这个问题的最优子结构公式：**
>dp[i] = Math.max(dp[i - 1], dp[i - 2] + i * all[i]);


## 代码

**自己优化了一下：**
```java
	class Solution {
	    public int deleteAndEarn(int[] nums) {
	        int max_Num = 0;
	        //遍历求取数组中最大值
	        for(int num: nums){ if(max_Num < num) max_Num = num; }
	        //根据最大值创建多大的数组
	        int[] dp = new int[max_Num + 1];
	
	        //把数组中的元素存在新的新的数组中，数组下标表示指，该下标的数组指表示该下标出现的次数
	        for (int num: nums) { dp[num]++; }
	
	        //动态规划求解 dp[i] = Math.max(dp[i - 1], dp[i - 2] + i * all[i]);
	        for(int i = 2; i <= max_Num; i++) {
	            dp[i] = Math.max(dp[i - 1], dp[i - 2] + dp[i] * i);
	        }
	        return dp[max_Num];
	    }
	}
```

# 752.打开转盘锁
## 题目
https://leetcode-cn.com/problems/open-the-lock/
## 题解
Set存一下每一步能到的（非锁死）位置，直到到达target 或者 遍历17次也没找到结束返回-1

## 代码

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> dead = new HashSet<>();
        for(int i = 0; i < deadends.length; i++){
            dead.add(deadends[i]);
        }
        if(dead.contains("0000")) return -1;
        int result = 0;
        Set<String> ant = new HashSet<>();
        ant.add("0000");
        while(result < 17){
            if(ant.contains(target)) return result;
            else result++;
            Set<String> temp = new HashSet<>(ant);
            ant.clear();
            Iterator iter = temp.iterator();
            while(iter.hasNext()){
                String tempS = (String)iter.next();
                dead.add(tempS);
                for(int i = 0; i < 4; i++){
                    StringBuffer sb1 = new StringBuffer(tempS);
                    StringBuffer sb2 = new StringBuffer(tempS);
                    char c = sb1.charAt(i);
                    c -= 1;
                    sb1.setCharAt(i, c);
                    c += 2;
                    sb2.setCharAt(i, c);
                    if(tempS.charAt(i) == '0'){
                        sb1.setCharAt(i, '9');
                    } else if(tempS.charAt(i) == '9'){
                        sb2.setCharAt(i, '0');
                    }
                    if(!dead.contains(sb1.toString())) ant.add(sb1.toString());
                    if(!dead.contains(sb2.toString())) ant.add(sb2.toString());
                    //System.out.println("result: " + result  + " " + sb1 + " " + sb2 + " ");
                }
            }
       
        }
        return -1;
    }
}
```


# 877.石子游戏
## 题目
https://leetcode-cn.com/problems/stone-game/
## 题解
必胜？
## 代码

```java
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;
    }
}
```

# 946.验证栈序列
## 题目
https://leetcode-cn.com/problems/validate-stack-sequences/
## 题解
创建一个栈ant和一个表示当前pushed数组的下标位置tempPosi的int：
1.遍历一遍popped数组
2.当栈ant为空或者当前栈顶不等于此时遍历到的popped数组位置对应元素时：
- ①如果当前pushed数组的下标位置tempPosi小于pushed数组的长度，把pushed的当前位置元素加入到栈中
- ②否则，返回false，说明pushed到尾了也没有匹配到和popped数组对应的元素

3.当栈ant为空或者当前栈顶等于此时遍历到的popped数组位置对应元素时：删除栈顶，接着遍历popped数组
4.遍历成功结束后，说明都匹配上了，所以返回true

## 代码

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> ant = new Stack<Integer>();
        int tempPosi = 0;
        for(int i = 0; i < popped.length; i++){
            while(ant.empty() || popped[i] != ant.peek()){
                if(tempPosi < pushed.length) ant.push(pushed[tempPosi++]);
                else return false;
            } 
            ant.pop();
        }
        return true;
    }
}
```

# 990.等式方程的可满足性
## 题目
https://leetcode-cn.com/problems/satisfiability-of-equality-equations/
## 题解
首先创建一个26*26的数组，用来存取字母间的关系：
1.如果数组relation[i][j] = 1,那么说明i表示的字母和j表示的字母对应的数值相同
2.如果数组relation[i][j] = 0,那么说明i表示的字母和j表示的字母对应的数值不知道相同不相同
3.如果数组relation[i][j] = -1,那么说明i表示的字母和j表示的字母对应的数值不相同
首先自己和自己肯定相同，然后把题目给的存一下，这里注意如果出现了矛盾就返回false，然后用三个for来判断是否出现a==b,b==c,但是a!=c这样的情况，出现了就直接返回false。
最后返回true

## 代码

```java
class Solution {
    public boolean equationsPossible(String[] equations) {
        int[][] relation = new int[26][26];
        for(int i = 0; i < 26; i++) relation[i][i] = 1;
        for(int i = 0; i < equations.length; i++){
            char flag = equations[i].charAt(1);
            int x = equations[i].charAt(0) - 'a';
            int y = equations[i].charAt(3) - 'a';
            if(flag == '!' && relation[x][y] != 1){
                relation[x][y] = -1;
                relation[y][x] = -1;
            }
            else if(flag == '=' && relation[x][y] != -1) {
                relation[x][y] = 1;
                relation[y][x] = 1;
            }
            else return false;
        }
        
        for(int i = 0; i < 26; i++){
            for(int j = 0; j < 26; j++){          
                for(int k = 0; k < 26; k++){
                    if(relation[i][j] == 1 && relation[j][k] == 1){
                        if(relation[i][k] == -1) return false;
                        else {
                            relation[i][k] = 1;
                            relation[k][i] = 1;
                        }    
                    }
                }
            }
        }
        return true;
    }
}
```

# 1049.最后一块石头的重量 II
## 题目
https://leetcode-cn.com/problems/last-stone-weight-ii/
## 题解
每次都是两个石头去碰，然后大的还剩下大的减去小的，小的没了，所以我们其实可以**把该问题变成两堆石头**，只要这两堆石头重量**尽可能接近**，让他们去分别相碰，最后结果肯定是最小的，怎么尽可能相近呢？其实只要让一堆尽可能接近所有石头总重量的一半，那么他们两堆一定最接近，所以该问题**变为了背包问题**：
`dp[j] `表示背包能容纳j重量石头时 实际上能装的最大石头重量，**dp公式：**
`dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);`

## 代码

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for(int num : stones) sum += num;
        int[] result = new int[sum / 2 + 1];
        for(int i = 0; i < stones.length; i++){
            for(int j = sum / 2; j >= stones[i]; j--){
                result[j] = Math.max(result[j], result[j - stones[i]] + stones[i]);
            }
        }
        return sum - 2 * result[sum / 2];
    }
}
```

# 1035.不相交的线
## 题目
https://leetcode-cn.com/problems/uncrossed-lines/
## 题解
dp[i][j] 二维数组代表的是数组1的i位置 **和** 数组2的j位置 最长匹配的个数  (dp[0][0] = 0,dp[0][j] = 0, dp[i][0] = 0)
所以对于nums1[i - 1]和nums2[j - 1]：
- 1.如果nums1[i - 1] == nums2[j - 1],那么dp[i][j] = dp[i - 1][j - 1] + 1;
- 2.如果nums1[i - 1] != nums2[j - 1],那么dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);

## 代码

```java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[][] dp = new int[m + 1][n + 1];
        for(int i = 1; i <= m; i++){
            int num1 = nums1[i - 1];
            for(int j = 1; j <= n; j++){
                int num2 = nums2[j - 1];
                if(num1 == num2) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[m][n];
    }
}
```

# 1190.反转每对括号间的子串
## 题目
https://leetcode-cn.com/problems/reverse-substrings-between-each-pair-of-parentheses/
## 题解
每在一对括号内就要反转一次，所以我们可以运用这个特性，当出现了左括号就把截止到现在的result字符存在栈中，出现右括号说明匹配上了一对括号，就把result现在存的字符反转一下，再取出栈顶部的元素放在result字符串首尾中并等着匹配下一对括号。

**例子：** `s = "(u(love)i)"`
`(`: 当前result为空，存在栈中，并清空result
`u`: 存在result中，此时`result = "u"`
`(`: 当前`result = "u"`，存在栈中，此时栈中仅一个String为"u",并清空result
`l`: 存在result中，此时`result = "l"`
`o`: 存在result中，此时`result = "lo"`
`v`: 存在result中，此时`result = "lov"`
`e`: 存在result中，此时`result = "love"`
`)`: 匹配上一对括号了，该反转了，反转result，此时result为"ovel",然后result再加上栈头的元素"u"到首位，此时`result = "uovel"`
`i`: 存在result中，此时`result = "uoveli"`
`)`: 匹配上一对括号了，该反转了，反转result，此时result为"iloveu",然后result再加上栈头的元素到首位(此时栈为空)，所以最终结果`result = "iloveu"`

## 代码

```java
class Solution {
    public String reverseParentheses(String s) {
        Stack<String> q = new Stack<>();
        StringBuffer result = new StringBuffer();
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == '(') {q.push(result.toString()); result.delete(0, result.length());}
            else if(c == ')') result.reverse().insert(0, q.pop());
            else result.append(c);
        }
        return result.toString();
    }
}

```

#1239.串联字符串的最大长度
## 题目
https://leetcode-cn.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/
## 题解
遍历一遍字符串，遍历到的直接加到当前判断里面，然后去dfs后面的字符串，后面的字符串就 **两种情况：** 要么有，要么没有，没有的直接dfs下一个直到遍历结束，有的就判断一下重复没有，没有重复才会加上
## 代码
```java
class Solution {
    int result = 0;
    public int maxLength(List<String> arr) {
        for(int i = 0; i < arr.size(); i++){
            int tempResult = 0;
            boolean flag = false;
            boolean[] used = new boolean[26];
            for(int j = 0; j < arr.get(i).length(); j++){
                int c = arr.get(i).charAt(j) - 'a';
                if(used[c]) {flag = true; break;}
                else {tempResult++; used[c] = true;}
            }
            if(!flag) dfs(arr, used, i + 1, tempResult);    
        }
        return result;
    }
    public void dfs(List<String> arr, boolean[] used, int posi, int tempResult){
        if(posi == arr.size()) result = Math.max(result, tempResult);
        else{
            boolean[] tempUsed = new boolean[26];
            for(int i = 0; i < 26; i++) tempUsed[i] = used[i];
            dfs(arr, tempUsed, posi + 1, tempResult);

            boolean flag = false;
            for(int i = 0; i < arr.get(posi).length(); i++){
                int c = arr.get(posi).charAt(i) - 'a';
                if(tempUsed[c]) {flag = true; break;}
                else {tempResult++; tempUsed[c] = true;}
            }
            if(!flag) dfs(arr, tempUsed, posi + 1, tempResult);
        }
    }
}
```

# 1310.子数组异或查询
## 题目
https://leetcode-cn.com/problems/xor-queries-of-a-subarray/
## 题解
**数学公式须知：** 
1.自己 ^ 自己 = 0

**思路：**
用数组result存一下前i个的异或结果，然后区间[l,r]异或的话就用result[l - 1] ^ result[r]就好了（注意下l是0时,结果直接就是result[r]就好了）
**之前题目的题解：**
https://leetcode-cn.com/problems/decode-xored-permutation/solution/su-kan-you-qu-de-shu-xue-ti-si-lu-qing-x-q8uu/
## 代码

```java
class Solution {
    public int[] xorQueries(int[] arr, int[][] queries) {
        int arrLength = arr.length;
        int queriesLenth = queries.length;
        int[] result = new int[arrLength];
        int[] ans = new int[queriesLenth];
        int l, r;
        
        result[0] = arr[0];
        for(int i = 1; i < arrLength; i++){result[i] = result[i - 1] ^ arr[i];}
        
        for(int i = 0; i < queriesLenth; i++){
            l = queries[i][0];
            r = queries[i][1];
            if(l == 0)
                ans[i] = result[r]; 
            else
                ans[i] = result[l - 1] ^ result[r];
        }
        return ans;
    }
}
```


# 1442. 形成两个异或相等数组的三元组数目
## 题目
https://leetcode-cn.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/

## 题解

把题目给的数组存一下前i位的所有数字的异或结果（比如arr = [2, 3, 1, 6, 7]，变化为arr = [2, 2^3, 2^3^1, 2^3^1^6, 2^3^1^6^7]）,这样有什么好处呢？这样可以快速计算某一个区间内的异或结果，比如计算区间[1,3],那么我们直接arr[3] ^ arr[0]就好了，因为根据上面公式(a ^ a = 0)自己异或自己为0，arr[3]里面包含了arr[0]的部分，所以一异或相当于就去掉了那一部分，所以做题的思路就出来了，暴力i,j,k然后分别求出a和b来，再比较一下，就好了。

**代码优化：** 
1.求a，b还是有点麻烦，我突然发现a == b才是符合题意的（所以a ^ b = 0），而a、b的求取区间又可以合并为一个区间，所以我们直接判断[i - 1, k] 这个区间的异或结果是不是0就可以了（注意一下i为0时的判断）
2.不用求a，b了那j遍历还有意义吗？其实也可以去掉了
3.还可以优化嘛？其实还可以，我们不是首先求了前i位的所有数字的异或结果，这里遍历了一遍，其实直接可以放在我们遍历i和k的地方就可以了，只需要修改一下i,k的遍历顺序。

## 代码


**优化前：**
```java
class Solution {
    public int countTriplets(int[] arr) {
        int result = 0;
        for(int i = 1; i < arr.length; i++) arr[i] ^= arr[i - 1];
        for(int i = 0; i < arr.length - 1; i++)
            for(int j = i + 1; j < arr.length; j++)
                for(int k = j; k < arr.length; k++)
                    if((i == 0 && arr[j - 1] == (arr[k] ^ arr[j - 1])) || (i != 0 && (arr[j - 1] ^ arr[i - 1]) == (arr[k] ^ arr[j - 1]))) result++;              
        return result;
    }
}
```
**仅优化1后：**
```java
class Solution {
    public int countTriplets(int[] arr) {
        int result = 0;
        for(int i = 1; i < arr.length; i++) arr[i] ^= arr[i - 1];
        for(int i = 0; i < arr.length - 1; i++)
            for(int j = i + 1; j < arr.length; j++)
                for(int k = j; k < arr.length; k++)
                    if((i == 0 && arr[k] == 0) || (i != 0 && (arr[k] ^ arr[i - 1]) == 0)) result++;              
        return result;
    }
}
```
 **仅优化1和2后：**
```java
class Solution {
    public int countTriplets(int[] arr) {
        int result = 0;
        for(int i = 1; i < arr.length; i++) arr[i] ^= arr[i - 1];
        for(int i = 0; i < arr.length - 1; i++)
            for(int k = i + 1; k < arr.length; k++)
                if((i == 0 && arr[k] == 0) || (i != 0 && (arr[k] ^ arr[i - 1]) == 0)) result += k - i;              
        return result;
   }
}
```
 **优化1和2和3后：**

```java
class Solution {
    public int countTriplets(int[] arr) {
        int result = 0;
        for(int k = 1; k < arr.length; k++){
            arr[k] ^= arr[k - 1];
            for(int i = 0; i < k; i++)
                if((i == 0 && arr[k] == 0) || (i != 0 && (arr[k] ^ arr[i - 1]) == 0)) result += k - i;              
        }       
        return result;
   }
}
```


# 1482.制作 m 束花所需的最少天数（二分）

## 题目
https://leetcode-cn.com/problems/minimum-number-of-days-to-make-m-bouquets
## 题解
https://leetcode-cn.com/problems/minimum-number-of-days-to-make-m-bouquets/solution/er-fen-ya-by-rain-ru-scmx/

凌晨看到题目，一开始感觉暴力?又感觉不太行，后来上了床想了想感觉可以二分呀！
1.二分bloomDay数组中最大最小值，得到temp，
2.然后判断bloomDay数组中元素是否能满足题意，

满足就存一下此时temp再二分小的和temp-1；
不满足就二分temp+1和大的
3.判断左右大小关系，左>右终止，返回最后存储的temp值


## 代码
```java
	class Solution {
	    int mm;
	    int kk;
	    int length;
	    int result;
	    public int minDays(int[] bloomDay, int m, int k) {
	        length = bloomDay.length;
	        if(m * k > length) return -1;
	        else{
	            int max_Num = 0;
	            int min_Num = Integer.MAX_VALUE;
	            for(int i = 0; i < length; i++){
	                if(bloomDay[i] > max_Num)
	                    max_Num = bloomDay[i];
	                if(bloomDay[i] < min_Num)
	                    min_Num = bloomDay[i];
	            }
	            if(m * k == length) return max_Num;
	            else{
	                mm = m;
	                kk = k;
	                result = max_Num;
	                two_Solve(bloomDay, min_Num, max_Num);
	                return result;
	
	            }
	        }
	        
	    }
	    public void two_Solve(int [] days, int l, int r){
	        if(l > r) return ;
	        int temp = (l + r) / 2;
	        int tempM = 0;
	        int tempK = 0;
	        boolean flag = false;
	
	        for(int i = 0; i < length; i++){
	            if(days[i] <= temp){
	                tempK++;
	                if(tempK == kk){
	                    tempK = 0;
	                    tempM++;
	                    if(tempM == mm){
	                        flag = true;
	                        break;
	                    }
	                }
	            }else{
	                tempK = 0;
	            }
	        }
	        if(flag){
	            result = temp;
	            two_Solve(days, l, temp - 1);
	        }else{
	            two_Solve(days, temp + 1, r);
	        }
	    }
	}
```

# 1600.皇位继承顺序
## 题目
https://leetcode-cn.com/problems/throne-inheritance/
## 题解
模拟一下，求的时候用DFS
## 代码
```java
class ThroneInheritance {
    Map<String, List<String>> child;
    Set<String> dead;
    String king;
    public ThroneInheritance(String kingName) {
        king = kingName;
        child = new HashMap<>();
        dead = new HashSet<>();
        child.put(kingName, new ArrayList<String>());
    }
    
    public void birth(String parentName, String childName) {
        List<String> children = child.getOrDefault(parentName, new ArrayList<String>());
        children.add(childName);
        child.put(parentName, children);
    }
    
    public void death(String name) {
        dead.add(name);
    }
    
    public List<String> getInheritanceOrder() {
        List<String> result = new ArrayList<String>();
        preorder(result, king);
        return result;
    }
    private void preorder(List<String> result, String name) {
        if (!dead.contains(name)) result.add(name);
        List<String> children = child.getOrDefault(name, new ArrayList<String>());
        for (String childName : children) preorder(result, childName);
    }
}
```

# 1679.K 和数对的最大数目
## 题目
https://leetcode-cn.com/problems/max-number-of-k-sum-pairs/
## 题解
HashMap存一下小于k的每个数字出现的次数，然后看和为k的有多少对

## 代码

```java
class Solution {
    public int maxOperations(int[] nums, int k) {
        int result = 0;
        Map<Integer, Integer> hashMap = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            if(nums[i] >= k) continue;
            else { 
                int a = hashMap.getOrDefault(nums[i], 0);
                int b = hashMap.getOrDefault(k - nums[i], 0);
                if(nums[i] == k - nums[i]) result -= a / 2;
                else result -= Math.min(a, b);
                hashMap.put(nums[i], a + 1);
                if(nums[i] == k - nums[i]) result += (a + 1) / 2;
                else result += Math.min(a + 1, b);
            }
        }
        return result;
    }
}
```

# 1734.解码异或后的排列
## 题目
https://leetcode-cn.com/problems/decode-xored-permutation/
## 题解
**数学公式须知：** 
1.自己 ^ 自己 = 0
2.如果a ^ b = c， 那么a ^ c = b, b ^ c = a

**思路：**
1.由题意可知，数组中为[1,encoded.length + 1]，那么我们能算出从1到encoded.length + 1的异或结果total
2.由题意可知，encoded[i] = perm[i] ^ perm[i + 1],那么我们其实也就能根据这个公式算出**perm除了某个位置之外**的其他所有位置的异或和，在这里:
- **假设我们去求perm 0位置**，那么我们只需要把perm原位置1到encoded.length + 1位置的全部异或了，就是除了0位置之外异或和，关键是这个该如何去计算?
- 其实只需要计算odd = encoded[1] ^ encoded[3] ^ encoded[5] ^ …… ^ encoded[encoded.length - 1]，
- 因为根据题中公式能直接转换为odd = perm[1] ^ perm[2] ^ perm[3] ^ …… ^ perm[n - 1] ^ perm[n], 
- 然后根据那个**数学公式1**，自己 ^ 自己 = 0，那么total ^ odd = perm[0]；

3.求出一个位置后,用**数学公式2**去计算其他位置
## 代码
```java
class Solution {
    public int[] decode(int[] encoded) {
        int n = encoded.length;
        int[] result = new int[n + 1];
        int total = 1;
        int odd = encoded[1];
        for(int i = 2; i <= n + 1; i++) total = total ^ i;
        for(int i = 3; i < n ; i += 2) odd = odd ^ encoded[i];
        result[0] = total ^ odd;
        for(int i = 1; i < n + 1 ; i++) result[i] = result[i - 1] ^ encoded[i - 1];
        return result;
    }
}
```

# 1738.找出第 K 大的异或坐标值
## 题目
https://leetcode-cn.com/problems/find-kth-largest-xor-coordinate-value/
## 题解
https://leetcode-cn.com/problems/find-kth-largest-xor-coordinate-value/solution/su-kan-si-lu-qing-xi-shi-jian-98-kong-ji-2d3k/
根据题意，(i,j)坐标意思是矩阵行为[0,i]列为[0,j]的全部元素异或和，那么我们还用原数组存一下异或结果，然后用一个新创建的一维数组存一下这些异或和，最后排序一下输出第k大就好了

**求异或：**
1.i == 0 && j == 0时，matrix[i][j] = matrix[0][0];
2.i == 0时，matrix[i][j] ^= matrix[i][j - 1];
3.j == 0时，matrix[i][j] ^= matrix[i - 1][j];
4.i > 0,j > 0时，matrix[i][j] ^= (matrix[i][j - 1] ^ matrix[i - 1][j - 1] ^ matrix[i - 1][j]) , 相当于下图中，用matrix[i][j - 1] ^ matrix[i - 1][j]时左上角那部分重合自己异或自己为0了，所以需要再次^ matrix[i - 1][j - 1]补回来


## 代码

```java
class Solution {
    public int kthLargestValue(int[][] matrix, int k) {
        int m = matrix.length;
        int n = matrix[0].length;
        int[] result = new int[m * n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(i == 0 && j == 0) ;
                else if(i == 0) matrix[i][j] ^= matrix[i][j - 1];
                else if(j == 0) matrix[i][j] ^= matrix[i - 1][j];
                else matrix[i][j] ^= (matrix[i][j - 1] ^ matrix[i - 1][j - 1] ^ matrix[i - 1][j]);
                result[i * n + j] = matrix[i][j];
            }
        }
        Arrays.sort(result);
        return result[m * n - k];
    }
}
```

# 1744.你能在你最喜欢的那天吃到你最喜欢的糖果吗？
## 题目
https://leetcode-cn.com/problems/can-you-eat-your-favorite-candy-on-your-favorite-day/
## 题解 
创建要给long的candies数组，对应下标i处存放前i种类型的糖果的总数，然后开始判断能不能在那一天吃到他喜欢的糖：
**思路：**
- 1.**每天吃一个糖:** 到他想吃的那天 吃的糖总数 <= 截止到他喜欢的那种糖的类型总数
- 2.**每天吃能吃的最大糖：** 到他想吃的那天 吃的糖总数 > 截止到他喜欢的那种糖的前一类型总数
- 3.满足1和2就说明他能在想吃糖的那天吃到想吃的糖


**注意:**
- 1.想吃的糖类型为0时第2种情况下0的前一类型会越界，为0时仅判断1就可以
- 2.算吃的糖总数时可能会超出int，所以需要用double存一下天数和能吃的最大糖。

我只会心疼哥哥，给哥哥糖吃，哥哥女朋友不会打我吧

## 代码

```java
class Solution {
    public boolean[] canEat(int[] candiesCount, int[][] queries) {
        boolean[] result = new boolean[queries.length];
        long[] candies = new long[candiesCount.length];
        candies[0] = candiesCount[0];
        for(int i = 1; i < candies.length; i++) candies[i] = candies[i - 1] + candiesCount[i];
        for(int i = 0; i < queries.length; i++){
            int type = queries[i][0];
            double day = queries[i][1] + 1;
            double dayMaxEatNum = queries[i][2];
            if(type == 0 && day <= candies[type]) result[i] = true;
            else if(type != 0 && day <= candies[type] && day * dayMaxEatNum > candies[type - 1]) result[i] = true;
        }
        return result;
    }
}
```


# 1828.统计一个圆中点的数目
## 题目
https://leetcode-cn.com/problems/queries-on-number-of-points-inside-a-circle/
## 题解
暴力，看每个点在不在圆内
## 代码

```java
class Solution {
    public int[] countPoints(int[][] points, int[][] queries) {
        int[] result = new int[queries.length];
        for(int i = 0; i < queries.length; i++){
            double x = queries[i][0], y = queries[i][1], r = queries[i][2];
            for(int j = 0; j < points.length; j++){
                if(Math.pow((double)points[j][0] - x, 2) + Math.pow((double)points[j][1] - y, 2) <= Math.pow(r, 2)) result[i]++;
            }
        }
        return result;
    }
}
```


# 1833.雪糕的最大数量
## 题目
https://leetcode-cn.com/problems/maximum-ice-cream-bars/
## 题解 
排序下，然后看累加和

## 代码

```java
class Solution {
    public int maxIceCream(int[] costs, int coins) {
        Arrays.sort(costs);
        int sum = 0;
        for(int i = 0; i < costs.length; i++){
            sum += costs[i];
            if(sum > coins) return i;
        }
        return costs.length;
    }
}
```

# 1860.增长的内存泄露
## 题目
https://leetcode-cn.com/problems/incremental-memory-leak/
## 题解 
谁大去减谁

## 代码

```java
class Solution {
    public int[] memLeak(int memory1, int memory2) {
        int s = 0;
        while(memory1 >= s || memory2 >= s){
            if(memory2 > memory1) memory2 -= s;
            else memory1 -= s;
            s++;
        }
        return new int[]{s, memory1, memory2};
    }
}
```


# 1861.旋转盒子
## 题目
https://leetcode-cn.com/problems/rotating-the-box/
## 题解 
## 代码
```java
class Solution {
    public char[][] rotateTheBox(char[][] box) {
        int m = box.length;
        int n = box[0].length;
        int nums = 0;
        int top = 0;
        boolean flag;
        for(int i = 0; i < m; i++){
            nums = 0;
            top = 0;
            flag = false;
            for(int j = 0; j < n; j++){
                if(box[i][j] == '#') nums++;
                else if(nums > 0 && (box[i][j] == '*' || j == n - 1) && flag){
                    if(box[i][j] == '*')
                    {
                        for(int k = top; k < j; k++){
                            if(j - k > nums) box[i][k] = '.';
                            else box[i][k] = '#';
                        }
                    }
                    else{
                        for(int k = top; k < n; k++){
                            if(j - k > nums - 1) box[i][k] = '.';
                            else box[i][k] = '#';
                        }
                    }
                    top = j + 1;
                    flag = false;
                    nums = 0;
                }
                else if(!flag && box[i][j] == '*') {top = j + 1; nums = 0;}
                else if(box[i][j] == '*'){
                    flag = false;
                    nums = 0;
                }
                else if(box[i][j] == '.' && nums > 0) flag = true;
                if(flag && j == n - 1 && nums > 0){
                    for(int k = top; k < n; k++){
                        if(j - k > nums - 1) box[i][k] = '.';
                        else box[i][k] = '#';
                    }
                    top = j + 1;
                    flag = false;
                    nums = 0;
                }
            }
        }
        char[][] result = new char[n][m];
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                result[i][j] = box[m - j - 1][i];
            }
        }
        return result;
    }
}
```

# 1864.构成交替字符串需要的最小交换次数
## 题目
https://leetcode-cn.com/problems/minimum-number-of-swaps-to-make-the-binary-string-alternating/
## 题解 
1.先判断是否 需要返回-1,就是 s长度为偶数，然后0的个数 != 1的个数, 或者 s长度为奇数，然后0的个数-1 != 1的个数且0的个数+1 != 1的个数
2.后面用0101010……0串和1010101……1串和s进行比较，n为奇数和偶数时分别返回的也不相同

## 代码

```java
class Solution {
    public int minSwaps(String s) {
        int n = s.length();
        int zeroNums = 0;
        for(int i = 0; i < n; i++) if(s.charAt(i) == '0') zeroNums++;
        if(zeroNums != n / 2 && n % 2 == 0) return -1;
        else if(zeroNums != n / 2 + 1 && zeroNums != n / 2 && n % 2 == 1) return -1;
        else{
            StringBuffer zeroOne = new StringBuffer();
            StringBuffer oneZero = new StringBuffer();
            for(int i = 0; i < n; i++){
                zeroOne.append(i % 2);
                oneZero.append((i + 1) % 2);
            }
            int oneZeroNums = 0;
            int zeroOneNums = 0;
            for(int i = 0; i < n; i++){
                if(zeroOne.charAt(i) != s.charAt(i)) zeroOneNums++;
                else oneZeroNums++;
            }
            if(zeroNums == n / 2 && n % 2 == 0)
                return Math.min(oneZeroNums, zeroOneNums) / 2;
            else if(zeroNums == n / 2 && n % 2 == 1)
                return oneZeroNums / 2;
            else
                return zeroOneNums / 2;
        }
    }
}
```


# 1865.找出和为指定值的下标对
## 题目
https://leetcode-cn.com/problems/finding-pairs-with-a-certain-sum/
## 题解 
因为有统计操作，然后nums2的最大长度有点大，所以我们可以创建一个HashMap存一下nums2中每一个数字出现的次数，key为这个数字，value为在nums2中出现的次数（需要注意的是我们要在初始化中就把HashMap就创建好并初始化好，这样后面add操作可以直接修改一个值，而不需要每进行一次统计操作就去HashMap初始化一下，否则会超时）

## 代码

```java
class FindSumPairs {
    public int[] nums1;
    public int[] nums2;
    Map<Integer, Integer> nums = new HashMap<>();
    public FindSumPairs(int[] nums1, int[] nums2) {
        this.nums1 = nums1;
        this.nums2 = nums2;
        for(int i = 0; i < nums2.length; i++) { 
            if(nums.containsKey(nums2[i]))
                nums.put(nums2[i],nums.get(nums2[i]) + 1);
            else
                nums.put(nums2[i],1);
        }
    }
    
    public void add(int index, int val) {
        nums.put(nums2[index],nums.get(nums2[index]) - 1);
        nums2[index] += val;
        if(nums.containsKey(nums2[index]))
                nums.put(nums2[index],nums.get(nums2[index]) + 1);
            else
                nums.put(nums2[index],1);
    }
    
    public int count(int tot) {
        int result = 0;
        for(int i = 0; i < nums1.length; i++){
            if(nums.containsKey(tot - nums1[i])) result += nums.get(tot - nums1[i]);
        }
        return result;
    }
}
```

# 1877.数组中最大数对和的最小值
## 题目
https://leetcode-cn.com/problems/minimize-maximum-pair-sum-in-array/
## 题解 

排一下序，然后左右加，找到最大的就返回

## 代码

```java
class Solution {
    public int minPairSum(int[] nums) {
        Arrays.sort(nums);
        int ans = 0;
        int left = 0;
        int right = nums.length - 1;
        for(;left < right;) ans = Math.max(ans, nums[left++] + nums[right--]);
        return ans;
    }
}
```

# 1878.矩阵中最大的三个菱形和
## 题目
https://leetcode-cn.com/problems/get-biggest-three-rhombus-sums-in-a-grid/
## 题解 
k表示菱形每条斜边占了几个方格，然后循环一下起始位置
## 代码

```java
class Solution {
    public int[] getBiggestThree(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[] ans = new int[]{0,0,0};
            
        for(int k = 1; k <= (Math.min(m, n) + 1) / 2; k++){
            for(int up = 0; up <= m - k * 2 + 1; up++){
                for(int left = 0; left <= n - k * 2 + 1; left++){
                    int sum = 0;
                    for(int i = 0; i <= k - 1; i++){
                        if(i == 0) sum += grid[up][left + k - 1];
                        else sum += (grid[up + i][left + k - 1 - i] + grid[up + i][left + k - 1 + i]);
                    }
                    //System.out.println(" k: " + k + " up: " + up + " left: " + left + " sum: " +sum);
                    for(int i = 0; i < k - 1; i++){
                        if(i == 0) sum += grid[up + k * 2 - 2][left + k - 1];
                        else sum += (grid[up + k * 2 - 2 - i][left + k - 1 - i] + grid[up + k * 2 - 2 - i][left + k - 1 + i]);
                    }
                    if(sum <= ans[2] || sum == ans[0] || sum == ans[1]) ;
                    else if(sum > ans[0]){ans[2] = ans[1]; ans[1] = ans[0]; ans[0] = sum;}
                    else if(sum > ans[1]){ans[2] = ans[1]; ans[1] = sum;}
                    else ans[2] = sum;
                }
            }     
        }
        if(ans[1] == 0 && ans[2] == 0) return new int[]{ans[0]};
        else if(ans[2] == 0) return new int[]{ans[0], ans[1]};
        else return ans;
    }
}
```


# 1881.插入后的最大值

## 题目
https://leetcode-cn.com/problems/maximum-value-after-insertion/
## 题解 

分正负，讨论一下：
1.负的插入的x越小越要放前面
2.正的插入的x越大越要放前面

## 代码

```java
class Solution {
    public String maxValue(String n, int x) {
        int temPosi = 0;
        boolean flag = false;
        StringBuffer result = new StringBuffer();
        if(n.charAt(0) == '-'){
            result.append(n.charAt(temPosi++));
            while(temPosi < n.length()){
                if(!flag && x < n.charAt(temPosi) - '0'){result.append(x); flag = true;}
                else result.append(n.charAt(temPosi++));
            }  
        }else{
            while(temPosi < n.length()){
                if(!flag && x > n.charAt(temPosi) - '0'){result.append(x); flag = true;}
                else result.append(n.charAt(temPosi++));
            }  
        }
        if(!flag) result.append(x);
        return result.toString();           
    }
}
```


# 1882.使用服务器处理任务
## 题目
https://leetcode-cn.com/problems/process-tasks-using-servers/
## 题解 
创建两个长度为3的int数组类型（int中位置0存放位置，1存放权重，2存放最后一次任务第几秒结束）优先队列：
1.一个存放所有服务器allServer的信息**并**根据服务器最后一个任务截至时间、服务器权重、服务器编号排序
2.另一个存放截止到当前时间time的空闲服务器freeServer信息**并**根据服务器权重、服务器编号排序
思路：
1.首先把每个服务器的信息放到优先队列allServer中，初始最后一个任务截至时间都为0
2.循环每一个任务：
- ①把所有服务器allServer中截止到当前时间空闲的都放到空闲服务器freeServer中
- ②如果空闲服务器freeServer大小为0，说明截止到当前时间没有空闲的服务器，那么就去找所有服务器allServer中每个服务器完成其最后一个任务的截止时间最小的，把他取出来然后做当前的任务
- ③如果空闲服务器freeServer大小不为0，取空闲服务器freeServer中的头做当前的任务
- ④更新取出的服务器信息并再放到所有服务器allServer中
- ⑤更新时间：如果进了②说明时间time需要更新为取出来的服务器的最后一个任务的截止时间。对于所有情况来说当当前的时间time <= 第i个任务的开始时间i时，time需要+1
## 代码

```java
class Solution {
    //int数组位置0存放位置，1存放权重，2存放最后一次任务第几秒结束
    public int[] assignTasks(int[] servers, int[] tasks) {
        PriorityQueue<int[]> allServer = new PriorityQueue<>(new Comparator<int[]>(){
            @Override
            public int compare(int [] o1, int [] o2) {
                if(o1[2] != o2[2]) return o1[2] - o2[2];
                else{
                    if(o1[1] != o2[1]) return o1[1] - o2[1];
                    else return o1[0] - o2[0];
                }
            }
        });
        PriorityQueue<int[]> freeServer = new PriorityQueue<>(new Comparator<int[]>(){
            @Override
            public int compare(int [] o1, int [] o2) {
                if(o1[1] != o2[1]) return o1[1] - o2[1];
                else return o1[0] - o2[0];              
            }
        });

        for(int i = 0; i < servers.length; i++)  
            allServer.add(new int[]{i, servers[i], 0});

        int temp[];
        int time = 0;
        for(int i = 0; i < tasks.length; i++){       
            while(allServer.size() != 0 && allServer.peek()[2] <= time)
                freeServer.add(allServer.poll());
            if(freeServer.size() == 0){
                temp = allServer.poll();
                time = temp[2];       
            }
            else temp = freeServer.poll();
                
            temp[2] = time + tasks[i];
            allServer.add(temp);
            tasks[i] = temp[0];

            if(time <= i) time++;
        }
        return tasks;
    }
}
```

# 1894.找到需要补充粉笔的学生编号
## 题目
https://leetcode-cn.com/problems/find-the-student-that-will-replace-the-chalk/
## 题解 
普通前缀和，注意总和超过int
## 代码

```java
class Solution {
    public int chalkReplacer(int[] chalk, int k) {
        long[] sum = new long[chalk.length + 1];
        for(int i = 0; i < chalk.length; i++){
            sum[i + 1] += (sum[i] + chalk[i]); 
        }
        k %= sum[chalk.length];
        for(int i = 0; i < chalk.length; i++){
            if(sum[i + 1] > k) return i;
        }
        return -1;
    }
}
```

# 1895.最大的幻方
## 题目
https://leetcode-cn.com/problems/largest-magic-square/

## 题解 
普通前缀和+模拟
## 代码

```java
class Solution {
    public int largestMagicSquare(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int k = Math.min(m, n);
        int[][] result = new int[m + 1][n + 1];
        for(int i = 1; i <= m; i++)
            for(int j = 1; j <= n; j++)
                result[i][j] = grid[i - 1][j - 1] + result[i - 1][j] + result[i][j - 1] - result[i - 1][j - 1];
        for(int l = k; l >= 2; l--){
            for(int x = 0; x <= m - l; x++){
                for(int y = 0; y <= n - l; y++){
                    boolean flag = false;
                    int sum = result[x + 1][y + l] - result[x + 1][y] - result[x][y + l] + result[x][y];
                    //System.out.println(l + " " + sum);
                    int sum1 = 0, sum2 = 0;
                    for(int i = 0; i < l; i++){
                        if(sum != result[x + 1 + i][y + l] - result[x + 1 + i][y] - result[x + i][y + l] + result[x + i][y]){
                            //int temp = result[x + 1 + i][y + l] - result[x + 1 + i][y] - result[x][y + l] + result[x][y + i];
                            //System.out.println("temp "+ temp + " i:" + i + "???" + 1);
                            flag = true;
                            break;
                        }
                        if(sum != result[x + l][y + 1 + i] - result[x + l][y + i] - result[x][y + 1 + i] + result[x][y + i]){
                            flag = true;
                            //System.out.println(i + "???" + 2);
                            break;
                        }
                        sum1 += grid[x + i][y + i];
                        sum2 += grid[x + i][y + l - i - 1];
                    }
                    if(sum1 != sum || sum2 != sum) flag = true;
                    if(!flag) return l;
                }
            }   
        }
        return 1;
    }
}
```

# 1910.删除一个字符串中所有出现的给定子字符串
## 题目
https://leetcode-cn.com/problems/remove-all-occurrences-of-a-substring/
## 题解 
简单模拟一下
## 代码

```java
class Solution {
    public String removeOccurrences(String s, String part) {
        int posi = s.indexOf(part);
        while(posi != -1){
            s = s.replaceFirst(part, "");
            posi = s.indexOf(part);
        }
        return s;
    }
}
```


# 1911.最大子序列交替和
## 题目
https://leetcode-cn.com/problems/maximum-alternating-subsequence-sum/
## 题解 
记录下上一次加的数和上一次减的数还有当前和，进行模拟

## 代码
```java
class Solution {
    public long maxAlternatingSum(int[] nums) {
        long result = nums[0];
        long tempSum = nums[0];
        boolean flag = false;
        int a = 0, b = nums[0];
        for(int i = 0; i < nums.length; i++){
            if(flag){ //要去加
                if(nums[i] <= a) {
                    tempSum += (a - nums[i]);
                    a = nums[i];
                } else{
                    b = nums[i];
                    tempSum += nums[i];
                    result = Math.max(tempSum, result);
                    flag = false;
                }
            } else{ //要去减
                if(nums[i] > b) {
                    tempSum += (nums[i] - b);
                    b = nums[i];
                    result = Math.max(tempSum, result);
                } else{
                    a = nums[i];
                    tempSum -= nums[i];
                    flag = true;
                }
            }
        }
        return result;
    }
}
```