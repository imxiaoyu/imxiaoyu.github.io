---
title: 'Leetcode(difficult)'
date: 2021/6/30 22:21:59 
tags:
	- Leetcode
---
>**题目列表：**
>4.寻找两个正序数组的中位数
>23.合并K个升序链表（优先队列）
>25.K个一组翻转链表(模拟)
>65.有效数字（模拟）
>149.直线上最多的点数（利用斜率，Map）
>154.寻找旋转排序数组中的最小值 II（二分）
>297.二叉树的序列化与反序列化（BFS）
>483.最小好进制(数学)
>664.奇怪的打印机（二维动态规划dp）
>773.滑动谜题（bfs）
>879.盈利计划（dp背包）
>810.黑板异或游戏
>1074.元素和为目标值的子矩阵数量（前缀和+暴力）
>1269.停在原地的方案数（dp）
>1449.数位成本和为目标值的最大数字（完全背包）
>1723.完成所有工作的最短时间（dfs+剪枝多写）
>1862.向下取整数对和 （化简遍历数目）


<!-- more -->

# 4.寻找两个正序数组的中位数
## 题目

https://leetcode-cn.com/problems/median-of-two-sorted-arrays/

## 题解

>**方法一：**
>创建一个数组，把两个数组的元素都加进去，然后排序，去中位数
>**方法二：**
>1.两个数组分别排序；
>2.然后把一个数组为空的情况输出一下，两个数组都只有一个元素的情况输出一下；
>3.对于剩下的情况，两个数组同步进行记录小的元素，然后次小的……直到中位数（两个数组长度为奇数）或者中间两个数（两个数组长度为偶数），输出。


## 代码

**方法一：**
```java
	class Solution {
	    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
	        int oneLength = nums1.length;
	        int twoLength = nums2.length;
	        int length = oneLength + twoLength;
	        int[] result = new int[length];
	        for(int i = 0; i < oneLength; i++){
	            result[i] = nums1[i];
	        }
	        for(int i = 0; i < twoLength; i++){
	            result[oneLength + i] = nums2[i];
	        }
	        Arrays.sort(result);
	
	        if(length == 1)
	            return (double)result[0];
	        else if(length % 2 == 0)
	            return ((double)result[length / 2 - 1] + (double)result[length / 2]) / 2.0;
	        else
	            return (double)result[length / 2];
	    }
	}
```
**方法二：**
```java
	class Solution {
	    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
	        int oneLength = nums1.length;
	        int twoLength = nums2.length;
	        int length = oneLength + twoLength;
	        boolean flag = true;
	        if(length % 2 == 0)
	            flag = false;
	        if(length == 1){
	            if(oneLength == 1)
	                return (double)nums1[0];
	            else
	                return (double)nums2[0];
	        }
	
	        Arrays.sort(nums1);
	        Arrays.sort(nums2);
	        
	        if(oneLength == 0){
	            if(!flag)
	                return ((double)nums2[twoLength / 2 - 1] + (double)nums2[twoLength / 2]) / 2.0;
	            else
	                return  (double)nums2[twoLength / 2];
	        }
	        else if(twoLength == 0){
	            if(!flag)
	                return ((double)nums1[oneLength / 2 - 1] + (double)nums1[oneLength / 2]) / 2.0;
	            else
	                return  (double)nums1[oneLength / 2];
	        }
	        if(length == 2){
	            return ( (double)nums1[0] + (double)nums2[0] ) / 2.0;
	        }
	
	        int ant = 0;
	        double ans1 = 0;
	        double ans2 = 0;
	        int onePos = 0;
	        int twoPos = 0;
	
	        while(true){
	            ant = Math.min(nums1[onePos],nums2[twoPos]);
	            if( (onePos + twoPos) == length / 2 && flag ){
	                return (double)ant;
	            }else if( (onePos + twoPos) == length / 2 - 1 && !flag ){
	                ans1 = (double)ant;
	            }else if( (onePos + twoPos) == length / 2 && !flag ){
	                ans2 = (double)ant;
	                return (ans1 + ans2) / 2;
	            }
	
	            
	            if(nums1[onePos] <= nums2[twoPos] && onePos == oneLength - 1){
	                int temp = nums1[onePos];
	                nums1[onePos] = nums2[twoPos];
	                nums2[twoPos] = temp;
	                twoPos++;
	            }else if(nums1[onePos] >= nums2[twoPos] && twoPos == twoLength - 1){
	                int temp = nums1[onePos];
	                nums1[onePos] = nums2[twoPos];
	                nums2[twoPos] = temp;
	                onePos++;
	            }else if(nums1[onePos] >= nums2[twoPos]){
	                twoPos++;
	            }else if(nums1[onePos] <= nums2[twoPos]){
	                onePos++;
	            }
	        }   
	    }
	}
```



# 23.合并K个升序链表
## 题目
https://leetcode-cn.com/problems/merge-k-sorted-lists

## 题解
优先队列的写法
## 代码
**暴力模拟：**
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        int[] pos = new int[lists.length];
        int tempMinPosi = 0;
        int nums = 0;
        ListNode ans = new ListNode(10000);
        for(int i = 0; i < lists.length; i++){
            if(lists[i] != null){
                if(lists[i].val < ans.val){
                    tempMinPosi = i;
                    ans = lists[i];
                }
            }
            else nums++;
        }
        if(nums == lists.length)
            return null;
        ListNode result = ans;
        lists[tempMinPosi] = lists[tempMinPosi].next;

        while(true)
        {
            ListNode tempMin = new ListNode(10000);
            tempMinPosi = 0;
            nums = 0;
            for(int i = 0; i < lists.length; i++){
                if(lists[i] != null){
                    if(lists[i].val < tempMin.val){
                        tempMinPosi = i;
                        tempMin = lists[i];
                    }
                }
                else{
                    nums++;
                }
            }
            if(nums == lists.length)
                break;
            ans.next = tempMin;
            ans = ans.next;
            lists[tempMinPosi] = lists[tempMinPosi].next;
        }
        return result;
    }
}
```

**优先队列：**
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> q = new PriorityQueue<>((x,y)->x.val-y.val);
        for(ListNode node : lists){
            if(node != null){
                q.add(node);
            }
        }
        ListNode head = new ListNode();
        ListNode tail = head;
        while(!q.isEmpty()){
            tail.next = q.poll();
            tail = tail.next;
            if (tail.next != null){
                q.add(tail.next);
            }
        }
        return head.next;
    }
}
```


# 25.K个一组翻转链表
## 题目

https://leetcode-cn.com/problems/reverse-nodes-in-k-group/

## 题解
**思路：**
用ArrayList存取所有的ListNode  然后看ListNode的长度是不是k的整数倍，不是话把最大整数倍的后一个节点取出来（后面会用到），然后把不是整数倍的节点在List中删除掉，接下里就是反转k个一组了，用一个while里面套取for来反转，下面举例子说一下反转过程。

**例子：** 1->2->3->4->5，k = 3
1.首先把1->2->3->4->5存到ArrayList中，然后得到ArrayList长度为5不是k（3）的整数倍，所以删除最后一个节点，然后还剩下4个节点此时ArrayList.size() % k = 1,所以把4节点存下来，后面需要用他来连接4和5节点，存下来后在ArrayList中 把4也删除掉，现在ArrayList中只有
1->2->3了
2.判断while(ArrayList.size() != 0) 所以进入while进行k个一组的反转，先把ArrayList中最后一个元素3存下里，然后删除掉ArrayList中的3
3.for
- 1.进入for(i = 1, i < k; i++)，让刚刚存取的3节点的next指向此时ArrayList中最后一个元素2，然后存下来2，删除ArrayList中的2；
- 2.再判断for循环条件，还在for里面，让刚刚存取的2节点的next指向此时ArrayList中最后一个元素1，然后存下来1，删除ArrayList中的1；
- 3.再判断for循环条件，此时不满足了，跳出for，再去判断while(ArrayList.size() != 0)，也不满足了，所以结束

返回我们存取的最后一次while循环里面存放的3节点
## 代码

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        List<ListNode> node = new ArrayList<>();
        while(head != null){
            node.add(head);
            head = head.next;
        }
        ListNode last = null;
        if(node.size() % k != 0){
            while(node.size() % k != 1){
                node.remove(node.size() - 1);
            }
            last = node.get(node.size() - 1);
            node.remove(node.size() - 1);
        }
        ListNode temp = null;
        ListNode tempLast = null;
        while(node.size() != 0){ 
            temp = node.get(node.size() - 1);
            tempLast = temp;
            node.remove(node.size() - 1);
            for(int i = 1; i < k; i++){
                temp.next = node.get(node.size() - 1);
                node.remove(node.size() - 1);
                temp = temp.next;
            }
            temp.next = last;
            last = tempLast;
        }
        return tempLast;
    }
}
```

# 65.有效数字
## 题目
https://leetcode-cn.com/problems/valid-number/

## 题解
记录一下当前的字母和上一个字母，还有'e'或'E'出现过没有,'.'出现过没有，'数字'出现过没有
## 代码

```java
class Solution {
    public boolean isNumber(String s) {
        char last = ' ', temp;
        boolean flag = false, num = false, fleg = false;
        for(int i = 0;i < s.length(); i++){
            temp = s.charAt(i);
            if((temp == 'e' || temp == 'E' || temp == '.' || temp == '+' || temp == '-') || (temp >= '0' && temp <= '9')){
                if((temp == 'e' || temp == 'E') && !fleg && num && ((last >= '0' && last <= '9') || last == '.') ) {flag = true; fleg = true;}
                else if((temp == '+' || temp == '-') && (last == ' ' || last == 'e' || last == 'E')) ;
                else if(temp == '.' && !flag && (last == ' ' || last == '+' || last == '-' || (last >= '0' && last <= '9'))) flag = true;
                else if(temp >= '0' && temp <= '9') num = true;
                else return false;
                last = temp;  
            }
            else return false;
        }
        if(last == 'e' || last == 'E' || last == '+' || last == '-' || !num) return false;
        return true;
    }
}
```

# 149.直线上最多的点数
## 题目
https://leetcode-cn.com/problems/max-points-on-a-line/

## 题解
定义一个HashMap,key为斜率值，value为该斜率下在同一条直线上有多少点
计算每一个点及其后面点是否同斜率（即在同一条直线上），最后返回最大的value即可
## 代码

```java
class Solution {
    public int maxPoints(int[][] points) {  
        int result = 1;
        for(int i = 0; i < points.length; i++){
            if(result >= points.length - i) break;
            double x1 = points[i][0], y1 = points[i][1];
            Map<Double, Integer> temp = new HashMap<>();
            for(int j = i + 1; j < points.length; j++){
                double x2 = points[j][0], y2 = points[j][1];
                double k = 0.0;
                if(x1 == x2) k = 10001.0;
                else if(y1 == y2) ;
                else k = (y2 - y1) / (x2 - x1);
                int num = temp.getOrDefault(k, 1);
                temp.put(k, num + 1);
                result = Math.max(result, num + 1);
            }
        }
        return result;
    }
}
```

# 154.寻找旋转排序数组中的最小值 II
## 题目
https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii

## 题解
**二分：**
每次对比都是**左侧元素**和**中间元素**对比，**共三种情况**：
**1.左 == 中：**此时最小值**可能在[左，mid]中**，**也可能在[mid + 1, r]中**，更新一下结果 ，左 = 左 + 1
**2.左 < 中：**此时最小值**仅可能**是**左或在[mid + 1，r]中**，更新一下结果 ，左 = mid + 1
**3.左 > 中：**此时最小值**仅可能**是**中或在[l，mid - 1]中**，更新一下结果 ，右 = mid - 1

**三种情况例子：**
**1.**[3，**1**，3，3，3]或[3，3，3，3，**2**]，3 == 3(nums[0] == nums[2])，最小值1在[左，mid]中 **或** 最小值2在[mid + 1, r]中
**2.**[**1**，2，3，4，5]或[1，2，3，4，**0**]，1 < 3(nums[0] < nums[2])，最小值1是左 **或** 最小值0在[mid + 1，r]中
**3.**[3，4，**2**，3，3]或[3，**1**，2，3，3]，3 > 2(nums[0] > nums[2])，最小值2是中 **或** 最小值1在在[l，mid - 1]中

## 代码


```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int result = nums[0];
        int l = 0, r = n - 1;
        while(l <= r){
            int mid = (l + r + 1) >> 1;
            if(nums[l] == nums[mid]){
                result = Math.min(result, nums[mid]); 
                l++;
            }else if(nums[l] < nums[mid]){
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

# 297.二叉树的序列化与反序列化
## 题目
https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/
## 题解
**序列化：** 用bfs存每一层的节点（也要把val存在String中），注意叶子节点的孩子（也就是空）要存下来，反序列化时需要用
**反序列化：** bfs的逆过程

## 代码

```java
public class Codec {
    public String serialize(TreeNode root) {
        StringBuffer s = new StringBuffer();
        List<TreeNode> next = new ArrayList<TreeNode>();
        if(root != null) next.add(root);
        while(next.size() != 0){
            List<TreeNode> temp = new ArrayList<TreeNode>(next);
            next.clear();
            for(int i = 0; i < temp.size(); i++){
                TreeNode tempNode = temp.get(i);
                if(tempNode != null){
                    s.append(String.valueOf(tempNode.val) + " ");
                    next.add(tempNode.left);
                    next.add(tempNode.right);
                } else {
                    s.append("n ");
                }
            }
        }
        return s.toString();
    }
    public TreeNode deserialize(String data) {
        if(data.length() == 0) return null;

        TreeNode root = null;
        List<TreeNode> temp = new ArrayList<TreeNode>();
        TreeNode tempNode = null;
        boolean flag = false;
        for(int i = 0; i < data.length(); i++){
            if(data.charAt(i) == '-'){
                flag = true;
            } else if(data.charAt(i) == 'n'){
                if(tempNode == null) {
                    tempNode = temp.remove(0);
                }
                else tempNode = null;
                i++;
            } else {
                int val = 0;
                while(data.charAt(i) != ' '){
                    val = val * 10 + data.charAt(i) - '0';
                    i++;
                }
                if(flag) {
                    val = -val;
                    flag = false;
                }
                if(root == null){
                    root = new TreeNode(val);
                    temp.add(root);
                } else{
                    if(tempNode == null){
                        tempNode = temp.remove(0);
                        TreeNode nodeLeft = new TreeNode(val);
                        tempNode.left = nodeLeft;
                        temp.add(nodeLeft);
                    } else {
                        TreeNode nodeRight = new TreeNode(val);
                        tempNode.right = nodeRight;
                        temp.add(nodeRight);
                        tempNode = null;
                    }
                }
            }          
        }
        return root;
    }
}
```

# 483.最小好进制
## 题目
https://leetcode-cn.com/problems/smallest-good-base/
## 题解
可以推出n的符合题意的除了(n - 1)进制外的最大进制不会超过Math.log(n) / Math.log(2)，这样就大大减小了遍历次数，然后从小到大判断一遍符合直接输出，都不符合说明需要返回对每一个数字都符合的(n - 1)进制
## 代码

```java
class Solution {
    public String smallestGoodBase(String n) {
        long num = Long.parseLong(n);
        int max = (int) (Math.log(num) / Math.log(2)), k;
        for (int i = 2; i <= max; i++) {
            k = (int) Math.pow(num, 1.0 / i);
            if (num == sum(k, i, 1)) return Integer.toString(k);
        }
        return Long.toString(num - 1);
    }

    private long sum(long k, long t, long sum) {
        for (int i = 0; i < t; i++) sum = sum * k + 1;
        return sum;
    }
}
```

# 664.奇怪的打印机
## 题目
https://leetcode-cn.com/problems/strange-printer/
## 题解

`dp[i][j]`表示的是区间`[i,j]`内的**字符需要的最少打印次数**，那么对于区间`[left,right]`我们进行分析:
1.如果区间`[left,right]`中`left`位置的字符 **等于** `right`位置的字符，此时`dp[left][right] = dp[left][right - 1]`,
- **举例:** 
- `s = "aaab",left = 0, right = 2,dp[0][0] = 1,dp[0][1] = 1`
- 那么`dp[left][right] = dp[0][2] = dp[0][1] = 1;`

2.如果区间`[left,right]`中`left`位置的字符 **不等于** `right`位置的字符，此时`dp[left][right] = min(dp[left][mid - 1] + dp[mid][right]) (left < mid <= right)`
- **举例:** 
- `s = "aaab",left = 0, right = 3,dp[0][0] = 1,dp[0][1] = 1,dp[0][2] = 1`
- 那么`dp[left][right] = min(dp[left][mid - 1] + dp[mid][right]) = min(dp[0][0] + dp[1][3], dp[0][1] + dp[2][3], dp[0][2] + dp[3][3]) = 2;`

## 代码

```java
class Solution {
    public int strangePrinter(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];
        for(int i = 0; i < n; i++) dp[i][i] = 1;
        for(int right = 0; right < n; right++){
            for(int left = right - 1; left >= 0; left--){
                if(s.charAt(left) == s.charAt(right)) dp[left][right] = dp[left][right - 1];
                else{
                    int min = Integer.MAX_VALUE;
                    for(int mid = right; mid > left; mid--) 
                        min = Math.min(min, dp[left][mid - 1] + dp[mid][right]);          
                    dp[left][right] = min;
                }
                
            }
        }
        return dp[0][n - 1];
    }
}
```

# 773.滑动谜题
## 题目
https://leetcode-cn.com/problems/sliding-puzzle/
## 题解
把这六个格子变成字符串存起来每一步可能变成啥，然后变成过啥也存起来，直到到结果

## 代码

```java
class Solution {
    public int slidingPuzzle(int[][] board) {
        Set<String> dead = new HashSet<>();
        int result = 0;
        Set<String> ant = new HashSet<>();
        ant.add(String.valueOf(board[0][0]) + String.valueOf(board[0][1]) + String.valueOf(board[0][2]) + String.valueOf(board[1][0]) + String.valueOf(board[1][1]) + String.valueOf(board[1][2]));
        while(result < 20){
            if(ant.contains("123450")) return result;
            else result++;
            Set<String> temp = new HashSet<>(ant);
            ant.clear();
            Iterator iter = temp.iterator();
            while(iter.hasNext()){
                String tempS = (String)iter.next();
                dead.add(tempS);
                int zeroPosi = tempS.indexOf("0");
                StringBuffer sb1 = new StringBuffer(tempS);
                StringBuffer sb2 = new StringBuffer(tempS);
                if(zeroPosi < 3){
                    sb1.setCharAt(zeroPosi, sb1.charAt(zeroPosi + 3));
                    sb1.setCharAt(zeroPosi + 3, '0');
                } else {
                    sb1.setCharAt(zeroPosi, sb1.charAt(zeroPosi - 3));
                    sb1.setCharAt(zeroPosi - 3, '0');
                }
                if(zeroPosi == 0 || zeroPosi == 3){
                    sb2.setCharAt(zeroPosi, sb2.charAt(zeroPosi + 1));
                    sb2.setCharAt(zeroPosi + 1, '0');
                } else if(zeroPosi == 2 || zeroPosi == 5){
                    sb2.setCharAt(zeroPosi, sb2.charAt(zeroPosi - 1));
                    sb2.setCharAt(zeroPosi - 1, '0');
                } else {
                    sb2.setCharAt(zeroPosi, sb2.charAt(zeroPosi + 1));
                    sb2.setCharAt(zeroPosi + 1, '0');
                    StringBuffer sb3 = new StringBuffer(tempS);
                    sb3.setCharAt(zeroPosi, sb3.charAt(zeroPosi - 1));
                    sb3.setCharAt(zeroPosi - 1, '0');
                    if(!dead.contains(sb3.toString())) ant.add(sb3.toString());
                }
                if(!dead.contains(sb1.toString())) ant.add(sb1.toString());
                if(!dead.contains(sb2.toString())) ant.add(sb2.toString());
            }
        }
        return -1;
    }
}
```

# 879.盈利计划
## 题目
https://leetcode-cn.com/problems/profitable-schemes/
## 题解
dp[i][j] :可用员工个数为i时，利润>=j的个数
for循环一下group、员工以及利润
## 代码

```java
class Solution {
    public int profitableSchemes(int n, int minProfit, int[] group, int[] profit) {
        
        int[][] dp = new int[n + 1][minProfit + 1];
        for(int i = 0; i <= n; i++) dp[i][0] = 1;
        
        for(int num = 0; num < group.length; num++){
            for(int i = n; i >= group[num]; i--){
                for(int j = minProfit; j >= 0; j--){
                    dp[i][j] = (dp[i][j] + dp[i - group[num]][Math.max(0, j - profit[num])]) % 1000000007;
                }
            }
        }
        return dp[n][minProfit];
    }
}
```

# 810.黑板异或游戏
## 题目
https://leetcode-cn.com/problems/chalkboard-xor-game/

## 题解
**思路：**
先手和后手肯定都不会想去擦除一个元素来使得剩余的结果异或和为0，**除非** 最后只剩下一个元素了  **或者** 初始数组[2,2,2,2,2] 这种情况（但是这种情况只可能会是初始数组长度为奇数，并且Alice输了，因为他先手去哪一个都会使得剩下的异或为0）
**分情况讨论：**
1.如果数组长度为偶数，先手的一方也就是Alice肯定会赢，因为数组长度为偶数去掉一个不可能立即使得剩下的异或和为0，所以最终肯定是Bob把最后一个擦掉，然后Alic赢了：
- 举例：[1,2,3,4]  Alice先手肯定不会去掉4，不然他就输了，所以他会去掉1，2，3中的一个，然后Bob再去掉一个，最后肯定是Bob去掉的最后一个，Alice赢了

2.如果数组长度为奇数，先手的一方也就是Bob一般也会赢，和上面同样的情况（但是有一个是例外的就是初始数组异或和本来就为0，那么Alice上来就赢了）

**总结：**
Bob（后手）仅一种情况赢，那就是数组长度为奇数 **而且** 初始数组异或和不为0
## 代码

```java
class Solution {
    public boolean xorGame(int[] nums) {
        if(nums.length % 2 == 1){
            for(int i = 1; i < nums.length; i++) nums[0] ^= nums[i];
            if(nums[0] != 0) return false;
        }
        return true;
    }
}
```

# 1074.元素和为目标值的子矩阵数量
## 题目
https://leetcode-cn.com/problems/number-of-submatrices-that-sum-to-target/

## 题解
1.先创建一个数组存一下矩阵`[0,i][0,j]`的矩阵和(存的时候注意加上两个小一号矩阵时会多加一部分，所以需要减去，其实跟下面暴力求解中求任意矩阵的和思路类似，可结合第二步理解这个公式)

2. **暴力上up下down左left右right矩阵边界** ，如上图：
求红色的矩阵和`[up,down][left,right]`，用矩阵和`[0,down][0,right]`**减去两个褐色的矩阵和**`[0,up][0,right]`、`[0,down][0,left]`，因为**多减去了**一遍紫色矩阵和，所以需要**再加上**`[0,up][0,left]`，然后判断该**矩阵和的值是否与target值相同**，相同结果`+1`


## 代码

```java
class Solution {
    public int numSubmatrixSumTarget(int[][] matrix, int target) {
        int n = matrix.length + 1;
        int m = matrix[0].length + 1;
        int ans = 0;
        int[][] result = new int[n][m];
        for(int i = 1; i < n; i++)
            for(int j = 1; j < m; j++)
                result[i][j] = matrix[i - 1][j - 1] + result[i - 1][j] + result[i][j - 1] - result[i - 1][j - 1];

        for(int down = 1; down < n; down++)
            for(int right = 1; right < m; right++)
                for(int up = 0; up < down; up++)
                    for(int left = 0; left < right; left++)
                        if((result[down][right] - result[up][right] - result[down][left] + result[up][left]) == target)
                            ans++;
        return ans;
    }
}
```

# 1269.停在原地的方案数
## 题目
https://leetcode-cn.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps

## 题解

**思路：**
- **动态规划公式** dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j] + dp[i - 1][j + 1]:
- **说明：** i表示**步数**，j表示**位置**，dp[i][j] 表示第i步到达j位置的方案数，此时因为只能从i - 1步的 j - 1 位置、j 位置、 j + 1 位置到达，公式由此得到。

**代码优化：**
- **1.创建数组的行数：** 因为第 i 步和上一步的结果有关，所以可以用两个一维数组来存取，这里直接创建一个二维数组 然后加上**位运算来分上下步**；
- **2.创建数组的列数：** 二维数组两行的话，那应该多少列呢？最简单的就是arrLen列了，但是我们可以优化一下，因为你如果想要返回起点，肯定**走到一半步数时**就要往回走了，不然就无法回到起点，所以**最多就走到一半的位置**，所以我们**取Math.min(steps / 2 + 1, arrLen)来创建数组**；
- **3.创建数组的列数（修正）：** 然后写代码时会发现，第i步j位置和 i - 1的j - 1位置 **以及** i - 1 的 j + 1位置有关，那么他有可能超出数组长度的上界和下界（即j为0 **或者** j为一半位置时），那么我们创建数组时**列空间再 +2**好了，**用数组列下标1表示起始位置，列下标arrLen表示终止位置**；
- **4.数据初始化：** 起始时，即0步1位置（也就是0步初始位置）要初始化为1（result[0][1] = 1;）
- **5.位运算须知：** 奇 & 1 = 1 ，偶数 & 1 = 0，所以for循环中的行进行位运算实现了上一步和下一步的转换

## 代码
```java
class Solution {
    public int numWays(int steps, int arrLen) {
        int  min = Math.min(steps / 2 + 1, arrLen);
        int[][] result = new int[2][min + 2];
        result[0][1] = 1;
        for(int i = 1; i <= steps; i++){
            for(int j = 1; j <= min; j++){
                result[i & 1][j] =( ( result[(i - 1) & 1][j - 1] + result[(i - 1) & 1][j] ) % (1000000007) + result[(i - 1) & 1][j + 1] ) % (1000000007);
            }
        }
        return result[steps & 1][1];
    }
}
```


# 1449.数位成本和为目标值的最大数字
## 题目
https://leetcode-cn.com/problems/form-largest-integer-with-digits-that-add-up-to-target/

## 题解
result[i]:表示和为i时存储的最长字串
思路就是判断长度，最后需要把数字大的提到前面来
## 代码

```java
class Solution {
    public String largestNumber(int[] cost, int target) {
        StringBuffer[] result = new StringBuffer[target + 1];
        for(int i = 0; i <= target; i++) result[i] = new StringBuffer();
        for(int i = 0; i < 9; i++){
            if(cost[i] > target) continue;
            StringBuffer t = new StringBuffer(String.valueOf(i + 1));
            if(result[cost[i]].length() < t.length()) result[cost[i]] = t;
            else if(result[cost[i]].length() == t.length() && t.compareTo(result[cost[i]]) > 0) result[cost[i]] = t;
            int iLength = result[cost[i]].length();
            for(int j = cost[i] + 1; j <= target; j++){
                int jLength = result[j].length();
                int jiLength = result[j - cost[i]].length();
                if(jLength < jiLength + iLength && jiLength != 0 && iLength != 0){
                    result[j] = new StringBuffer(result[j - cost[i]].toString() + result[cost[i]].toString());
                }
                else if(jLength == jiLength + iLength && jiLength != 0 && iLength != 0){
                    StringBuffer a = new StringBuffer(result[j - cost[i]].toString() + result[cost[i]].toString());
                    StringBuffer b = new StringBuffer(result[cost[i]].toString() + result[j - cost[i]].toString());
                    if(a.compareTo(b) < 0) a = b;
                    if(a.compareTo(result[j]) > 0) result[j] = a;
                }
            }
        }
        if(result[target].length() == 0) return "0";
        else {
            int[] num = new int[10];
            for(int i = 0; i < result[target].length(); i++){
                num[result[target].charAt(i) - '0']++;
            }
            result[target].delete(0, result[target].length());
            for(int i = 9; i > 0; i--){
                while(num[i]-- > 0) result[target].append(i);
            }
            return result[target].toString();
        }
    }
}
```

# 1723.完成所有工作的最短时间（dfs+剪枝多写）**
## 题目
https://leetcode-cn.com/problems/find-minimum-time-to-finish-all-jobs/

## 题解

https://leetcode-cn.com/problems/find-minimum-time-to-finish-all-jobs/solution/gong-shui-san-xie-yi-ti-shuang-jie-jian-4epdd/

1.dfs（TL）
2.dfs+剪枝
3.模拟退火
4.状压dp

## 代码

**dfs无剪枝（会超时）：**
```java
	class Solution {
	    int num;
	    int[] job;
	    int kk;
	    int ans = Integer.MAX_VALUE;
	    public int minimumTimeRequired(int[] jobs, int k) {
	        num = jobs.length;
	        job = jobs;
	        kk = k;
	
	        //工人长度的数组，dfs相当于给工人分任务
	        int[] sum = new int[kk];
	        dfs(0, sum, 0);
	        return ans;
	    }
	    public void dfs(int cur, int[] sum, int max){
	        if(max >= ans) return ;
	        if(cur == num){
	            ans = max;
	            return ;
	        }
	        for(int i = 0; i < kk; i++){
	            if(sum[i] == 0){
	                sum[i] += job[cur];
	                dfs(cur + 1, sum, Math.max(max,sum[i]));
	                sum[i] -= job[cur];
	            }
	        }
	        
	
	        for(int i = 0; i < kk; i++){
	            sum[i] += job[cur];
	            dfs(cur + 1, sum, Math.max(max,sum[i]));
	            sum[i] -= job[cur];
	        }
	        
	    }
	}
```
**dfs剪枝：**
```java
	class Solution {
	    int num;
	    int[] job;
	    int kk;
	    int ans = Integer.MAX_VALUE;
	    public int minimumTimeRequired(int[] jobs, int k) {
	        num = jobs.length;
	        job = jobs;
	        kk = k;
	
	        //工人长度的数组，dfs相当于给工人分任务
	        int[] sum = new int[kk];
	        dfs(0, 0, sum, 0);
	        return ans;
	    }
	    public void dfs(int cur, int used, int[] sum, int max){
	        if(max >= ans) return ;
	        if(cur == num){
	            ans = max;
	            return ;
	        }

			//剪枝
	        if(used < kk){
	            sum[used] += job[cur];
	            dfs(cur + 1, used + 1, sum, Math.max(max,sum[used]));
	            sum[used] = 0;
	        }
	        //剪枝结束
			
			//剪枝中uesd也改了
	        for(int i = 0; i < used; i++){
	            sum[i] += job[cur];
	            dfs(cur + 1, used, sum, Math.max(max,sum[i]));
	            sum[i] -= job[cur];
	        }
	        
	    }
	}
```

# 1862.向下取整数对和
## 题目
https://leetcode-cn.com/problems/sum-of-floored-pairs/

## 题解

**举个例子：** nums = [2,5,9]
我们创建一个数组number[] 长度为nums中最大值+1，这里就是10,然后number初始化：
number[0] = 0, number[1] = 0, number[2] = 1, number[3] = 1, number[4] = 1, 
number[5] = 2, number[6] = 2, number[7] = 2, number[8] = 2, number[9] = 3, 
对的，就是把截止到现在在nums中<=当前number下标的数字出现的次数，然后我们就可以用来求取结果了，对于2来说，我们分段求取[2,3]是÷2 = 1的，[4,5]是÷2 = 2的，[6,7]是÷2 = 3的，[8,9]是÷2 = 4的，分段就能够求取出所有除以2的结果的和，然后再去求5，求9，结束
## 代码

```java
class Solution {
    public int sumOfFlooredPairs(int[] nums) {
        Arrays.sort(nums);
        int maxNum = nums[nums.length - 1];
        int[] number = new int[maxNum + 1];
        
        long res = 0;
        for(int num : nums){number[num]++;}
        for(int i = 1; i <= maxNum; i++){number[i] += number[i - 1];}
        for(int i = 0; i < nums.length; i++){
            for(int j = 1; j < maxNum / nums[i] + 1; j++){
                res += (number[Math.min(nums[i] * (j + 1) - 1, maxNum)] - number[nums[i] * j - 1]) * j;
                res %= 1000000007;
            }
        }
        return (int)res;
    }
}
```