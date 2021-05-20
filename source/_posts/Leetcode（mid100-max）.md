---
title: 'Leetcode(mid100-)'
date: 2021/5/19 22:29:31
tags:
	- Leetcode
---
>**题目列表：**
>153.寻找旋转排序数组中的最小值（二分）
>198.打家劫舍（dp）
>213.打家劫舍II（dp）
>240.搜索二维矩阵 II
>337.打家劫舍III(需要多写几次**)
>343.整数拆分（dp）
>421.数组中两个数的最大异或值(字典树)
>692.前K个高频单词
>740.删除并获得点数（dp）
>1310.子数组异或查询（异或运用）
>1442.形成两个异或相等数组的三元组数目
>1482.制作 m 束花所需的最少天数（二分）
>1734.解码异或后的排列
>1738.找出第 K 大的异或坐标值
>1860.增长的内存泄露
>1861.旋转盒子
>1864.构成交替字符串需要的最小交换次数
>1865.找出和为指定值的下标对





<!-- more -->
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