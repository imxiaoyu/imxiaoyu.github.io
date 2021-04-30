---
title: 'JAVA枚举类和注解'
date: 2021/4/30 20:19:22 
tags:
	- JAVA
	- JAVA基础
---

# 一、枚举类的使用
## 1.枚举类的说明
>1.枚举类的理解：类的对象只有有限个，确定的。我们称此类为枚举类
>2.当需要定义一组常量时，强烈建议使用枚举类
>3.如果枚举类中只一个对象，则可以作为单例模式的实现方式。


<!-- more -->
## 2.如何自定义枚举类？

	//自定义枚举类
	class Season{
	    //1.声明Season对象的属性:private final修饰
	    private final String seasonName;
	    private final String seasonDesc;
	
	    //2.私化类的构造器,并给对象属性赋值
	    private Season(String seasonName,String seasonDesc){
	        this.seasonName = seasonName;
	        this.seasonDesc = seasonDesc;
	    }
	
	    //3.提供当前枚举类的多个对象：public static final的
	    public static final Season SPRING = new Season("春天","春暖花开");
	    public static final Season SUMMER = new Season("夏天","夏日炎炎");
	    public static final Season AUTUMN = new Season("秋天","秋高气爽");
	    public static final Season WINTER = new Season("冬天","冰天雪地");
	
	    //4.其他诉求1：获取枚举类对象的属性
	    public String getSeasonName() {
	        return seasonName;
	    }
	
	    public String getSeasonDesc() {
	        return seasonDesc;
	    }
	    //4.其他诉求1：提供toString()
	    @Override
	    public String toString() {
	        return "Season{" +
	                "seasonName='" + seasonName + '\'' +
	                ", seasonDesc='" + seasonDesc + '\'' +
	                '}';
	    }
	}

## 3.jdk 5.0 新增使用enum定义枚举类

	//使用enum关键字枚举类
	enum Season1 {
	    //1.提供当前枚举类的对象，多个对象之间用","隔开，末尾对象";"结束
	    SPRING("春天","春暖花开"),
	    SUMMER("夏天","夏日炎炎"),
	    AUTUMN("秋天","秋高气爽"),
	    WINTER("冬天","冰天雪地");
	
	    //2.声明Season对象的属性:private final修饰
	    private final String seasonName;
	    private final String seasonDesc;
	
	    //2.私化类的构造器,并给对象属性赋值
	
	    private Season1(String seasonName,String seasonDesc){
	        this.seasonName = seasonName;
	        this.seasonDesc = seasonDesc;
	    }
	
	    //4.其他诉求1：获取枚举类对象的属性
	    public String getSeasonName() {
	        return seasonName;
	    }
	
	    public String getSeasonDesc() {
	        return seasonDesc;
	    }
	
	}


## 4.使用enum定义枚举类之后，枚举类常用方法：（继承于java.lang.Enum类）

	Season1 summer = Season1.SUMMER;
	        //toString():返回枚举类对象的名称
	        System.out.println(summer.toString());
	
	//        System.out.println(Season1.class.getSuperclass());
	        System.out.println("****************");
	        //values():返回所的枚举类对象构成的数组
	        Season1[] values = Season1.values();
	        for(int i = 0;i < values.length;i++){
	            System.out.println(values[i]);
	        }
	        System.out.println("****************");
	        Thread.State[] values1 = Thread.State.values();
	        for (int i = 0; i < values1.length; i++) {
	            System.out.println(values1[i]);
	        }
	
	        //valueOf(String objName):返回枚举类中对象名是objName的对象。
	        Season1 winter = Season1.valueOf("WINTER");
	        //如果没objName的枚举类对象，则抛异常：IllegalArgumentException
	//        Season1 winter = Season1.valueOf("WINTER1");
	        System.out.println(winter);


## 5.使用enum定义枚举类之后，如何让枚举类对象分别实现接口
	interface Info{
	    void show();
	}
	
	//使用enum关键字枚举类
	enum Season1 implements Info{
	    //1.提供当前枚举类的对象，多个对象之间用","隔开，末尾对象";"结束
	    SPRING("春天","春暖花开"){
	        @Override
	        public void show() {
	            System.out.println("春天在哪里？");
	        }
	    },
	    SUMMER("夏天","夏日炎炎"){
	        @Override
	        public void show() {
	            System.out.println("宁夏");
	        }
	    },
	    AUTUMN("秋天","秋高气爽"){
	        @Override
	        public void show() {
	            System.out.println("秋天不回来");
	        }
	    },
	    WINTER("冬天","冰天雪地"){
	        @Override
	        public void show() {
	            System.out.println("大约在冬季");
	        }
	    };
	}


# 二、注解的使用
## 1.注解的理解
>1.jdk 5.0 新增的功能
>2.Annotation 其实就是代码里的特殊标记, 这些标记可以在编译, 类加载, 运行时被读取, 并执行相应的处理。通过使用 Annotation,
>- 程序员可以在不改变原逻辑的情况下, 在源文件中嵌入一些补充信息。
>
>3.在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在JavaEE/Android
>- 中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替JavaEE旧版中所遗留的繁冗
>- 代码和XML配置等。

**框架 = 注解 + 反射机制 + 设计模式**

## 2.注解的使用示例
>**示例一：**生成文档相关的注解
>**示例二：**在编译时进行格式检查(JDK内置的个基本注解)
>@Override: 限定重写父类方法, 该注解只能用于方法
>@Deprecated: 用于表示所修饰的元素(类, 方法等)已过时。通常是因为所修饰的结构危险或存在更好的择
>@SuppressWarnings: 抑制编译器警告

>**示例：**跟踪代码依赖性，实现替代配置文件功能


## 3.如何自定义注解：参照@SuppressWarnings定义


>1.注解声明为：@interface
>2.内部定义成员，通常使用value表示
>3.可以指定成员的默认值，使用default定义
>4.如果自定义注解没成员，表明是一个标识作用。


>**说明：**
>如果注解有成员，在使用注解时，需要指明成员的值。
>自定义注解必须配上注解的信息处理流程(使用反射)才意义。
>自定义注解通过都会指明两个元注解：Retention、Target

**代码举例：**

	@Inherited
	@Repeatable(MyAnnotations.class)
	@Retention(RetentionPolicy.RUNTIME)
	@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE,TYPE_PARAMETER,TYPE_USE})
	public @interface MyAnnotation {
	
	    String value() default "hello";
	}

## 4.元注解 ：对现有的注解进行解释说明的注解

>**jdk 提供的4种元注解：**
>**1.Retention：**指定所修饰的 Annotation 的生命周期：SOURCE \ CLASS（默认行为） \ RUNTIME
>- 只声明为RUNTIME生命周期的注解，才能通过反射获取。
>
>**2.Target:**用于指定被修饰的 Annotation 能用于修饰哪些程序元素
>**下面两个出现的频率较低:**
>**3.Documented:**表示所修饰的注解在被javadoc解析时，保留下来。
>**4.Inherited:**被它修饰的 Annotation 将具继承性。
>- 类比：元数据的概念：String name = "Tom";

## 5.如何获取注解信息:通过发射来进行获取、调用

**前提：**要求此注解的元注解Retention中声明的生命周期状态为：RUNTIME.
## 6.JDK8中注解的新特性：可重复注解、类型注解

### (1)可重复注解
>1.在MyAnnotation上声明@Repeatable，成员值为MyAnnotations.class
>2.MyAnnotation的Target和Retention等元注解与MyAnnotations相同。


### (2)类型注解
>1.ElementType.TYPE_PARAMETER 表示该注解能写在类型变量的声明语句中（如：泛型声明。
>2.ElementType.TYPE_USE 表示该注解能写在使用类型的任何语句中。



