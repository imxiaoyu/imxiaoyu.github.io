---
title: 'JAVA语法（3）'
date: 2021-04-20 23:08:54
tags:
	- JAVA
	- JAVA基础
---

# 一、循环结构

1. for(;;)
2. while()
3. do{……} while()  //特点：肯定会执行一次do


<!-- more -->

**系统当前时间：**

	 System.currentTimeMillis()


## 1.问题：输出质数 算法优化

**问题：**
>100000以内的所有质数的输出。
>质数：素数，只能被1和它本身整除的自然数。-->从2开始，到这个数-1结束为止，都不能被这个数本身整除。


### 实现方式一：使用break和sqrt进行优化


	class PrimeNumberTest1 {
		public static void main(String[] args) {
			
			boolean isFlag = true;//标识i是否被j除尽，一旦除尽，修改其值
			int count = 0;//记录质数的个数
	
			//获取当前时间距离1970-01-01 00:00:00 的毫秒数
			long start = System.currentTimeMillis();
	
			for(int i = 2;i <= 100000;i++){//遍历100000以内的自然数
				
				//优化二：对本身是质数的自然数是有效的。
				//for(int j = 2;j < i;j++){
				for(int j = 2;j <= Math.sqrt(i);j++){//j:被i去除
					
					if(i % j == 0){ //i被j除尽
						isFlag = false;
						break;//优化一：只对本身非质数的自然数是有效的。
					}
					
				}
				//
				if(isFlag == true){
					//System.out.println(i);
					count++;
				}
				//重置isFlag
				isFlag = true;
			
			}
	
			//获取当前时间距离1970-01-01 00:00:00 的毫秒数
			long end = System.currentTimeMillis();
			System.out.println("质数的个数为：" + count);
			System.out.println("所花费的时间为：" + (end - start));//17110 - 优化一：break:1546 - 优化二：13
	
		}
	}

### 实现方式二：使用continue标签的方式优化



	class PrimeNumberTest2 {
		public static void main(String[] args) {
			
			
			int count = 0;//记录质数的个数
	
			//获取当前时间距离1970-01-01 00:00:00 的毫秒数
			long start = System.currentTimeMillis();
	
			label:for(int i = 2;i <= 100000;i++){//遍历100000以内的自然数
				
				for(int j = 2;j <= Math.sqrt(i);j++){//j:被i去除
					
					if(i % j == 0){ //i被j除尽
						continue label;
					}
					
				}
				//能执行到此步骤的，都是质数
				count++;
			
			}
	
			//获取当前时间距离1970-01-01 00:00:00 的毫秒数
			long end = System.currentTimeMillis();
			System.out.println("质数的个数为：" + count);
			System.out.println("所花费的时间为：" + (end - start));//17110 - 优化一：break:1546 - 优化二：13
	
		}
	}



# 二、break 和 continue

\|使用范围|不同点|相同点
---|:--:|:-----:|:----:
break|switch-case 或 循环结构中|结束**当前**循环|关键字后面不能声明执行语句
continue|循环结构中|结束**当次**循环|关键字后面不能声明执行语句


**continue标签用法是直接跳过去，跟goto差不多**

	label:for(;;){
			for(;;){
				if(){ 
					continue label;//跟goto差不多 直接跳过去
				}
			}
	}

**break标签用法为结束指定标签的for循环**

	label:for(;;){
			for(;;){
				if(){ 
					break label;//直接结束最外层for循环
				}
			}
	}

# 三、项目