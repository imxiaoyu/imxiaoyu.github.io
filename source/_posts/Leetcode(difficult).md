---
title: 'Leetcode(difficult)'
date: 2021/5/18 0:00:26 
tags:
	- Leetcode
---
>**题目列表：**
>4.寻找两个正序数组的中位数
>23.合并K个升序链表（优先队列）
>25.K个一组翻转链表(模拟)
>154.寻找旋转排序数组中的最小值 II（二分）
>810.黑板异或游戏
>1269.停在原地的方案数（dp）
>1723.完成所有工作的最短时间（dfs+剪枝多写）**
>1862.向下取整数对和 （化简遍历数目）



664.奇怪的打印机
1707.与数组中元素的最大异或值
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