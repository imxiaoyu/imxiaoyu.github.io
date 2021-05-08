---
title: 'Leetcode(difficult)'
date: 2021/5/7 22:37:04 
tags:
	- Leetcode
	- 困难题目
---
>**题目列表：**
>4.寻找两个正序数组的中位数
>1723.完成所有工作的最短时间（dfs+剪枝多写）**


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

**方法二：**

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

**dfs剪枝：**

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