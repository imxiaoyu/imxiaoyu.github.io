---
title: 'Leetcode(dp)'
date: 2021/5/5 23:27:47 
tags:
	- Leetcode
	- 动态规划
---
>**题目列表：**
>740.删除并获得点数
>198.打家劫舍
>213.打家劫舍II
>337.打家劫舍III(需要多写几次**)


<!-- more -->
# 740.删除并获得点数
## 题目
https://leetcode-cn.com/problems/delete-and-earn
>1.给你一个整数数组 nums ，你可以对它进行一些操作。
>2.每次操作中，选择任意一个 nums[i] ，删除它并获得 nums[i] 的点数。之后，你必须删除每个等于 nums[i] - 1 或 nums[i] + 1 的元素。
>3.开始你拥有 0 个点数。返回你能通过这些操作获得的最大点数。


>**提示：**
>1 <= nums.length <= 2 * 10^4
>1 <= nums[i] <= 10^4


### 示例 1
>**输入：**nums = [3,4,2]
>**输出：**6
>**解释：**
>- 删除 4 获得 4 个点数，因此 3 也被删除。
>- 之后，删除 2 获得 2 个点数。总共获得 6 个点数。


### 示例 2

>**输入：**nums = [2,2,3,3,3,4]
>**输出：**9
>**解释：**
>- 删除 3 获得 3 个点数，接着要删除两个 2 和 4 。
>- 之后，再次删除 3 获得 3 个点数，再次删除 3 获得 3 个点数。总共获得 9 个点数。



## 思路

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

# 198.打家劫舍

## 题目
https://leetcode-cn.com/problems/house-robber
>1.你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
>2.给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。


>**提示：**
>1 <= nums.length <= 100
>1 <= nums[i] <= 400


### 示例 1
>**输入：**nums = [1,2,3,1]
>**输出：**4
>**解释：**
>- 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
>- 偷窃到的最高金额 = 1 + 3 = 4 。


### 示例 2

>**输入：**nums = [2,7,9,3,1]
>**输出：**12
>**解释：**
>- 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
>- 偷窃到的最高金额 = 2 + 9 + 1 = 12 。


## 思路

>跟上题类似

**最优子结构的公式：**
>dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);

**注意：**
>1.当数组长度为1时:返回nums[0];
>2.当数组长度为2时:返回Math.max(nums[0],nums[1]);
>3.当数组长度大于3时:结构为上式。

## 代码


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

# 213.打家劫舍II

## 题目
https://leetcode-cn.com/problems/house-robber-ii
>1.你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都** 围成一圈 **，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。
>2.给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。


>**提示：**
>1 <= nums.length <= 100
>1 <= nums[i] <= 1000



### 示例 1
>**输入：**nums = [2,3,2]
>**输出：**3
>**解释：**
>- 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。


### 示例 2

>**输入：**nums = [1,2,3,1]
>**输出：**4
>**解释：**
>- 你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
>- 偷窃到的最高金额 = 1 + 3 = 4 。


### 示例 3
>**输入：**nums = [0]
>**输出：**0


## 思路

>**跟上题类似，但是可以分别去掉首部和尾部，然后判断两个的大小，返回大的。**


**最优子结构的公式：**
>dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);

**注意：**
>1.当数组长度为1时:返回nums[0];
>2.当数组长度为2时:返回Math.max(nums[0], nums[1]);
>3.当数组长度为3时:返回Math.max(Math.max(nums[0], nums[1]), nums[2]);
>4.当数组长度大于4时:去掉首部和尾部分为两部分进行求解。

## 代码


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

# 337.打家劫舍III(需要多写几次) **

## 题目
https://leetcode-cn.com/problems/house-robber-iii
二叉树状房间
## 思路

>**简化一下这个问题：**一棵二叉树，树上的每个点都有对应的权值，每个点有两种状态（选中和不选中），问在不能同时选中有父子关系的点的情况下，能选中的点的最大权值和是多少。

>我们可以用 f(o)f(o) 表示选择 o 节点的情况下，o 节点的子树上被选择的节点的最大权值和；g(o)g(o) 表示不选择 o 节点的情况下，o 节点的子树上被选择的节点的最大权值和；l 和 r 代表 o 的左右孩子。

>**1.**当 o 被选中时，o 的左右孩子都不能被选中，故 o 被选中情况下子树上被选中点的最大权值和为 l 和 r 不被选中的最大权值和相加，即 f(o) = g(l) + g(r)f(o)=g(l)+g(r)。
>**2.**当 o 不被选中时，o 的左右孩子可以被选中，也可以不被选中。对于 o 的某个具体的孩子 xx，它对 o 的贡献是 xx 被选中和不被选中情况下权值和的较大值。故 g(o) = \max \{ f(l) , g(l)\}+\max\{ f(r) , g(r) \}g(o)=max{f(l),g(l)}+max{f(r),g(r)}。


>至此，我们可以用哈希表来存 f 和 g 的函数值，用深度优先搜索的办法后序遍历这棵二叉树，我们就可以得到每一个节点的 f 和 g。根节点的 f 和 g 的最大值就是我们要找的答案。


## 代码


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

用数组优化后：
	
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