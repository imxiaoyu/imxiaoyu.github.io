---
title: 'Leetcode(剑指offer面试)'
date: 2021/6/10 22:17:47 
tags:
	- Leetcode
---
>**题目列表：**
>剑指 Offer 04 二维数组中查找
>剑指 Offer 12.矩阵中的路径（dfs）
>剑指 Offer 13.机器人的运动范围（dfs）
>剑指 Offer 14-I.剪绳子（dp）
>剑指 Offer 14-II.剪绳子 II（找规律？）
>剑指 Offer 16.数值的整数次方（位运算）
>剑指 Offer 26.树的子结构（dfs或者递归）
>剑指 Offer 31.栈的压入、弹出序列（栈）
>剑指 Offer 32.-I.从上到下打印二叉树(BFS)
>剑指 Offer 32.-III.从上到下打印二叉树 III(BFS)
>剑指 Offer 34.二叉树中和为某一值的路径（DFS）
>剑指 Offer 44.数字序列中某一位的数字(找规律)
>剑指 Offer 47.礼物的最大价值（dp）
>剑指 Offer 49.丑数(数学规律推导)
>剑指 Offer 63.股票的最大利润（数组）
>剑指 Offer 64.求1+2+…+n（数学）
>剑指 Offer 66.构建乘积数组(双指针或者二分)
>面试题 (10.03).搜索旋转数组（二分）


18道题目
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

# 剑指 Offer 26.树的子结构

## 题目
https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/
## 题解
1.先用dfs求二叉树B的长度，接下来需要用长度来判断是不是A的部分和B匹配上了
2.再用dfs去匹配A和B：
- ①如果当前A的val和当前tempB的val一样，匹配长度+1继续去匹配A的左子树、tempB的左子树，A的右子树、tempB的右子树
- ②如果当前A的val和当前tempB的val不一样，那么tempB变为B的初始根节点去匹配A的左子树、A的右子树
- ③当匹配长度等于二叉树B的长度时，停止匹配

## 代码

```java
class Solution {
    int tempNum = 0;
    int bLength = 0;
    boolean result = false;
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A == null || B == null) return false;
        bLengthDfs(B);
        dfs(A, B, B);
        return result;
    }
    public void bLengthDfs(TreeNode B){
        if(B == null) return;
        else{
            bLength++;
            bLengthDfs(B.left);
            bLengthDfs(B.right);
        }
    }
    public void dfs(TreeNode A, TreeNode B, TreeNode tempB){
        if(tempNum == bLength) result = true;
        else if(result || A == null || tempB == null) return ;
        else{
            if(A.val == tempB.val){
                tempNum++;
                dfs(A.left, B, tempB.left);
                dfs(A.right, B,  tempB.right);
            }
            else{
                tempNum = 0;
                dfs(A.left, B, B);
                dfs(A.right, B, B);
            }
        }
    }
}
```

# 剑指 Offer 31.栈的压入、弹出序列

## 题目
https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/
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

# 剑指 Offer 32.-I.从上到下打印二叉树

## 题目
https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/
## 题解
- 1.用一个List ant储存树中的节点
- 2.当ant不为空时说明还有节点，然后按顺序取出ant中的每一个节点
- 3.对于每一个节点在ant的最后存一下他的val并把他的非空左孩子和非空右孩子，直到ant为空
- 4.最后把存的val放到数组里返回

## 代码

```java
class Solution {
    public int[] levelOrder(TreeNode root) {
        if(root == null) return new int[]{};
        List<TreeNode> ant = new ArrayList<>();
        List<Integer> ans = new ArrayList<>();
        ant.add(root);
        while(ant.size() != 0){
            TreeNode temp = ant.get(0);
            ans.add(temp.val);
            if(temp.left != null) ant.add(temp.left);
            if(temp.right != null) ant.add(temp.right);
            ant.remove(0);           
        }
        int[] result = new int[ans.size()];
        for(int i = 0; i < ans.size(); i++)
            result[i] = ans.get(i);
        return result;    
    }
}
```


# 剑指 Offer 32.-III.从上到下打印二叉树 III

## 题目
https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/
## 题解
- 1.定义一个**当前层的List tempDeep存放非空的根节点root**
- 2.tempDeep非空时**进入第一层while**，创建一个下一层的List nextDeep 和 当前层的val结果List
- 3.tempDeep非空时**进入第二层while**，把当前层tempDeep中每一个节点的val放入一个Integer的List中，然后他的非空左右孩子按顺序放到下一层nextDeep中，删除遍历完的节点
- 4.当tempDeep判断完一遍后也就是tempDeep变为空**退出第二层while**，然后更新当前层为下一层即tempDeep = nextDeep，把存放val的List放入结果result中(注意题目要求奇数层正向输出，偶数层反向输出，所以定义一个flag，偶数层的时候反转一下val存放的List)
- 5.tempDeep非空时又**进入第二层while（也就是第3步）**
- 6.**tempDeep为空**了，第一层while也结束，返回结果result

## 代码

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        boolean flag = false;
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
            if(flag) Collections.reverse(ans);
            result.add(ans);
            tempDeep = nextDeep; 
            if(flag) flag = false;
            else flag = true;
        }
        return result;  
    }
}
```

# 剑指 Offer 34.二叉树中和为某一值的路径

## 题目
https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/
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

# 剑指 Offer 44.数字序列中某一位的数字

## 题目
https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/
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

# 剑指 Offer 47.礼物的最大价值

## 题目
https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/
## 题解
**dp[x][y]表示走到(x,y)位置时的最大价值：** 对于(x,y)位置来说 **只能从** 上面(x - 1, y) **或者** 左面(x, y - 1)走过来，所以dp[x][y] = 上面位置、左面位置的 **最大值加上** 当前位置的价值
**dp公式：**
`dp[x][y] = Math.max(dp[x][y - 1], dp[x - 1][y]) + grid[x][y]`

**注意：**
1.边界情况,即：
- ①(x = 0, y = 0)时，dp[x][y] = grid[0][0]
- ②(x = 0, y != 0)时，dp[x][y] = dp[0][y - 1] + grid[0][y]
- ③(x != 0, y = 0)时，dp[x][y] = dp[x - 1][0] + grid[x][0]

2.代码中我没有创建dp数组，直接用原数组替代了

## 代码

```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int x = 0; x < m; x++){
            for(int y = 0; y < n; y++){
                if(x == 0 && y == 0) ;
                else if(x == 0) grid[x][y] += grid[x][y - 1];
                else if(y == 0) grid[x][y] += grid[x - 1][y];
                else grid[x][y] += Math.max(grid[x][y - 1], grid[x - 1][y]);
            }
        }
        return grid[m - 1][n - 1];
    }
}
```


# 剑指 Offer 49.丑数
## 题目
https://leetcode-cn.com/problems/chou-shu-lcof/
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


# 剑指 Offer 63.股票的最大利润

## 题目
https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/
## 题解
创建两个int，一个记录当前最小股票tempMin价格，一个记录结果result，遍历一遍数组：
1.如果数组当前位置的价格比tempMin小，则更新tempMin
2.如果数组当前位置的价格比tempMin大，则更新结果（看一下当前位置卖出是否更赚钱）
## 代码

```java
class Solution {
    public int maxProfit(int[] prices) {
        int tempMin = 10000, result = 0;
        for(int price : prices){
            if(price < tempMin) tempMin = price;
            else result = Math.max(result, price - tempMin);
        }
        return result;
    }
}
```

# 剑指 Offer 64.求1+2+…+n

## 题目
https://leetcode-cn.com/problems/qiu-12n-lcof/solution/
## 题解
求和公式 `(n + 1) * n / 2 = (n * n + n) / 2 = ((int)Math.pow(n, 2) + n) >> 1`

## 代码

```java
class Solution {
    public int sumNums(int n) {
        return ((int)Math.pow(n, 2) + n) >> 1;
    }
}
```

# 剑指 Offer 66.构建乘积数组

## 题目
https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/solution/
## 题解
**法1 循环计算：**
对于result[i]来说：
他其实可以分成两部分计算:一部分是a[0] * a[1] *…… *a[i - 1],另一部分是a[i + 1] * a[i + 2] *…… *a[a.length - 1]。所以我们可以根据这两部分用两个for来计算，第一个for正序遍历用来把第一部分求出来并存放到reuslt中，第二个for用来计算第二部分并把最终的结果算出来存在result中。


**法2 二分：**
二分的话就是先计算一个所有相乘的总乘积，然后for循环对于每一个数二分查找结果即可（注意下数组中数值为负、总乘积为负、总乘积为0的情况）
## 代码
**循环计算：**
```java
class Solution {
    public int[] constructArr(int[] a) {
        if(a.length == 0) return a;
        int[] result = new int[a.length];
        result[0] = 1;
        for(int i = 1; i < a.length; i++){
            result[i] = result[i - 1] * a[i - 1]; 
        }
        int temp = 1;
        for(int i = a.length - 2; i >= 0; i--){
            temp *= a[i + 1];
            result[i] *= temp;
        }
        return result;
    }
}
```
**二分：**
```java
class Solution {
    public int[] constructArr(int[] a) {
        int sum = 1, zeroSum = 1, zero = 0;
        int[] result = new int[a.length];
        for(int num : a) {
            if(num == 0) zero++;
            else zeroSum *= num;
            sum *= num;
        }
        for(int i = 0; i < a.length; i++){
            if(sum == 0 && (a[i] != 0 || zero != 1)) result[i] = 0;
            else if(sum == 0 && zero == 1) result[i] = zeroSum;
            else if(a[i] == 1) result[i] = zeroSum;
            else if(a[i] == -1) result[i] = -zeroSum;
            else if(a[i] == 2) result[i] = zeroSum >> 1;
            else if(a[i] == -2) result[i] = -zeroSum >> 1;
            else {
                boolean flag = false;
                if(sum < 0 && a[i] < 0){sum = -sum; a[i] = -a[i];}
                else if(sum < 0){flag = true; sum = -sum;}
                else if(a[i] < 0){flag = true; a[i] = -a[i];}
                long l = 0, r = sum;
                while(l <= r){
                    long mid = (l + r + 1) >> 1;
                    if(mid * a[i] == sum){
                        if(flag) result[i] = (int)-mid;
                        else result[i] = (int)mid;
                        sum = zeroSum;
                        l = r + 1;
                        break;
                    }
                    else if(mid * a[i] > sum) r = mid - 1;
                    else l = mid + 1;
                }
            } 
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