---
title: 'Leetcode(mid1-100)'
date: 2021/5/21 22:07:51 
tags:
	- Leetcode
---
>**题目列表：**
>2.两数相加
>3.无重复字符的最长子串
>5.最长回文子串（有dp解法**）
>6.Z字形变换（模拟寻找位置）
>8.字符串转换整数 (atoi)
>11.盛最多水的容器
>12.整数转罗马数字
>15.三数之和
>16.最接近的三数之和
>17.电话号码的字母组合
>18.四数之和
>19.删除链表的倒数第 N 个结点
>22.括号生成
>24.两两交换链表中的节点（链表）
>29.两数相除（位运算，模拟）
>33.搜索旋转排序数组（二分）
>34.在排序数组中查找元素的第一个和最后一个位置(二分)
>36.有效的数独
>38.外观数列
>39.组合总和（dfs）
>40.组合总和 II(dfs)
>50.Pow(x, n)(位运算)
>54.螺旋矩阵
>79.单词搜索(dfs)
>81.搜索旋转排序数组 II（二分）

25道中等题目


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
```java
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
```

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
```java
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
```
**方法二：**
**最长公共子串的代码：**
```java
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
```
**对于该问题修改后的：**
```java
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
```
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
```java
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
```
**方法二：**
```java
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
```

# 8.字符串转换整数 (atoi)

## 题目
https://leetcode-cn.com/problems/string-to-integer-atoi/
## 题解

https://leetcode-cn.com/problems/string-to-integer-atoi/solution/liang-chong-jian-dan-mo-ni-by-rain-ru-ezao/

第一种用了StringBuffer把数字存在里面，最后强转为long判断
第二种直接定义了一个long判断，第二种执行时间明显少了很多。

## 代码
```java
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
```
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
```java
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
```

# 12.整数转罗马数字

## 题目
https://leetcode-cn.com/problems/integer-to-roman
## 题解

https://leetcode-cn.com/problems/integer-to-roman/solution/mo-ni-si-lu-qing-xi-by-rain-ru-n0a0/
## 代码
```java
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
```
# 15.三数之和

## 题目
https://leetcode-cn.com/problems/3sum
## 题解
https://leetcode-cn.com/problems/3sum/solution/shuang-zhi-zhen-nei-cun-100-by-rain-ru-ow22/

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

# 19.删除链表的倒数第 N 个结点
## 题目
https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/
## 题解
先把头指针存下来，然后遍历一遍看链表多长：
1.如果n==长度，则返回头指针的next结果
2.如果不相等，则再从头开始遍历到删除节点前的那个节点，让他head.next = head.next.next
最后返回存储的头指针
##代码
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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int temp = 0;
        ListNode result = head;
        while(head != null){temp++; head = head.next;}
        if(n == temp) return result.next;
        n = temp - n;
        head = result; 
        temp = 1;
        while(head != null){
            if(temp == n){
                head.next = head.next.next;
                return result;
            }else{
                head = head.next;
                temp++;
            }
        }
        return result;
    }
}
```
# 22.括号生成
## 题目
https://leetcode-cn.com/problems/generate-parentheses/
## 题解
dfs里面循环(StringBuffer字符串,"("出现次数，")"出现次数，题目的n)：
1.剪枝if(r > l || l > n || r > n || l + r > 2 * n) return; （这些都不符合题意）
2.l == n && r == n result.add(s.toString()); 此时加入结果List中
3.l == n 此时"("数量够了，只加")"
4.l == r 此时"("、")"数量相同，只加"("
5.其他,此时"("数量 > ")"数量，"("、")"都可以加
##代码
```java
class Solution {
    List<String> result = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        StringBuffer s = new StringBuffer();
        dfs(s, 0, 0, n);
        return result;
    }
    public void dfs(StringBuffer s, int l, int r, int n){
        if(r > l || l > n || r > n || l + r > 2 * n) return;
        else if(l == n && r == n) result.add(s.toString());
        else if(l == n){
            s.append(")");
            dfs(s, l, r + 1, n);
            s.deleteCharAt(l + r);
        }else if(l == r){
            s.append("(");
            dfs(s, l + 1, r, n);
            s.deleteCharAt(l + r);
        }else{
            s.append(")");
            dfs(s, l, r + 1, n);
            s.deleteCharAt(l + r);
            s.append("(");
            dfs(s, l + 1, r, n);
            s.deleteCharAt(l + r);
        }
    }
}
```


# 24.两两交换链表中的节点
## 题目
https://leetcode-cn.com/problems/swap-nodes-in-pairs
## 题解
**非递归：**
分三种情况在while内循环：
**1.head.next.next == null ：** 此时只要完成head节点和head.next节点的互换就可以，注意换完后最后的listNode.next == null ,否则就成了一个环了（**举例** 1,2:我们想换成2,1）先初始1->2,转换后没有null 2->1且1->2
**2.head.next.next.next != null ：** 此时要完成4个点之间的关联 （**举例** 1,2,3,4,……：我们想换成2,1,4,3）所以先把2，3分别存一下，然后1->4,2->1,把head节点换为3（此时节点内指向为2->1->4->……,3->4->……）
**3.其他情况，也就是head.next.next != null且head.next.next.next == null** （**举例** 1,2,3：我们想换成2,1,3）所以我们把2->1,1->3就好了，注意提前存一下2节点

**递归(配合例子理解)：**
递归是重复做一样的事情，所以只需要去考虑一步是怎么进行的：
假设需要交换的节点分别为head和next，hand因为通过交换要去next后面，所以hand肯定需要接受递归返回的后面交换好的链表为他的next（也就是后面递归完成交换的next节点），接受后就可以把hand赋给next.next完成当前轮次的交换了，所以终止只需要当没有节点交换了，也就是head == null或者heda.next == null 时结束（返回hand）。
**举例：** 1,2,3,4,5:
**第一轮head = 1,next = 2：**
head.next = swapPairs(next.next); 也就是 head(1).next = swapPairs(2.next);
**第二轮head = 3,next = 4：**
head.next = swapPairs(next.next); 也就是 head(3).next = swapPairs(4.next);
**第三轮head = 5,next = null：**  返回head = 5 给第二轮
**第二轮head = 3,next = 4接受第三轮返回并返回给第一轮：** head(3).next = swapPairs(4.next) = 5, next(4).next = 3;此时next(4)及其之后排序为 ->4->3->5 ,返回next = 4 给第一轮
**第一轮head = 1,next = 2接受第二轮返回并返回最终结果：** head(1).next = swapPairs(2.next) = 4, next(2).next = 1;此时next(2)及其之后排序为 2->1->4->3->5 ,返回next = 2
## 代码
**非递归：**
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;
        else{
            ListNode result = head.next;
            ListNode tempSecond = new ListNode();
            ListNode tempThird = new ListNode();
            while(head != null && head.next != null){
                if(head.next.next == null){
                    head.next.next = head;

                    head.next = null;
                }
                else if(head.next.next.next != null){
                    tempSecond = head.next;
                    tempThird = tempSecond.next;
                    head.next = tempThird.next;
                    tempSecond.next = head;

                    head = tempThird;
                    }
                else{
                    tempSecond = head.next;
                    head.next = head.next.next;
                    tempSecond.next = head;

                    head = head.next.next;
                }
            }
            return result;
        }

    }
}

```
**递归：**
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode next = head.next;
        head.next = swapPairs(next.next);
        next.next = head;
        return next;
    }
}
```

# 29.两数相除
## 题目
https://leetcode-cn.com/problems/divide-two-integers
## 题解
**位运算须知：**  
- result << 表示电脑存储的result二进制数左移，result << 1 表示就是 result * 2
- result >> 表示电脑存储的result二进制数右移，result >> 1 表示就是 result / 2

**思路：** 用long存一下，然后判断一下结果正负号，并把除数和被除数都转成正的，然后两个while循环 位运算（比如31 / 1）
**例子（31 / 1）：** 
**第一轮：**
- 31 - 1 >= 0 所以进入**第一个while**，然后 temp = 1 << 1 = 2 ,result = 1 ;
- 31 - 2 >= 0 所以进入**第二个while**，然后 result = result << 1 = 2; temp = temp << 1 = 4;
- 31 - 4 >= 0 所以还在**第二个while**，然后 result = result << 1 = 4; temp = temp << 1 = 8;
- 31 - 8 >= 0 所以还在**第二个while**，然后 result = result << 1 = 8; temp = temp << 1 = 16;
- 31 - 16 >= 0 所以还在**第二个while**，然后 result = result << 1 = 16; temp = temp << 1 = 32;
- 31 - 32 < 0 所以**跳出第二个while**，然后更新被除数divid -= temp >> 1 = 31 - 16 = 15; 更新结果ans += result = 0 + 16 = 16;

**新一轮：**
- 15 - 1 >= 0 所以进入**第一个while**，然后 temp = 1 << 1 = 2 ,result = 1 ;
- 15 - 2 >= 0 所以进入**第二个while**，然后 result = result << 1 = 2; temp = temp << 1 = 4;
- ……
- 15 - 8 >= 0 所以还在**第二个while**，然后 result = result << 1 = 8; temp = temp << 1 = 16;
- 15 - 16 < 0 所以**跳出第二个while**，然后更新被除数divid -= temp >> 1 = 15 - 8 = 7; 更新结果ans += result = 16 + 8 = 24;

**新一轮：**
- ……


最后**第一个while都没有进去**就可以得到最终结果了：注意结果的符号
## 代码
```java
class Solution {
    public int divide(int dividend, int divisor) {
        if(dividend == Integer.MIN_VALUE && divisor == -1) return Integer.MAX_VALUE;
        long divid = (int)dividend;
        long divis = (int)divisor;

        boolean flag = true;
        long temp, result = 1, ans = 0;

        if(divid < 0 && divis > 0){divid = - divid; flag = false;}
        else if(divid > 0 && divis < 0){divis = - divis; flag = false;}
        else if(divid < 0 && divis < 0){divid = - divid; divis = - divis;}
        else ;

        while(divid - divis >= 0){
            temp = divis << 1;
            result = 1;
            while(divid - temp >= 0){
                result = result << 1;
                temp = temp << 1;
            }
            divid -= temp >> 1;
            ans += result;
        }
        
        if(flag) return (int)ans;
        else return (int)-ans;
    }
}
```

# 33.搜索旋转排序数组
## 题目
https://leetcode-cn.com/problems/search-in-rotated-sorted-array
## 题解
**共三种情况（请综合我下面给的例子来看后两种情况）**：
**1.左 == target：** 直接返回 左
**2.左 < 中：** 此时需要分情况讨论：
- （1）target比左小**或者**target比中大时（比小的都小**或者**比大的都大）：此时target只可能在[mid, r]中，所以l = mid;
- （2）其他，即target比左大**并且**target比中小时（大小在左和中之间）：此时target只可能在[左 + 1, mid]中，所以 l = l + 1; r = mid;

**3.左 > 中：** 此时需要分情况讨论：
- （1）target比左小**并且**target比中大时（大小在左和中之间）：此时target只可能在[mid, r]中，所以l = mid;
- （2）其他，即target比左大**或者**比中小时（比大的都大**或者**比小的都小）：此时target只可能在[左 + 1, mid]中，所以 l = l + 1; r = mid;

**后两种情况即2.3情况例子：**

**2.** [1，2，3，4，5] 或 [1，2，3，4，0]，1 < 3(nums[0] < nums[2]，**左 < 中**)
- （1）[1，2，3，4，**0**]中目标target 0 或者 [1，2，3，4，**5**]中目标target 5 都在[mid, r]中
- （2）[1，**2**，3，4，0]中目标target 2 只在[左 + 1, mid]中

**3.** [5，6，2，3，4] 或 [5，1，2，3，4]，5 > 2(nums[0] > nums[2]，**左 > 中**)
- （1）[5，6，2，**3**，4]中目标target 3 只在[mid, r]中
- （2）[5，**6**，2，3，4]中目标target 6 或者 [5，**1**，2，3，4]中目标target 1 都只在[左 + 1, mid]中


## 代码

```java
class Solution {
    public int search(int[] nums, int target) {
        int n = nums.length;
        int result = -1;
        int l = 0, r = n - 1;
        while(l <= r){
            int mid = (l + r + 1) >> 1;
            if(nums[l] == target) return l;
            else if(nums[l] < nums[mid]){
                if(nums[l] > target || nums[mid] < target) l = mid;
                else{
                    l = l + 1;
                    r = mid;
                }
            }else{
                if(nums[l] > target && nums[mid] < target) l = mid;
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

# 34.在排序数组中查找元素的第一个和最后一个位置
## 题目
https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array
## 题解
**共四种情况**：
**1.左 == target：** 此时需要分情况讨论：
- （1）右 == target，则return new int[]{l, r};
- （2）nums[mid] == target，此时target范围可能在[左，右 - 1]中，所以右 = 右 + 1
- （3）其余情况，也就是nums[mid] > target,此时target范围可能在[左，mid - 1]中，所以右 = mid - 1;

**2.中 == target：** 此时target范围可能在[左 + 1，mid]中，所以左 = 左 + 1
**3.中 < target：** 此时target范围可能在[mid + 1，右]中，所以左 = mid + 1
**4.其他，中 > target：** 此时target范围可能在[左，mid - 1]中，所以右 = mid - 1;

## 代码

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        while(l <= r){
            int mid = (l + r + 1) / 2;
            if(nums[l] == target){
                if(nums[r] == target) return new int[]{l, r};
                else if(nums[mid] == target) r--;
                else r = mid - 1;
            }
            else if(nums[mid] == target) l++;
            else if(nums[mid] < target) l = mid + 1;
            else r = mid - 1;
        }
        return new int[]{-1, -1};
    }
}
```

# 36.有效的数独
## 题目
https://leetcode-cn.com/problems/valid-sudoku/
## 题解
1.判断行和列，两个for循环，用两个boolean直接判断了
2.判断九宫格，需要四个for来控制行和列

## 代码

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        //判断行和列
        for(int i = 0; i < 9; i++){
            boolean[] usedRow = new boolean[9];
            boolean[] usedList = new boolean[9];
            for(int j = 0; j < 9; j++){
                if(board[i][j] == '.') ;
                else if(board[i][j] > '0' && board[i][j] <= '9' && !usedRow[board[i][j] - '1']) usedRow[board[i][j] - '1'] = true;
                else return false;
                if(board[j][i] == '.') ;
                else if(board[j][i] > '0' && board[j][i] <= '9' && !usedList[board[j][i] - '1']) usedList[board[j][i] - '1'] = true;
                else return false;
            }
        }

        //判断九宫格
        for(int m = 0; m < 3; m++){
            for(int n = 0; n < 3; n++){
                boolean[] used = new boolean[9];
                for(int i = m * 3; i < m * 3 + 3; i++){
                    for(int j = n * 3; j < n * 3 + 3; j++){
                        if(board[i][j] == '.') ;
                        else if(board[i][j] > '0' && board[i][j] <= '9' && !used[board[i][j] - '1']) used[board[i][j] - '1'] = true;
                        else return false;
                    }
                }
            }
        }
        return true;
    }
}
```


# 38.外观数列
## 题目
https://leetcode-cn.com/problems/count-and-say/
## 题解
把每个数的都存下里，每次都找下个数

## 代码

```java
class Solution {
    public String countAndSay(int n) {
        StringBuffer[] result = new StringBuffer[n];
        for(int i = 0; i < n; i++) result[i] = new StringBuffer();
        result[0].append(1);
        int tempNum = 1;
        for(int i = 1; i < n; i++){
            tempNum = 1;
            for(int j = 0; j < result[i - 1].length(); j++){
                if(j != result[i - 1].length() - 1){
                    if(result[i - 1].charAt(j) != result[i - 1].charAt(j + 1)){
                        result[i].append(tempNum + "" + result[i - 1].charAt(j));
                        tempNum = 1;
                    }
                    else if(result[i - 1].charAt(j) == result[i - 1].charAt(j + 1))
                        tempNum++;
                }
                else{
                        result[i].append(tempNum + "" + result[i - 1].charAt(j));
                }
            }
        }
        return result[n - 1].toString();
    }
}
```


# 39.组合总和
## 题目
https://leetcode-cn.com/problems/combination-sum
## 题解
## 代码
```java
class Solution {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        dfs(candidates, target, 0, 0, new ArrayList<Integer>());
        return result;
    }
    public void dfs(int[] candidates, int target, int temp, int sum, List<Integer> list){
        if(sum == target){
            List<Integer> ans = new ArrayList<Integer>();
            for(int i = 0; i < list.size(); i++) ans.add(list.get(i));
            result.add(ans);
        }
        else if(temp == candidates.length) return;
        else{
            for(int i = temp; i < candidates.length; i++){
                for(int j = 1; j <= (target - sum) / candidates[i]; j++){
                    for(int k = 0; k < j; k++) list.add(candidates[i]);
                    dfs(candidates, target, i + 1, sum + j * candidates[i], list);
                    for(int k = 0; k < j; k++) list.remove(new Integer(candidates[i]));
                }  
            }
        }
    }
}

```

# 40.组合总和 II
## 题目
https://leetcode-cn.com/problems/combination-sum-ii
## 题解
39修改
## 代码

```java
class Solution {
    int length = 0;
    int[] nums = new int[201];
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);

        for(int i = 0; i < candidates.length; i++) {nums[candidates[i]]++; if(nums[candidates[i]] == 1) length++;}
        int[] candidate = new int[length];
        
        int temp = 0;
        for(int i = 1; i < 201; i++){
            if(nums[i] > 0) candidate[temp++] = i;
        }
        dfs(candidate, target, 0, 0, new ArrayList<Integer>());
        return result;
    }
    public void dfs(int[] candidates, int target, int temp, int sum, List<Integer> list){
        if(sum == target){
            List<Integer> ans = new ArrayList<Integer>();
            for(int i = 0; i < list.size(); i++) ans.add(list.get(i));
            result.add(ans);
        }
        else if(temp == length || candidates[temp] > target - sum) return;
        else{
            for(int i = temp; i < length; i++){
                if(candidates[i] > target - sum) return;
                for(int j = 1; j <= Math.min((target - sum) / candidates[i], nums[candidates[i]]); j++){
                    for(int k = 0; k < j; k++) list.add(candidates[i]);
                    dfs(candidates, target, i + 1, sum + j * candidates[i], list);
                    for(int k = 0; k < j; k++) list.remove(new Integer(candidates[i]));
                }  
            }
        }
    }
}

```


# 50.Pow(x, n)
## 题目
https://leetcode-cn.com/problems/powx-n/
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

# 54.螺旋矩阵
## 题目
https://leetcode-cn.com/problems/spiral-matrix/
## 题解
因为就四种走法，所以定义一个上up下down左left右right的边界，定义一个当前的坐标(x,y)，定义一个结果队列result：
- 1.左->右:result.add(matrix[x][y++]);
- 2.上->下:result.add(matrix[x++][y]);
- 3.右->左:result.add(matrix[x][y--]);
- 4.下->上:result.add(matrix[x--][y]);
- 上面四种情况while里面跑完记得更新下边界还有(x,y)坐标使得下一阶段起始时坐标是对的

返回结果result

## 代码

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        List<Integer> result = new ArrayList<>();
        //上下左右边界
        int up = 0;
        int down = m - 1;
        int left = 0;
        int right = n - 1;
        //当前坐标(x,y)
        int x = 0;
        int y = 0;
        while(up <= down && left <= right){
            //左->右
            while(y <= right && up <= down && left <= right) result.add(matrix[x][y++]);

            x = ++up;
            y = right;
            //上->下
            while(x <= down && up <= down && left <= right) result.add(matrix[x++][y]);

            x = down;
            y = --right;
            //右->左
            while(y >= left && up <= down && left <= right) result.add(matrix[x][y--]);
            
            x = --down;
            y = left;
            //下->上
            while(x >= up && up <= down && left <= right) result.add(matrix[x--][y]);
            
            x = up;
            y = ++left;
        }
        return result;
    }
}
```


# 79.单词搜索
## 题目
https://leetcode-cn.com/problems/word-search/
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
    int length;
    public boolean exist(char[][] board, String word) {
        m = board.length;
        n = board[0].length;
        length = word.length();
        boolean[][] used = new boolean[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(result)
                    return result;
                else if(board[i][j] == word.charAt(0))
                    dfs(board, used, i, j, 0, word);
                else 
                    continue;
            }
        }
        return result;
    }
    public void dfs(char[][] board, boolean[][] used, int x, int y, int temp, String word){
        if(temp == length) {result = true; return;}
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


# 81.搜索旋转排序数组 II

## 题目
https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii
## 题解
**共四种情况（请综合我下面给的例子来看后三种情况）**：
**1.左 == target 或 中 == target 或 右 == target：** 直接返回 true
**2.左 == 中：**此时target**可能在[左，mid]中**，**也可能在[mid + 1, r]中**，左 = 左 + 1
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
    public boolean search(int[] nums, int target) {
        int n = nums.length;
        boolean result = false;
        int l = 0, r = n - 1;
        while(l <= r){
            int mid = (l + r + 1) >> 1;
            if(nums[l] == target || nums[mid] == target || nums[r] == target) return true;
            else if(nums[l] == nums[mid]) l++;
            else if(nums[l] < nums[mid]){
                if(nums[l] > target || nums[mid] < target) l = mid;
                else{
                    l = l + 1;
                    r = mid;
                }
            }else{
                if(nums[l] > target && nums[mid] < target) l = mid;
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