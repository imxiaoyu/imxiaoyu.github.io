---
title: 'JAVA面向对象（上）'
date: 2021-04-23 21:06:54
tags:
	- JAVA
	- JAVA基础
---
# 一、类与对象
## 1.面向对象学习的三条主线：

> 1.Java类及类的成员：属性、方法、构造器；代码块、内部类
> 2.面向对象的大特征：**封装性、继承性、多态性、(抽象性)**
> 3.其它关键字：this、super、static、final、abstract、interface、package、import等

> “**大处着眼，小处着手**”


<!-- more -->

## 2.面向对象与面向过程（理解）
> 1.面向过程：强调的是功能行为，以函数为最小单位，考虑怎么做。
> 2.面向对象：强调具备了功能的对象，以类/对象为最小单位，考虑谁来做。

**举例对比：人把大象装进冰箱。**

* 1.**面向过程：**强调的是功能行为，以函数为最小单位，考虑怎么做。

	① 把冰箱门打开
	② 抬起大象，塞进冰箱
	② 把冰箱门关闭


 * 2.**面向对象：**强调具备了功能的对象，以类/对象为最小单位，考虑谁来做。


	人{
		打开(冰箱){
			冰箱.开开();
		}

		抬起(大象){
			大象.进入(冰箱);
		}

		关闭(冰箱){
			冰箱.闭合();
		}

	}

	冰箱{
		开开(){}
		闭合(){}
	}

	大象{
		进入(冰箱){
		}
	}

## 3.完成一个项目（或功能）的思路：

> 1.根据问题需要，选择问题所针对的现实世界中的实体。
> 2.从实体中寻找解决问题相关的属性和功能，这些属性和功能就形成了概念世界中的类。
> 3.把抽象的实体用计算机语言进行描述，形成计算机世界中类的定义。即借助某种程序语言，把类构造成计算机能够识别和处理的数据结构。
> 4.将类实例化成计算机世界中的对象。对象是计算机世界中解决问题的最终工具。


## 4.面向对象中两个重要的概念：

**类：**对一类事物的描述，是抽象的、概念上的定义
**对象：**是实际存在的该类事物的每个个体，因而也称为实例(instance)
  >面向对象程序设计的重点是类的设计
  >设计类，就是设计类的成员。

**二者的关系：**
  >对象，是由类new出来的，派生出来的。

## 5.面向对象思想落地实现的规则一
  >1.创建类，设计类的成员
  >2.创建类的对象
  >3.通过“对象.属性”或“对象.方法”调用对象的结构

补充：几个概念的使用说明
  >属性 = 成员变量 = field = 域、字段
  >方法 = 成员方法 = 函数 = method
  >创建类的对象 = 类的实例化 = 实例化类

## 6.对象的创建与对象的内存解析

**典型代码：**

	Person p1 = new Person();
	Person p2 = new Person();
	Person p3 = p1;//没有新创建一个对象，共用一个堆空间中的对象实体。

**说明：**
>如果创建了一个类的多个对象，则每个对象都独立的拥有一套类的属性。（非static的）
>意味着：如果我们修改一个对象的属性a，则不影响另外一个对象属性a的值。


## 7.匿名对象

>我们创建的对象，没显式的赋给一个变量名。即为匿名对象

**举例：**

	new Phone().sendEmail();
	new Phone().playGame();
		
	new Phone().price = 1999;
	new Phone().showPrice();//0.0

**应用场景：**

	PhoneMall mall = new PhoneMall();
	
	//匿名对象的使用
	mall.show(new Phone());
	其中，
	class PhoneMall{
		public void show(Phone phone){
			phone.sendEmail();
			phone.playGame();
		}
	}

## 8.理解"万事万物皆对象"

1.在Java语言范畴中，我们都将功能、结构等封装到类中，通过类的实例化，来调用具体的功能结构

>Scanner,String等
>文件：File
>网络资源：URL

2.涉及到Java语言与前端Html、后端的数据库交互时，前后端的结构在Java层面交互时，都体现为类、对象。


**例题：**
>定义类Student，包含三个属性：学号number(int)，年级state(int)，成绩score(int)。 创建20个学生对象，学号为1到20，年级和成绩都由随机数确定。
>问题一：打印出3年级(state值为3）的学生信息。
>问题二：使用冒泡排序按学生成绩排序，并遍历所有学生信息

	class Student_Test{
		public static void main(String[] args){
			Student_Test student = new Student_Test();
			Student[] Stu = new Student[20];
			Stu = student.Initi();//学生信息初始化
			student.print(Stu);//打印学生信息
			int id = 4;//选中一个学号
			student.print(Stu, id);//打印固定学号的学生信息
			student.Sort(Stu);//学生按照成绩从高到底排序
			student.print(Stu);//再打印学生信息
		}
		/**
		 * 初始化学生信息
		 */
		public Student[] Initi(){
			Student[] Stu = new Student[20];
			for(int i = 0; i < 20; i++){
				Stu[i] = new Student();
				Stu[i].number = i + 1;
				Stu[i].state = (int)(Math.random() * 6 + 1);
				Stu[i].score = (int)(Math.random() * 101);
			}
			return Stu;
		}
		void Sort(Student[] Stu){
			System.out.println("成绩排序：");
			for(int i = 0; i < 20; i++){
				for(int j = 0; j < 20 - i - 1; j++){
					if(Stu[j].score < Stu[j + 1].score){
						Student temp = new Student();
						temp = Stu[j];
						Stu[j] = Stu[j + 1];
						Stu[j + 1] = temp;
					}
				}
			}
		}
		void print(Student[] Stu){
			for(int i = 0; i < 20; i++){
				System.out.println("学号：" + Stu[i].number + " 年级：" 
				+ Stu[i].state + " 分数：" + Stu[i].score + "\n");
			}
		}
		void print(Student[] Stu, int t){
			for(int i = 0; i < 20; i++){
				if(Stu[i].state == t)
					System.out.println("学号：" + Stu[i].number + " 年级：" 
					+ Stu[i].state + " 分数：" + Stu[i].score + "\n");
				}
		}
	}
	public class Student {
		int number;
		int state;
		int score;
	}