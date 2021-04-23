---
title: 'eclipse使用'
date: 2021-04-21 13:24:33
tags:
	- JAVA
	- JAVA基础
	- 软件使用
---

**以下教程基于eclipse-jee-neon-1a-win_64版本**

# 一、Eclipse 的使用配置
## 1. Eclipse 的安装
## 2.设置 workspace

首次启动,选择指定的工作空间(workspace)，用于存放 java 代码。
<!-- more -->

## 3.设置透视图(perspective)

右上角的红框,设置透视图选择 JavaEE

## 4.添加透视图需要显示的结构
右上角**Quick Access**文本框中依次输入相应结构的名字,调取出来。
> 结构 1：Package Explorer
> 结构 2：Navigator  
> 结构 3：Outline
 **1-3统一拖放在界面的左端:**
> 结构 4: Console 

## 5.关闭其它不需要的结构
## 6.设置编码集

	Window->Preferences->General->Workspace中：
		Text file encoding 中设置 Other中的 UTF-8

## 7.设置字体,字形,字体大小

	Window->Preferences->General->Appearance->Colors and For：
		Basic->Text Font 双击可以设置字体

## 8.设置 package explorer 中右键:new 下显示的结构

	Window->Preferences->Customize Perspective->Menu Visibility中：
	File左下角->New左边方框勾选去掉：
		去除 New 前的选择,从子列表中选择常用的结构即可。

## 9.保存当前透视图

	Window->Perspective->Save Perspective As
	Java EE(default)->OK：
		YES
	覆盖默认的透视图即可。

# 二、完成第一个 HelloWorld 程序

## 1. 创建工程
## 2. 创建包
	选中 src 后,右键,new-Package
	名字命名规范：
		com.imxiaoyu.java
## 3. 创建类
编写代码,并运行:

	package top.imxiaoyu.contact;
	// 快捷键 Alt+/
	public class HelloWorld {
		public static void main(String[] args) {
			System.out.println("helloworld!");
		}
	}


# 三、常见问题
## 1. 双击 Eclipse 启动图标，不能正常启动 Eclipse

启动不了的原因有很多种，这里需要大家从如下几个方面排查：
1. 环境变量是否正确配置，需要在命令行输入 javac.exe 或 java.exe 进行检查
2. 是否正确的安装了 JDK 和 JRE
3. 安装的 JDK 的版本（32 位还是 64 位），必须与 Eclipse 版本一致
4. 修改 Eclipse 安装目录下的 eclipse.ini 配置文件

## 2. 工程中的代码有乱码怎么办

原因：
出现乱码的代码所使用的字符编码集与工程设置使用的字符编码集不一致导致的。
如何解决：
建议修改乱码文件的字符编码集即可。
## 3. 运行程序，误选择了 Java 透视图，如何调整为 JavaEE 的
Window->Preferences->General->Network Connections->Perspectives中：

	Open a new perspective 勾选 In the same window

	Open the associated perspective when creating a new project 勾选 Never open

## 3. 如何在编写的代码中显示程序员的相关信息
依次选择：
Window-->Preferences-->Java-->Code Style-->Code Templates
点击 Comments 
**（1） 找到 Types 然后双击填入以下几个信息即可**

	/**
	* @Description
	* @author Rain Email:378745706@qq.com
	* @version
	* @date ${date}${time}
	* 
	*/
大家对应填写自己的信息即可。
**（2）找到 Methods 然后双击填入以下几个信息即可**

	/**
	* @Description 
	* @author Rain
	* @date ${date}${time}
	* ${tags}
	*/

大家对应填写自己的信息即可。

# 四、常用快捷键的使用
	
	* 1.补全代码的声明：alt + /
	* 2.快速修复: ctrl + 1 
	* 3.批量导包：ctrl + shift + o
	* 4.使用单行注释：ctrl + /
	* 5.使用多行注释： ctrl + shift + / 
	* 6.取消多行注释：ctrl + shift + \
	* 7.复制指定行的代码：ctrl + alt + down 或 ctrl + alt + up
	* 8.删除指定行的代码：ctrl + d
	* 9.上下移动代码：alt + up 或 alt + down
	* 10.切换到下一行代码空位：shift + enter
	* 11.切换到上一行代码空位：ctrl + shift + enter
	* 12.如何查看源码：ctrl + 选中指定的结构 或 ctrl + shift + t
	* 13.退回到前一个编辑的页面：alt + left 
	* 14.进入到下一个编辑的页面(针对于上面那条来说的)：alt + right
	* 15.光标选中指定的类，查看继承树结构：ctrl + t
	* 16.复制代码： ctrl + c
	* 17.撤销： ctrl + z
	* 18.反撤销： ctrl + y
	* 19.剪切：ctrl + x 
	* 20.粘贴：ctrl + v
	* 21.保存： ctrl + s
	* 22.全选：ctrl + a
	* 23.格式化代码： ctrl + shift + f
	* 24.选中数行，整体往后移动：tab
	* 25.选中数行，整体往前移动：shift + tab
	* 26.在当前类中，显示类结构，并支持搜索指定的方法、属性等：ctrl + o
	* 27.批量修改指定的变量名、方法名、类名等：alt + shift + r
	* 28.选中的结构的大小写的切换：变成大写： ctrl + shift + x
	* 29.选中的结构的大小写的切换：变成小写：ctrl + shift + y
	* 30.调出生成 getter/setter/构造器等结构： alt + shift + s
	* 31.显示当前选择资源(工程 or 文件)的属性：alt + enter
	* 32.快速查找：参照选中的 Word 快速定位到下一个 ：ctrl + k
	* 33.关闭当前窗口：ctrl + w
	* 34.关闭所有的窗口：ctrl + shift + w
	* 35.查看指定的结构使用过的地方：ctrl + alt + g
	* 36.查找与替换：ctrl + f
	* 37.最大化当前的 View：ctrl + m
	* 38.直接定位到当前行的首位：home
	* 39.直接定位到当前行的末位：end