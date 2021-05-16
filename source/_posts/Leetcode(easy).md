---
title: 'Leetcode(easy)'
date: 2021/5/6 23:46:00 
tags:
	- Leetcode
---
>**题目列表：**
>1.两数之和
>7.整数反转
>9.回文数
>1486.数组异或操作
>1480.一维数组的动态和
>1720.解码异或后的数组
>剑指offer 58 - II 左旋转字符串
>面试题 02.03 删除中间节点


简单的不再更新，以后只更新中等和困难，我LeetCode也都写了题解，Leetcode个人网页
https://leetcode-cn.com/u/rain-ru/
<!-- more -->
# 1.两数之和
## 题目
https://leetcode-cn.com/problems/two-sum
## 题解
## 代码

### java暴力
```java
	class Solution {
	    public int[] twoSum(int[] nums, int target) {
	        int numsLength = nums.length;
	        for(int i = 0; i < numsLength; i++){
	            for(int j = i + 1; j < numsLength; j++){
	                if(nums[i] + nums[j] == target){
	                    return new int[]{i,j};
	                }
	            }
	        }
	        return new int[]{0,0};
	    }
	    
	}
```
### HashMap优化
```java
	class Solution {
	    public int[] twoSum(int[] nums, int target) {
	        Map<Integer,Integer> hashMap = new HashMap<>();
	            int numsLength = nums.length;
	
	            for(int i = 0; i < numsLength; i++){
	                if(hashMap.containsKey(target - nums[i])){
	                    return new int[]{hashMap.get(target - nums[i]),i};
	                }
	                hashMap.put(nums[i],i);
	            }
	            return new int[]{0,0};
	    }
	    
	}
```

# 7.整数反转

## 题目
https://leetcode-cn.com/problems/reverse-integer
## 题解
## 代码
简单的取余 *10 复杂度反而小
```java
	class Solution {
	    public int reverse(int x) {
	        boolean flag = x > 0 ? true : false;
	            long temp = Math.abs(x);
	            long result = 0;
	            while(temp > 0){
	                result = result * 10 + temp % 10;
	                temp = temp / 10;
	            }
	            if(!flag){
	                result = -result;
	            }
	            if(result > Integer.MAX_VALUE || result < Integer.MIN_VALUE)
	                return 0;
	            else
	                return (int)result;
	        
	    }
	}
```
# 9.回文数

## 题目
https://leetcode-cn.com/problems/palindrome-number/
## 题解
简单模拟，数字转为 String,然后从左右开始判断
## 代码
```java
	class Solution {
	    public boolean isPalindrome(int x) {
	        int l, r;
	        String s = new String(Integer.toString(x));
	        for(l = 0 , r = s.length() - 1; l < r; l++, r--){
	            if(s.charAt(l) != s.charAt(r)){
	                return false;
	            }
	        }
	        return true;
	    }
	}
```
# 1486.数组异或操作

## 题目
https://leetcode-cn.com/problems/xor-operation-in-an-array/
## 题解
## 代码
### 傻瓜方法
```java
	class Solution {
	    public int xorOperation(int n, int start) {
	        int result = start;
	        for(int i = start + 2; i < start + 2 * n; i += 2){
	            result = result ^ i;
	        }
	        return result; 
	    }
	}
```
### 优化方法：数学来做(题解)

# 1480.一维数组的动态和

## 题目
https://leetcode-cn.com/problems/running-sum-of-1d-array
## 题解
没啥说的，太简单
## 代码
```java
	class Solution {
	    public int[] runningSum(int[] nums) {
	        int numsLength = nums.length;
	        for(int i = 1; i < numsLength; i++){
	            nums[i] += nums[i - 1];
	        }
	        return nums;
	    }
	}
```
# 1720.解码异或后的数组

## 题目
https://leetcode-cn.com/problems/decode-xored-array
## 题解
## 代码

简单的模拟：
```java
	class Solution {
	    public int[] decode(int[] encoded, int first) {
	        int arrLength = encoded.length + 1;
	        int[] arr = new int[arrLength];
	        int temp = 0;
	        arr[temp] = first;
	        while(temp < arrLength - 1){
	            arr[++temp] = arr[temp - 1] ^ encoded[temp - 1];
	        }
	        return arr;
	    }
	}
```


# 剑指offer 58 - II 左旋转字符串

## 题目
https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof
## 题解
StringBuffer.append()的使用
## 代码
```java
	class Solution {
	    public String reverseLeftWords(String s, int n) {
	        StringBuffer sb = new StringBuffer();
	            int sLength = s.length();
	            sb.append(s,n,sLength);
	            sb.append(s,0,n);
	            return sb.toString();
	    }
	}
```
# 面试题 02.03 删除中间节点
## 题目
https://leetcode-cn.com/problems/delete-middle-node-lcci
## 题解
	//将这个结点的数据替换为下一个结点
	node.val = node.next.val;
	//删除下一个结点
	node.next = node.next.next;


## 代码
```java
	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public void deleteNode(ListNode node) {
	            node.val = node.next.val;
	            node.next = node.next.next;       
	    }
	}
```

