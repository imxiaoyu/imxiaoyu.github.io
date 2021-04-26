---
title: 'JAVA异常处理'
date: 2021-04-26 22:03:54
tags:
	- JAVA
	- JAVA基础
---
# 一、异常
## 1. 异常的体系结构
	
	java.lang.Throwable
			|-----java.lang.Error:一般不编写针对性的代码进行处理。
			|-----java.lang.Exception:可以进行异常的处理
				|------编译时异常(checked)
						|-----IOException
							|-----FileNotFoundException
						|-----ClassNotFoundException
				|------运行时异常(unchecked,RuntimeException)
						|-----NullPointerException
						|-----ArrayIndexOutOfBoundsException
						|-----ClassCastException
						|-----NumberFormatException
						|-----InputMismatchException
						|-----ArithmeticException

<!-- more -->


## 2.从程序执行过程，看编译时异常和运行时异常

>**编译时异常：**执行javac.exe命名时，可能出现的异常
>**运行时异常：**执行java.exe命名时，出现的异常

## 3.常见的异常类型，请举例说明

**以下是运行时异常:**

	//1.ArithmeticException
	@Test
	public void test6(){
		int a = 10;
		int b = 0;
		System.out.println(a / b);
	}
	
	//2.InputMismatchException
	@Test
	public void test5(){
		Scanner scanner = new Scanner(System.in);
		int score = scanner.nextInt();
		System.out.println(score);
		
		scanner.close();
	}
	
	//3.NumberFormatException
	@Test
	public void test4(){
		
		String str = "123";
		str = "abc";
		int num = Integer.parseInt(str);
		
		
		
	}
	
	//4.ClassCastException
	@Test
	public void test3(){
		Object obj = new Date();
		String str = (String)obj;
	}
	
	//5.IndexOutOfBoundsException
	@Test
	public void test2(){
		//ArrayIndexOutOfBoundsException
		int[] arr = new int[10];
		System.out.println(arr[10]);
		//StringIndexOutOfBoundsException
		String str = "abc";
		System.out.println(str.charAt(3));
	}
	
	//6.NullPointerException
	@Test
	public void test1(){
		
		int[] arr = null;
		System.out.println(arr[3]);
		
		String str = "abc";
		str = null;
		System.out.println(str.charAt(0));
		
	}

**以下是编译时异常:**

	@Test
	public void test7(){
		File file = new File("hello.txt");
		FileInputStream fis = new FileInputStream(file);
		
		int data = fis.read();
		while(data != -1){
			System.out.print((char)data);
			data = fis.read();
		}
		
		fis.close();
		
	}

# 二、异常的处理

## 1.java异常处理的抓抛模型

>**过程一：**"抛"：程序在正常执行的过程中，一旦出现异常，就会在异常代码处生成一个对应异常类的对象。
>* 并将此对象抛出。
>* 一旦抛出对象以后，其后的代码就不再执行。
>* 关于异常对象的产生：
> ① 系统自动生成的异常对象
> ② 手动的生成一个异常对象，并抛出（throw）


>**过程二：**"抓"：可以理解为异常的处理方式：① try-catch-finally  ② throws



## 2.异常处理方式一：try-catch-finally
### (1)使用说明
	try{
	  		//可能出现异常的代码
	  
	  }catch(异常类型1 变量名1){
	  		//处理异常的方式1
	  }catch(异常类型2 变量名2){
	  		//处理异常的方式2
	  }catch(异常类型3 变量名3){
	  		//处理异常的方式3
	  }
	  ....
	  finally{
	  		//一定会执行的代码
	  }

**说明：**

>1.finally是可的。
>2.使用try将可能出现异常代码包装起来，在执行过程中，一旦出现异常，就会生成一个对应异常类的对象，根据此对象的类型，去catch中进行匹配
>3.一旦try中的异常对象匹配到某一个catch时，就进入catch中进行异常的处理。一旦处理完成，就跳出当前的try-catch结构（在没写finally的情况。继续执行其后的代码
>4.catch中的异常类型如果没子父类关系，则谁声明在上，谁声明在下无所谓。
>catch中的异常类型如果满足子父类关系，则要求子类一定声明在父类的上面。否则，报错
>5.常用的异常对象处理的方式：
> ① String  getMessage()    
> ② printStackTrace()
>6.在try结构中声明的变量，再出了try结构以后，就不能再被调用
>7.try-catch-finally结构可以嵌套


**总结：如何看待代码中的编译时异常和运行时异常？**

>**体会1：**使用try-catch-finally处理编译时异常，是得程序在编译时就不再报错，但是运行时仍可能报错。相当于我们使用try-catch-finally将一个编译时可能出现的异常，延迟到运行时出现。
>**体会2：**开发中，由于运行时异常比较常见，所以我们通常就不针对运行时异常编写try-catch-finally了。针对于编译时异常，我们说一定要考虑异常的处理。


### (2)finally的再说明
>1.finally是可的
>2.finally中声明的是一定会被执行的代码。即使catch中又出现异常了，try中return语句，catch中return语句等情况。
>3.像数据库连接、输入输出流、网络编程Socket等资源，JVM是不能自动的回收的，我们需要自己手动的进行资源的释放。此时的资源释放，就需要声明在finally中。


### (3)面试题

**final、finally、finalize三者的区别？**

**类似：**
>1.throw 和 throws
>2.Collection 和 Collections
>3.String 、StringBuffer、StringBuilder
>4.ArrayList 、 LinkedList
>5.HashMap 、LinkedHashMap
>6.重写、重载


**结构不相似的：**

>1.抽象类、接口
>2.== 、 equals()
>3.sleep()、wait()


## 3.异常处理方式二
>1."throws + 异常类型"写在方法的声明处。指明此方法执行时，可能会抛出的异常类型。
>2.一旦当方法体执行时，出现异常，仍会在异常代码处生成一个异常类的对象，此对象满足throws后异常类型时，就会被抛出。异常代码后续的代码，就不再执行！


## 4. 对比两种处理方式
>1.try-catch-finally:真正的将异常给处理掉了。
>2.throws的方式只是将异常抛给了方法的调用者。并没真正将异常处理掉。


## 5. 体会开发中应该如何选择两种处理方式？
>1.如果父类中被重写的方法没throws方式处理异常，则子类重写的方法也不能使用throws，意味着如果子类重写的方法中异常，必须使用try-catch-finally方式处理。
>2.执行的方法a中，先后又调用了另外的几个方法，这几个方法是递进关系执行的。我们建议这几个方法使用throws的方式进行处理。而执行的方法a可以考虑使用try-catch-finally方式进行处理。


**补充：**
>方法重写的规则之一：
>子类重写的方法抛出的异常类型不大于父类被重写的方法抛出的异常类型


# 三、手动抛出异常对象

## 1.使用说明
>在程序执行中，除了自动抛出异常对象的情况之外，我们还可以手动的throw一个异常类的对象。


## 2.面试题 

>**throw 和  throws区别：**
>throw 表示抛出一个异常类的对象，生成异常对象的过程。声明在方法体内。
>throws 属于异常处理的一种方式，声明在方法的声明处。


## 3.典型例题

	class Student{
		
		private int id;
		
		public void regist(int id) throws Exception {
			if(id > 0){
				this.id = id;
			}else{
				//手动抛出异常对象
				throw new RuntimeException("您输入的数据非法！");
				throw new Exception("您输入的数据非法！");
				throw new MyException("不能输入负数");
	
			}
			
		}
	
		@Override
		public String toString() {
			return "Student [id=" + id + "]";
		}
		
		
	}


# 四、自定义异常类

## 1.如何自定义一个异常类？

>**如何自定义异常类？**
>1.继承于现的异常结构：RuntimeException 、Exception
>2.提供全局常量：serialVersionUID
>3.提供重载的构造器

**例子：**

	public class MyException extends Exception{
		
		static final long serialVersionUID = -7034897193246939L;
		
		public MyException(){
			
		}
		
		public MyException(String msg){
			super(msg);
		}
	}


