---
title: 'JAVA第一章'
date: 2021-04-17 15:24:33
tags:
	- JAVA
	- JAVA基础
---

# java文档注释命令行生成doc
	javadoc -d myHello -author -version HelloChina.java



<!-- more -->
HelloChina.java 内的代码

	/**
	@author YY
	@version v1.0
	*/
	
	public class HelloChina{
		public static void main(String[] args){
			System.out.println("Hello World");
			}
	}
# 核心机制—Java虚拟机


## JVM(Java Virtal Machine)是一个虚拟的计算机，具有指令集并使用不同的存储区域。负责执行指令，管理数据、内存、寄存器。
	1.  对于不同的平台，有不同的虚拟机。
	2.  只有某平台提供了对应的java虚拟机，java程序才可在此平台运行
	3.  Java虚拟机机制屏蔽了底层运行平台的差别，实现了“一次编译，到处运行”
# 核心机制—垃圾回收
	1.  不再使用的内存空间应回收—— 垃圾回收。
		在C/C++等语言中，由程序员负责回收无用内存。
		Java语言消除了程序员回收无用内存空间的责任：它提供一种系统级线程跟踪存储空间的分配情况。
		并在JVM空闲时，检查并释放那些可被释放的存储空间。
	2.  垃圾回收在Java程序运行过程中自动进行，程序员无法精确控制和干预。
## Java程序还会出现内存泄漏和内存溢出问题吗？Yes!