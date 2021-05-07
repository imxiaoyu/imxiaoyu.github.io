---
title: 'Leetcode(difficult)'
date: 2021/5/7 22:37:04 
date: 2021/5/6 22:37:04 
tags:
	- Leetcode
	- 困难题目
---
# 4.寻找两个正序数组的中位数
## 题目
https://leetcode-cn.com/problems/median-of-two-sorted-arrays/

<!-- more -->
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