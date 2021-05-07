---
title: 'Leetcode(mid)'
date: 2021/5/7 13:29:32  
tags:
	- Leetcode
	- 中等题目
---
# 2.两数相加
## 题目
https://leetcode-cn.com/problems/add-two-numbers
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


# 3.无重复字符的最长子串
## 题目
https://leetcode-cn.com/problems/longest-substring-without-repeating-characters
## 题解

	一共两种情况：
	1.如果哈希Map中发现了当前位置的字符：
	    两种情况：
	    - 1.如果在哈希Map中发现的位置在当前最大字串之前（说明当前最大字串中没有这个字符）：
	        则更新哈希Map中的该字符位置，并且当前最长字串长度+1
	    - 2.如果在哈希Map中发现的位置在当前最大字串之后（说明当前最大字串中有这个字符）：
	        首先对比也下之前的最大长度和现在长度，大的赋给截至到当前的最大长度；
	        然后当前最大长度更新为当前位置减去哈希Map中该字符的位置；
	        再跟新当前最大字符串的起始位置；
	        最后更新哈希Map中该字符的当前位置。
	2.如果哈希Map中没发现当前位置的字符：字符-位置 加入到哈希Map，并且当前最长字串长度+1


## 代码

	class Solution {
	    public int lengthOfLongestSubstring(String s) {
	        Map<Character,Integer> hashMap = new HashMap<>();
	        //当前字符数组的坐标
	        int temp = 0;
	        //当前最长字串长度
	        int tempAns = 0;
	        //当前最大字串长度的起始位置
	        int tempPosi = 0;
	        //到目前为止最大长度
	        int ans = 0;
	        //传入的字符串的长度
	        int sLength = s.length();
	
	        //当前坐标小于字符串长度进入
	        while(temp < sLength){
	            //两种情况：
	            // 1.如果哈希Map中发现了当前位置的字符：
	            //     两种情况：
	            //     1.如果在哈希Map中发现的位置在当前最大字串之前（说明当前最大字串中没有这个字符）：
	            //         则更新哈希Map中的该字符位置，并且当前最长字串长度+1
	            //     2.如果在哈希Map中发现的位置在当前最大字串之后（说明当前最大字串中有这个字符）：
	            //         首先对比也下之前的最大长度和现在长度，大的赋给截至到当前的最大长度ans；
	            //         然后当前最大长度更新为当前位置减去哈希Map中该字符的位置；
	            //         再跟新当前最大字符串的起始位置tempPosi；
	            //         最后更新哈希Map中该字符的当前位置。
	            // 2.如果哈希Map中没发现当前位置的字符：字符-位置 加入到哈希Map，并且当前最长字串长度+1
	            if(hashMap.containsKey(s.charAt(temp))){
	                if(tempPosi > hashMap.get(s.charAt(temp))){
	                    hashMap.put(s.charAt(temp),temp);
	                    tempAns++;
	                }else{
	                    ans = Math.max(ans,tempAns);
	                    tempAns = (temp - hashMap.get(s.charAt(temp)));
	                    tempPosi = hashMap.get(s.charAt(temp)) + 1;
	                    hashMap.put(s.charAt(temp),temp);
	                }
	                    
	            }else{
	                hashMap.put(s.charAt(temp),temp);
	                tempAns++;
	            }
	            temp++;
	        }
	        return Math.max(ans,tempAns);
	    }
	}

# 5.最长回文子串（有dp解法）
## 题目
https://leetcode-cn.com/problems/longest-palindromic-substring/
## 题解

https://leetcode-cn.com/problems/longest-palindromic-substring/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-bao-gu/

>**方法一（自己写的）：**
>三层for：
>- 1.第一层为字串长度；
>- 2.第二层为字串初始位置、终止位置；
>- 3.第三层为判断是否回文。


下面为大神题解：
>**方法二：**
>**最长公共子串(有dp解法，原问题他是求的两个字符串中有多少是相同的比如 S1="abc435cba"，S2="abc534cba"；最长公共子串是 "abc" 和 "cba"，但很明显这两个字符串都不是回文串。所以需要修改)：**
>- 1.根据回文串的定义，正着和反着读一样，那我们是不是把原来的字符串倒置了，然后找最长的公共子串就可以了。例如 S = "caba" ，S = "abac"，最长公共子串是 "aba"，所以原字符串的最长回文串就是 "aba"。
>- 2.关于求最长公共子串（不是公共子序列），有很多方法，这里用**动态规划**的方法。
>- 3.整体思想就是，申请一个二维的数组初始化为 0，然后判断对应的字符是否相等，相等的话arr [ i ][ j ] = arr [ i - 1 ][ j - 1] + 1 。当 i = 0 或者 j = 0 的时候单独分析，字符相等的话 arr [ i ][ j ] 就赋为 1 。arr [ i ][ j ] 保存的就是公共子串的长度。


## 代码
**方法一:**
	
	class Solution {
	    public String longestPalindrome(String s) {
	        boolean flag = true;
	        int length = s.length();
	        int l,r;
	        for(int i = length; i > 0; i--){
	            for(int j = 0; j < length - i + 1; j++){
	                flag = true;
	                for(l = j, r = l + i - 1; l < r; l++,r--){
	                    if(s.charAt(l) != s.charAt(r)){
	                        flag = false;
	                        break;
	                    }
	                }
	                if(flag)
	                    return s.substring(j,j + i);
	            }
	        }
	        return "";
	    }
	}
**方法二：**
**最长公共子串的代码：**

	public String longestPalindrome(String s) {
	    if (s.equals(""))
	        return "";
	    String origin = s;
	    String reverse = new StringBuffer(s).reverse().toString(); //字符串倒置
	    int length = s.length();
	    int[][] arr = new int[length][length];
	    int maxLen = 0;
	    int maxEnd = 0;
	    for (int i = 0; i < length; i++)
	        for (int j = 0; j < length; j++) {
	            if (origin.charAt(i) == reverse.charAt(j)) {
	                if (i == 0 || j == 0) {
	                    arr[i][j] = 1;
	                } else {
	                    arr[i][j] = arr[i - 1][j - 1] + 1;
	                }
	            }
	            if (arr[i][j] > maxLen) { 
	                maxLen = arr[i][j];
	                maxEnd = i; //以 i 位置结尾的字符
	            }
	
	        }
		}
		return s.substring(maxEnd - maxLen + 1, maxEnd + 1);
	}

**对于该问题修改后的：**

	public String longestPalindrome(String s) {
	    if (s.equals(""))
	        return "";
	    String origin = s;
	    String reverse = new StringBuffer(s).reverse().toString();
	    int length = s.length();
	    int[][] arr = new int[length][length];
	    int maxLen = 0;
	    int maxEnd = 0;
	    for (int i = 0; i < length; i++)
	        for (int j = 0; j < length; j++) {
	            if (origin.charAt(i) == reverse.charAt(j)) {
	                if (i == 0 || j == 0) {
	                    arr[i][j] = 1;
	                } else {
	                    arr[i][j] = arr[i - 1][j - 1] + 1;
	                }
	            }
	            /**********修改的地方*******************/
	            if (arr[i][j] > maxLen) {
	                int beforeRev = length - 1 - j;
	                if (beforeRev + arr[i][j] - 1 == i) { //判断下标是否对应
	                    maxLen = arr[i][j];
	                    maxEnd = i;
	                }
	                /*************************************/
	            }
	        }
	    return s.substring(maxEnd - maxLen + 1, maxEnd + 1);
	}

