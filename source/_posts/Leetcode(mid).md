---
title: 'Leetcode(mid)'
date: 2021/5/7 13:29:32  
tags:
	- Leetcode
	- 中等题目
---
# 2.两数相加
## 题目
## 题解

创建一个链表，直接while循环中判断创建并把结果加到链表中；
代码中有思路；
代码用时2ms 超过100%用户；
代码内存消耗超过70.9%用户.

<!-- more -->
## 代码

	```java
	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode() {}
	 *     ListNode(int val) { this.val = val; }
	 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
	 * }
	 */
	class Solution {
	    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
	        //存储结果链表的表头
	        ListNode ans = new ListNode();
	        //判断是否需要进位
	        boolean flag = false;
	        //用此链表在while中循环使用，用作现在的链表
	        ListNode result = ans;
	
	        while(l1 != null || l2 != null || flag){
	            
	            if(l1 != null && l2 != null){
	                if(flag){
	                    if(l1.val + l2.val + 1 > 9){
	                        flag = true;
	                        result.val = l1.val + l2.val + 1 - 10;
	                    }else{
	                        flag = false;
	                        result.val = l1.val + l2.val + 1;
	                    }
	                }else{
	                    if(l1.val + l2.val > 9){
	                        flag = true;
	                        result.val = l1.val + l2.val - 10;
	                    }else{
	                        flag = false;
	                        result.val = l1.val + l2.val;
	                    }
	                }
	                l1 = l1.next;
	                l2 = l2.next;
	                
	            }else if(l1 != null){
	                if(flag){
	                    if(l1.val + 1 > 9){
	                        flag = true;
	                        result.val = l1.val + 1 - 10;
	                    }else{
	                        flag = false;
	                        result.val = l1.val + 1;
	                    }
	                }else{
	                    result.val = l1.val;
	                }
	                l1 = l1.next;
	
	            }else if(l2 != null){
	                if(flag){
	                    if(l2.val + 1 > 9){
	                        flag = true;
	                        result.val = l2.val + 1 - 10;
	                    }else{
	                        flag = false;
	                        result.val = l2.val + 1;
	                    }
	                }else{
	                    result.val = l2.val;
	                }
	                l2 = l2.next;
	
	            }else{
	                flag = false;
	                result.val = 1;
	            }
	            if(l1 != null || l2 != null || flag){
	                ListNode ant = new ListNode();
	                result.next = ant;
	                result = ant;
	            }
	        }
	        return ans;
	
	
	
	    }
	}
	```