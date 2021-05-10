---
title: 'Leetcode(mid)'
date: 2021/5/7 13:29:32  
tags:
	- Leetcode
	- 中等题目
---
>**题目列表：**
>2.两数相加
>3.无重复字符的最长子串
>5.最长回文子串（有dp解法**）
>6.Z字形变换（模拟寻找位置）
>8.字符串转换整数 (atoi)
>11.盛最多水的容器
>12.整数转罗马数字
>1482.制作 m 束花所需的最少天数（二分）
>剑指 Offer 04 二维数组中查找
>15.三数之和
>16.最接近的三数之和
>17.电话号码的字母组合
>18.四数之和


<!-- more -->
# 2.两数相加

## 题目
https://leetcode-cn.com/problems/add-two-numbers
## 题解

创建一个链表，直接while循环中判断创建并把结果加到链表中；
代码中有思路；
代码用时2ms 超过100%用户；
代码内存消耗超过70.9%用户.

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

# 5.最长回文子串（有dp解法**）

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

# 6.Z字形变换

## 题目
https://leetcode-cn.com/problems/zigzag-conversion/
## 题解
https://leetcode-cn.com/problems/zigzag-conversion/solution/stringbuffercun-chu-xing-qiu-jie-by-rain-5oep/
>**思路一：**
>按照行列两个for硬模拟，有点麻烦
>**思路二：**
>1.对于numRows行为1或者行数大于字串长度的特殊情况if(numRows == 1 || numRows >= length) return s;
>2.其他情况
>- 创建长度为 行数+1 的StringBuffer数组，按照坐标存储字符串应该所在的行，如示例1中第一个最后results[1]="PAHN",其他坐标的类似


## 代码
**方法一：**

	class Solution {
	    public String convert(String s, int numRows) {
	        int l = numRows + numRows - 2;
	        int length = s.length();
	        if(numRows == 1 || numRows >= length) return s;
	        StringBuffer result = new StringBuffer();
	        if(numRows == 2){
	            for(int i = 0; i < length; i++){
	                if(i % 2 == 0)
	                    result.append(s.charAt(i));
	            }
	            for(int i = 0; i < length; i++){
	                if(i % 2 == 1)
	                    result.append(s.charAt(i));
	            }
	            return result.toString();
	        }
	        //可以分成zNum组
	        int zNum = length / l + 1;
	        //最多有这些列
	        int numL = zNum  * (numRows - 1) - 1;
	        //numRows 行
	        for(int i = 0; i < numRows; i++){
	            //numL 列
	            for(int j = 0; j < numL; j++){
	                int temp = j / (numRows - 1);
	                if(l * temp + i < length && j % (numRows - 1) == 0){
	                    result.append(s.charAt(l * temp + i));
	                }else if(l * temp + numRows - 1 + numRows - i - 1 < length && j % (numRows - 1) == numRows - i - 1){
	                    result.append(s.charAt(l * temp + numRows - 1 + numRows - i - 1));
	                }else{
	                    ;
	                }
	                
	                    
	            }
	        }
	        return result.toString();
	    }
	}

**方法二：**

	class Solution {
	    public String convert(String s, int numRows) {
	        int length = s.length();
	
	
	        //1.对于行为1或者行数大于字串长度的特殊情况
	        if(numRows == 1 || numRows >= length) return s;
	
	        //2.其他情况
	        //创建长度为 行数+1 的StringBuffer数组，按照坐标存储字符串应该所在的行，如最后results[1]="PAHN";其他坐标的类似
	        StringBuffer[] results = new StringBuffer[numRows + 1];
	        for(int i = 0; i < numRows + 1; i++){results[i] = new StringBuffer();}
	        //当前该记录在哪一行
	        int temp = 1;
	        //真：Z往下走的 假：Z往上走的
	        boolean flag = true;
	        //把行相同的分别存起来
	        for(int i = 0; i < length; i++){
	            results[temp].append(s.charAt(i));
	            if(flag){temp++;}
	            else{temp--;}
	            if(temp == 1 || temp == numRows){flag = !flag;}
	        }
	        //一行一行的整合到结果results[0]中
	        for(int i = 0; i < numRows + 1; i++)
	            results[0].append(results[i].toString());
	        //返回结果results[0]
	        return results[0].toString();
	    }
	}


# 8.字符串转换整数 (atoi)

## 题目
https://leetcode-cn.com/problems/string-to-integer-atoi/
## 题解

https://leetcode-cn.com/problems/string-to-integer-atoi/solution/liang-chong-jian-dan-mo-ni-by-rain-ru-ezao/

第一种用了StringBuffer把数字存在里面，最后强转为long判断
第二种直接定义了一个long判断，第二种执行时间明显少了很多。

## 代码

	class Solution {
	    public int myAtoi(String s) {
	        //字符数组长度
	        int length = s.length();
	        //存答案
	        long ans = 0;
	        //存取正负：正为真，负为假
	        boolean flag = true;
	        //判断是否已经读取过数字或者正负号，没有为true，读取过为false
	        boolean yes = true;
	        for(int i = 0; i < length; i++){
	            //1.没读过数字和正负号，开头是空格就继续
	            if(yes && s.charAt(i) == ' ') continue;
	            //2.没读过数字和正负号，遇到正号，yes变为false
	            else if(yes && ans == 0 && s.charAt(i) == '+') yes = false;
	            //3.没读过数字和正负号，遇到负号，yes和flag均变为false
	            else if(yes && ans == 0 && s.charAt(i) == '-') yes = flag = false;
	            //4.遇到非数字了，也不是上面三种情况，返回ans或者-ans（flag决定的）
	            else if(s.charAt(i) < '0' || s.charAt(i) > '9') return flag ? (int)ans : (int)-ans;
	            //5.其他（读取到的数字）,yes变为false且ans更新
	            else {yes = false;ans = ans * 10 + s.charAt(i) - '0';}
	
	            //现在的ans已经( ans > int的上界 或 -ans <= int的下界)越界就直接返回
	            if(ans > Integer.MAX_VALUE)
	                return flag ? (int)Math.min((long)Integer.MAX_VALUE, ans) : (int)Math.max((long)Integer.MIN_VALUE, -ans);
	        }
	        //字符判断一遍也没越界，返回ans或者-ans（flag决定的）
	        return flag ? (int)ans : (int)-ans;
	    }
	}

# 11.盛最多水的容器

## 题目
https://leetcode-cn.com/problems/container-with-most-water
## 题解
https://leetcode-cn.com/problems/container-with-most-water/solution/si-lu-qing-xi-shuang-zhi-zhen-by-rain-ru-kegg/
对于**当前数组位置temp_Pos**的元素来说，他能**容纳最多的水在以下两种情况可能出现：**
- 1.**离他最远的那个位置即max_Pos**, 此时容纳量(max_Pos - temp_Pos) * Math.min(heigth[temp_Pos], heigth[max_Pos])；
- 2.**离他最远并且大于他当前高度的位置即h_Pos**,此时容纳量 (h_Pos - temp_Pos) * Math.min(heigth[temp_Pos], heigth[h_Pos]),因为这种情况当前高度本来本来就小，也可以写成(h_Pos - temp_Pos) * heigth[temp_Pos]。


**示例1：**
- **1.从0位置开始**，对于0位置的桶边来说，他最大容纳的水出现在上述两种情况中，但是两种情况在同一位置，即离他最远的位置和离他最远并且大于他当前高度的位置是同一个，即最后一个8位置，那么对于0位置来说他的最大值就求出来了，他可以不用管了（因为容纳水是两个桶来决定的，当前位置求出了最大，那么别的桶不可能用当前桶求出比此时结果还要大的值），所以我们可以继续分析1位置和8位置中的桶；
- **2.那么对于1位置到8位置呢**，对于0位置来说是不是8位置 就是他最大容纳位置，因为8位置即离他最远的位置和离他最远并且大于他当前高度的位置，那么对于8位置来说，是不是1位置也是即离他最远的位置和离他最远并且大于他当前高度的位置，对的，所以对于8位置来说，他的最大容量也讨论出来了，所以同样可以不去思考了（跟上面1.中抛弃0位置一样），所以我们可以继续分析1位置和7位置中的桶；
- …………


**规律：**
你肯定发现规律了吧！对的！我们只需要考虑两边短的那个桶就可以了，所以只需要两个指针就能解决问题

## 代码

	class Solution {
	    public int maxArea(int[] height) {
	        //存放最大容量结果
	        int result = 0;
	        //左指针
	        int l = 0;
	        //右指针
	        int r = height.length - 1;
	
	        //开始找最大容量 当左位置小于右位置继续寻找
	        while(l < r){
	            //当前短桶位置的最大容量如果大于之前的结果，则赋给之前的结果
	            result = Math.max(result, (r - l) * Math.min(height[l],height[r]));
	            
	            //如果左位置的桶短那么他的最大容量已经找到，抛弃他，左位置+1
	            if(height[l] < height[r]) l++;
	            //如果右位置的桶短那么他的最大容量已经找到，抛弃他，右位置-1
	            else r--;
	        }
	        //返回最大结果
	        return result;
	    }
	}


# 12.整数转罗马数字

## 题目
https://leetcode-cn.com/problems/integer-to-roman
## 题解

https://leetcode-cn.com/problems/integer-to-roman/solution/mo-ni-si-lu-qing-xi-by-rain-ru-n0a0/
## 代码

	class Solution {
	    public String intToRoman(int num) {
	        StringBuffer result = new StringBuffer();
	        int I, V, X, L, C, D, M;
	        //分别求M、D、C、L、X、V、I
	        M = num / 1000;
	        num %= 1000;
	
	        D = num / 500;
	        num %= 500;
	
	        C = num / 100;
	        num %= 100;
	
	        L = num / 50;
	        num %= 50;
	
	        X = num / 10;
	        num %= 10;
	
	        V = num / 5;
	        num %= 5;
	
	        I = num;
	
	        //输出千
	        while(M > 0) {result.append("M"); M--;}
	        //900
	        if(D == 1 && C == 4) {result.append("CM"); D = 0; C = 0;}
	        //不是900但>500
	        while(D > 0) {result.append("D"); D--;}
	        //400
	        if(C == 4) {result.append("CD"); C = 0;}
	        //不是400但大于等于100
	        while(C > 0) {result.append("C"); C--;}
	        //90
	        if(L == 1 && X == 4) {result.append("XC"); L = 0; X = 0;}
	        //不是90但大于等于50
	        while(L > 0) {result.append("L") ;L--;}
	        //40
	        if(X == 4) {result.append("XL"); X = 0;}
	        //不是40但大于等于10
	        while(X > 0) {result.append("X"); X--;}
	        //9
	        if(V == 1 && I == 4) {result.append("IX"); V = 0; I = 0;}
	        //不是9但大于等于5
	        while(V > 0) {result.append("V") ;V--;}
	        //4
	        if(I == 4) {result.append("IV"); I = 0;}
	        //不是4但大于等于1
	        while(I > 0) {result.append("I"); I--;}
	
	        return String.valueOf(result);
	    }
	}

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

# 15.三数之和

## 题目
https://leetcode-cn.com/problems/3sum
## 题解
https://leetcode-cn.com/problems/3sum/solution/shuang-zhi-zhen-nei-cun-100-by-rain-ru-ow22/
![image.png](https://pic.leetcode-cn.com/1620630918-PVcEtx-image.png)
一开始暴力写的，两个for虽然也过了，但是时间空间可怜，就用双指针写了一个：

**主要思路**:先排序再从0位置开始循环当前位置、当前位置+1和最后位置这三个数，看和是否为0
**具体思路：**
1.排序
2.for循环从数组0坐标开始遍历
（1）如果当前位置不是数组起始位置0并且当前的i位置的数组值和前一i位置数组值相同，则跳过本次for循环（因为和前一回合的结果重复）
（2）如果当前的i位置数组值都大于0了，直接返回结果（因为数组排序了,当前大于0，则加后面的两个数字总和肯定大于零）
（3）其他情况下，初始化l和r (l = i + 1; r = nums.length - 1;) ,当l < r执行（sum = nums[i] + nums[l] + nums[r]）：
- ①sum = 0：则把此时的三个数加入到结果List中，然后l++,r--（并且需要while()一下当此时新的l位置数组值和r位置数组值不能与之前的l、r位置相同，说明结果是重复的，就继续l++，r--，直到与之前不同或者l >=r 了，结束）
- ②sum > 0: 说明此时的r位置数组值太大，所以r--
- ③sum < 0: 说明此时的l位置数组值太小，所以l++
## 代码
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        int l, r, sum, n = nums.length;
        Arrays.sort(nums);
        
        for(int i = 0; i < n; i++){
            if(i != 0 && nums[i] == nums[i - 1]) continue;
            else if(nums[i] > 0) return result;
            else{
                l = i + 1;
                r = n - 1;
                while(l < r){
                    sum = nums[i] + nums[l] + nums[r];
                    if(sum == 0){
                        result.add(Arrays.asList(nums[i], nums[l++], nums[r--])); 
                        while(l < r && nums[l] == nums[l - 1]) l++;
                        while(l < r && nums[r] == nums[r + 1]) r--;
                    }
                    else if(sum < 0) l++;
                    else r--;              
                }
            }  
        }
        return result;
    }
}
```
# 16.最接近的三数之和

## 题目
https://leetcode-cn.com/problems/3sum-closest
## 题解
![image.png](https://pic.leetcode-cn.com/1620633756-GHLZOW-image.png)
上一道题目双指针修改了一下，上一道题目我的解析：
https://leetcode-cn.com/problems/3sum/solution/shuang-zhi-zhen-nei-cun-100-by-rain-ru-ow22/

**本题主要思路**:先排序再从0位置开始循环当前位置、当前位置+1和最后位置这三个数，看和是否为0
**具体思路：**
1.排序
2.for循环从数组0坐标开始遍历
（1）如果当前位置不是数组起始位置0并且当前的i位置的数组值和前一i位置数组值相同，则跳过本次for循环（因为和前一回合的结果重复）
（2）其他情况下，初始化l和r (l = i + 1; r = nums.length - 1;) ,当l < r执行（sum = nums[i] + nums[l] + nums[r]）：
- ①sum = target：直接返回target
- ②sum > target 或 sum < target: 
    首先更新此时的result和当前的差值；
    如果sum > target说明此时的r位置数组值太大，所以需要再r--；
    如果sum < target说明此时的l位置数组值太小，所以需要再l++。

## 代码
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int l, r, sum, temCha;

        int n = nums.length;
        int result = nums[0] + nums[1] + nums[2];
        int cha = Math.abs(target - result);
        
        for(int i = 0; i < n - 2; i++){
            if(i != 0 && nums[i] == nums[i - 1]) continue;
            else{
                l = i + 1;
                r = n - 1;
                while(l < r){
                    sum = nums[i] + nums[l] + nums[r];
                    if(sum == target){
                        return target;
                    }else{
                        temCha = Math.abs(sum - target);
                        if(cha > temCha){result = sum; cha = temCha;}
                        if(sum < target) l++;
                        else r--;
                    }           
                }
            }  
        }
        return result;
    }
}
```
# 17.电话号码的字母组合

## 题目
https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number
## 题解
![image.png](https://pic.leetcode-cn.com/1620656272-sdACSF-image.png)
如果知道字符的确定长度，肯定就直接可以暴力for循环了，但是不知道，所以可以用DFS来写
需要注意的是如果字符长度为0，需要新建一个List<String>返回，直接返回我创建的result是[""],我也不知道为啥……
## 代码
```java
class Solution {
    List<String> result = new ArrayList<String>();
    String[] ant = new String[]{"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0) return new ArrayList<String>();
        StringBuffer ans = new StringBuffer();
        dfs(digits, 0, ans);
        return result;
    }
    public void dfs(String digits, int temp, StringBuffer ans){
        if(temp == digits.length()) result.add(ans.toString());
        else{
            int num = digits.charAt(temp) - '0' - 2;
            for(int i = 0; i < ant[num].length(); i++){
                ans.append(ant[num].charAt(i));
                dfs(digits, temp + 1, ans);
                ans.deleteCharAt(temp);
            }
        }
    }
}
```
# 18.四数之和

## 题目
https://leetcode-cn.com/problems/4sum
## 题解
![image.png](https://pic.leetcode-cn.com/1620660356-iLECIv-image.png)

双指针，前面三个数之和又加了一层for，前面两题我的题解：
https://leetcode-cn.com/problems/3sum/solution/shuang-zhi-zhen-nei-cun-100-by-rain-ru-ow22/
https://leetcode-cn.com/problems/3sum-closest/solution/shuang-zhi-zhen-80shi-jian-80kong-jian-b-07wd/
## 代码
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        int temp, i, l, r, sum, n = nums.length;
        if(n < 4) return result;
        else Arrays.sort(nums);
    
        for(temp = 0; temp < n - 3; temp++){
            if(temp != 0 && nums[temp] == nums[temp - 1]) continue;
            else if(nums[temp] > target && nums[temp] > 0) return result;
            else{
                for(i = temp + 1; i < n - 2; i++){
                    if(i != temp + 1 && nums[i] == nums[i - 1]) continue;
                    else if(nums[i] > target - nums[temp] && nums[i] > 0) break;
                    else{
                        l = i + 1;
                        r = n - 1;
                        while(l < r){
                            sum = nums[temp] + nums[i] + nums[l] + nums[r];
                            if(sum == target){
                                result.add(Arrays.asList(nums[temp], nums[i], nums[l++], nums[r--])); 
                                while(l < r && nums[l] == nums[l - 1]) l++;
                                while(l < r && nums[r] == nums[r + 1]) r--;
                            }
                            else if(sum < target) l++;
                            else r--;              
                        }
                    }  
                }
            }   
        }
        return result;
    }
}
```