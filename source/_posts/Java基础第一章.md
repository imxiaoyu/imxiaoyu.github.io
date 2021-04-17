---
title: 'JAVA第一章'
date: 2021-04-17 15:24:33
tags:
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
