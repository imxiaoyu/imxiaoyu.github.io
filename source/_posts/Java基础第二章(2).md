---
title: 'JAVA语法（2）'
date: 2021-04-19 13:24:33
tags:
	- JAVA
	- JAVA基础
---


# 一、从键盘输入过程

**1. 导包：import java.util.Scanner;**
**2. Scanner的实例化：Scanner scan = new Scanner(System.in);**
**3. 调用Scanner类的相关方法（next() / nextXxx()……），来获取指定类型的变量**
<!-- more -->

	import java.util.Scanner;
	public class Test{
			public static void main(String[] args){
				Scanner scan  = new Scanner(System.in);
				int num = scan.nextInt();
				System.out.println(num);
			}
	}


# 二、如何获取一个随机数：10~99
	int value = (int)(Math.random() * 90 + 10);//[0.0, 1.0)-->[0.0, 90.0)-->[10.0, 100.0)-->[10.0, 99.0)
**公式：[a,b]:(int)(Math.random() * (b - a + 1) + a)**


# 三、switch case break default
	
	switch(表达式){
	case 常量1:
		执行语句1;
		//break;
	case 常量2:
		执行语句2;
		//break;
	...
	default:
		执行语句n;
		//break;
	}
**说明：**
1. 根据switch表达式中的值，依次匹配各个case中的常量。一旦匹配成功，则进入相应case结构中，调用其执行语句。当调用完执行语句以后，则仍然继续向下执行其他case结构中的执行语句，直到遇到break关键字或此switch-case结构末尾结束为止。
2. break,可以使用在switch-case结构中，表示一旦执行到此关键字，就跳出switch-case结构
3. switch结构中的表达式，只能是如下的6种数据类型之一：
   byte 、short、char、int、枚举类型(JDK5.0新增)、String类型(JDK7.0新增)
4. case 之后**只能声明常量**。**不能声明范围**。
5. default:相当于if-else结构中的else。
6. default结构是可选的，**而且位置是灵活的，即可以放在case前面。**
7. 如果switch-case结构中的多个case的执行语句相同，则可以考虑进行合并。
8. break在switch-case中是可选的



**说明：**
1. 凡是可以使用switch-case的结构，都可以转换为if-else。反之，不成立。
2. 我们写分支结构时，当发现既可以使用switch-case,（同时，switch中表达式的取值情况不太多），
  又可以使用if-else时，我们优先选择使用switch-case。原因：switch-case执行效率稍高。

## 题目1：键盘输入成绩合格输出yes，不合格输出no


	import java.util.Scanner;
	public class Test{
			public static void main(String[] args){
				Scanner scan  = new Scanner(System.in);
				int score = scan.nextInt();
				switch(score/10){
					case 0:
					case 1:
					case 2:
					case 3:
					case 4:
					case 5:
						System.out.println("no");
						break;
					case 6:
					case 7:
					case 8:
					case 9:
					case 10:
						System.out.println("yes");
						break;
					default:
						System.out.println("成绩错误");
						break;
				}
			}
	}



## 题目2：从键盘分别输入年、月、日，判断这一天是当年的第几天
 
**   注：判断一年是否是闰年的标准：
       1）可以被4整除，但不可被100整除
	或
       2）可以被400整除**



	import java.util.Scanner;
	class SwitchCaseExer {
		public static void main(String[] args) {
			
			Scanner scan = new Scanner(System.in);
			System.out.println("请输入year：");
			int year = scan.nextInt();
			System.out.println("请输入month：");
			int month = scan.nextInt();
			System.out.println("请输入day：");
			int day = scan.nextInt();
	
	
			//定义一个变量来保存总天数
			int sumDays = 0;
	
			switch(month){
			case 12:
				sumDays += 30;
			case 11:
				sumDays += 31;
			case 10:
				sumDays += 30;
			case 9:
				sumDays += 31;
			case 8:
				sumDays += 31;
			case 7:
				sumDays += 30;
			case 6:
				sumDays += 31;
			case 5:
				sumDays += 30;
			case 4:
				sumDays += 31;
			case 3:
				//sumDays += 28;
				//判断year是否是闰年
				if((year % 4 == 0 && year % 100 != 0 ) || year % 400 == 0){
					sumDays += 29;
				}else{
					sumDays += 28;
				}
	
			case 2:
				sumDays += 31;
			case 1:
				sumDays += day;
			}
	
			System.out.println(year + "年" + month + "月" + day + "日是当年的第" + sumDays + "天");
		}
	}
