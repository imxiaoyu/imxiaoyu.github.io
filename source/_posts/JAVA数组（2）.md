---
title: 'JAVA数组（2）'
date: 2021-04-22 15:24:33
tags:
	- JAVA
	- JAVA基础
---

# 一、数组中的算法

## 1. 数组的赋值与复制数据

	int[] array1,array2;
	array1 = new int[]{1,2,3,4};

	//赋值，相当于把地址直接给他了
	array2 = array1;

	//复制数据
	array2 = new int[array1.length];
	for(int i = 0;i < array2.length;i++){
		array2[i] = array1[i];
	}
<!-- more -->



## 2.数组的反转

**手写：**

	//方法一：
	for(int i = 0;i < arr.length / 2;i++){
		String temp = arr[i];
		arr[i] = arr[arr.length - i -1];
		arr[arr.length - i -1] = temp;
	}
		

	//方法二：
	for(int i = 0,j = arr.length - 1;i < j;i++,j--){
		String temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}

## 3.数组元素的查找

**线性查找：** 
>实现思路：通过遍历的方式，一个一个的数据进行比较、查找。
>适用性：具有普遍适用性。

**二分法查找：**
>实现思路：每次比较中间值，折半的方式检索。
>适用性：（前提：数组必须有序）

## 4.数组的排序

**会写：冒泡（BubbleSort）、快排（QuickSort）**
**理解：堆排序，归并排序**

>1）衡量排序算法的优劣：
>时间复杂度、空间复杂度、稳定性(AB相同，排序后AB前后不变则稳定)
>2）排序的分类：内部排序 与 外部排序（需要借助于磁盘）
>3）不同排序算法的时间复杂度


排序方法|时间复杂度（平均）|时间复杂度（最坏）|时间复杂度（最好）|空间复杂度|稳定性
:--|:--:|:--:|:--:|:--:|:--:
插入排序|O(n²)|O(n²)|O(n)|O(1)|稳定
希尔排序|O(n(1.3)次方)|O(n²)|O(n)|O(1)|不稳定
选择排序|O(n²)|O(n²)|O(n²)|O(1)|不稳定
**堆排序**|**O(n*log2(n))**|**O(n*log2(n))**|**O(n*log2(n))**|O(1)|不稳定
**冒泡排序**|**O(n²)**|**O(n²)**|**O(n)**|O(1)|稳定
**快速排序**|**O(n*log2(n))**|**O(n²)**|**O(n*log2(n))**|O(n*log2(n))|不稳定
**归并排序**|**O(n*log2(n))**|**O(n*log2(n))**|**O(n*log2(n))**|O(n)|稳定
计数排序|O(n+k)|O(n+k)|O(n+k)|O(n+k)|稳定
桶排序|O(n+k)|O(n²)|O(n)|O(n+k)|稳定
基数排序|O(n*k)|O(n*k)|O(n*k)|O(n+k)|稳定
**冒泡排序：**
	
	public class Sort {
		//BubbleSort
		/**
		 * 
		* @Description  冒泡排序
		* @author Rain
		* @date 2021年4月22日下午6:28:42
		* @param args
		 */
		public static void main(String[] args){
			int[] num = new int[]{25, 65, 12, 56, 67, 0, 54, 45};
			for(int i = 0; i < num.length; i++ ){
				for(int j = 0; j < num.length - i - 1; j++){
					if(num[j] > num[j + 1]){
						int temp = num[j];
						num[j] = num[j + 1];
						num[j + 1] = temp;
					}			
				}
			}
			System.out.print(java.util.Arrays.toString(num));
		}
	}


**快排：**

	public class QuickSort {
		private static void swap(int[] data, int i, int j) {
			int temp = data[i];
			data[i] = data[j];
			data[j] = temp;
		}

		//重要的是这个函数，写法可以记一下
		private static void subSort(int[] data, int start, int end) {
			if (start < end) {
				int base = data[start];
				int low = start;
				int high = end + 1;
				while (true) {
					while (low < end && data[++low] - base <= 0)
						;
					while (high > start && data[--high] - base >= 0)
						;
					if (low < high) {
						swap(data, low, high);
					} else {
						break;
					}
				}
				swap(data, start, high);
				
				subSort(data, start, high - 1);//递归调用
				subSort(data, high + 1, end);
			}
		}


		public static void quickSort(int[] data){
			subSort(data,0,data.length-1);
		}
		
		
		public static void main(String[] args) {
			int[] data = { 9, -16, 30, 23, -30, -49, 25, 21, 30 };
			System.out.println("排序之前：\n" + java.util.Arrays.toString(data));
			quickSort(data);
			System.out.println("排序之后：\n" + java.util.Arrays.toString(data));
		}
	}


# 二、Arrays工具类的使用

## 1.理解

① 定义在java.util包下。
② Arrays:提供了很多操作数组的方法。

## 2.使用

### (1).boolean equals(int[] a,int[] b) :判断两个数组是否相等

	int[] arr1 = new int[]{1,2,3,4};
	int[] arr2 = new int[]{1,3,2,4};
	boolean isEquals = Arrays.equals(arr1, arr2);
	System.out.println(isEquals);
		
### (2).String toString(int[] a) :输出数组信息

	System.out.println(Arrays.toString(arr1));
		
			
### (3).void fill(int[] a,int val) :将指定值填充到数组之中

	Arrays.fill(arr1,10);
	System.out.println(Arrays.toString(arr1));
		

### (4).void sort(int[] a) :对数组进行排序

	Arrays.sort(arr2);
	System.out.println(Arrays.toString(arr2));
		

### (5).int binarySearch(int[] a,int key) :二分查找

	int[] arr3 = new int[]{-98,-34,2,34,54,66,79,105,210,333};
	int index = Arrays.binarySearch(arr3, 210);
	if(index >= 0){
		System.out.println(index);
	}else{
		System.out.println("未找到");
	}