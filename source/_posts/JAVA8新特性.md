---
title: 'JAVA jdk8新特性'
date: 2021/5/4 13:08:02 
tags:
	- JAVA
	- JAVA基础
---
# 一、Lambda表达式
## 1.Lambda表达式使用前后的对比
**举例一：**

	@Test
	public void test1(){
	
	    Runnable r1 = new Runnable() {
	        @Override
	        public void run() {
	            System.out.println("我爱北京天安门");
	        }
	    };
	
	    r1.run();
	
	    System.out.println("***********************");
	
	    Runnable r2 = () -> System.out.println("我爱北京故宫");
	
	    r2.run();
	}

<!-- more -->
**举例二：**

	@Test
	public void test2(){
	
	    Comparator<Integer> com1 = new Comparator<Integer>() {
	        @Override
	        public int compare(Integer o1, Integer o2) {
	            return Integer.compare(o1,o2);
	        }
	    };
	
	    int compare1 = com1.compare(12,21);
	    System.out.println(compare1);
	
	    System.out.println("***********************");
	    //Lambda表达式的写法
	    Comparator<Integer> com2 = (o1,o2) -> Integer.compare(o1,o2);
	
	    int compare2 = com2.compare(32,21);
	    System.out.println(compare2);
	
	
	    System.out.println("***********************");
	    //方法引用
	    Comparator<Integer> com3 = Integer :: compare;
	
	    int compare3 = com3.compare(32,21);
	    System.out.println(compare3);
	}

## 2.Lambda表达式的基本语法

>1.举例： (o1,o2) -> Integer.compare(o1,o2);
>2.格式：
>- :lambda操作符 或 箭头操作符
>- 左边：lambda形参列表 （其实就是接口中的抽象方法的形参列表
>- 右边：lambda体 （其实就是重写的抽象方法的方法体


## 3.如何使用：分为六种情况

**总结六种情况：**
>**左边：**lambda形参列表的参数类型可以省略(类型推断)；如果lambda形参列表只一个参数，其一对()也可以省略
>**右边：**lambda体应该使用一对{}包裹；如果lambda体只一条执行语句（可能是return语句，省略这一对{}和return关键字



# 二、函数式接口
## 1.函数式接口的使用说明
>1.如果一个接口中，只声明了一个抽象方法，则此接口就称为函数式接口。
>2.我们可以在一个接口上使用 @FunctionalInterface 注解，这样做可以检查它是否是一个函数式接口。
>3.Lambda表达式的本质：作为函数式接口的实例


## 2.Java8中关于Lambda表达式提供的4个基本的函数式接口
**具体使用：**

函数式接口|参数类型|返回类型|用途
:--:|:--:|:--:|:--:
Consumer<T> 消费型接口|T|void|对类型为T的对象应用操作，包含方法：void accept(T t)
Supplier<T> 供给型接口|无|T|返回类型为T的对象，包含方法：T get()
Function<T, R> 函数型接口|T|R|对类型为T的对象应用操作，并返回结果。结果是R类型的对象。包含方法：R apply(T t)
Predicate<T> 断定型接口|T|boolean|确定类型为T的对象是否满足某约束，并返回


## 3.总结
### (1)何时使用lambda表达式？
当需要对一个函数式接口实例化的时候，可以使用lambda表达式。

### (2)何时使用给定的函数式接口？
如果我们开发中需要定义一个函数式接口，首先看看在已有的jdk提供的函数式接口是否提供了能满足需求的函数式接口。如果有，则直接调用即可，不需要自己再自定义了。


# 三、方法引用

## 1.理解
方法引用可以看做是Lambda表达式深层次的表达。换句话说，方法引用就是Lambda表达式，也就是函数式接口的一个实例，通过方法的名字来指向一个方法。

## 2.使用情境
当要传递给Lambda体的操作，已经实现的方法了，可以使用方法引用！

## 3.格式
类(或对象) :: 方法名

## 4.分为如下的三种情况
    情况1     对象 :: 非静态方法
    情况2     类 :: 静态方法
    情况3     类 :: 非静态方法

## 5.要求
>1.要求接口中的抽象方法的形参列表和返回值类型与方法引用的方法的形参列表和返回值类型相同！（针对于情况1和情况2）
>2.当函数式接口方法的第一个参数是需要引用方法的调用者，并且第二个参数是需要引用方法的参数(或无参数)时：ClassName::methodName（针对于情况3）


## 6.使用建议
如果给函数式接口提供实例，恰好满足方法引用的使用情境，大家就可以考虑使用方法引用给函数式接口提供实例。如果大家不熟悉方法引用，那么还可以使用lambda表达式。

## 7.使用举例
// 情况一：对象 :: 实例方法
//Consumer中的void accept(T t)
//PrintStream中的void println(T t)

	@Test
	public void test1() {
		Consumer<String> con1 = str -> System.out.println(str);
		con1.accept("北京");
	
		System.out.println("*******************");
		PrintStream ps = System.out;
		Consumer<String> con2 = ps::println;
		con2.accept("beijing");
	}

//Supplier中的T get()
//Employee中的String getName()

	@Test
	public void test2() {
		Employee emp = new Employee(1001,"Tom",23,5600);
	
		Supplier<String> sup1 = () -> emp.getName();
		System.out.println(sup1.get());
	
		System.out.println("*******************");
		Supplier<String> sup2 = emp::getName;
		System.out.println(sup2.get());
	
	}

// 情况二：类 :: 静态方法
//Comparator中的int compare(T t1,T t2)
//Integer中的int compare(T t1,T t2)

	@Test
	public void test3() {
		Comparator<Integer> com1 = (t1,t2) -> Integer.compare(t1,t2);
		System.out.println(com1.compare(12,21));
	
		System.out.println("*******************");
	
		Comparator<Integer> com2 = Integer::compare;
		System.out.println(com2.compare(12,3));
	
	}

//Function中的R apply(T t)
//Math中的Long round(Double d)

	@Test
	public void test4() {
		Function<Double,Long> func = new Function<Double, Long>() {
			@Override
			public Long apply(Double d) {
				return Math.round(d);
			}
		};
	
		System.out.println("*******************");
	
		Function<Double,Long> func1 = d -> Math.round(d);
		System.out.println(func1.apply(12.3));
	
		System.out.println("*******************");
	
		Function<Double,Long> func2 = Math::round;
		System.out.println(func2.apply(12.6));
	}

// 情况：类 :: 实例方法  (难度)
// Comparator中的int comapre(T t1,T t2)
// String中的int t1.compareTo(t2)

	@Test
	public void test5() {
		Comparator<String> com1 = (s1,s2) -> s1.compareTo(s2);
		System.out.println(com1.compare("abc","abd"));
	
		System.out.println("*******************");
	
		Comparator<String> com2 = String :: compareTo;
		System.out.println(com2.compare("abd","abm"));
	}

//BiPredicate中的boolean test(T t1, T t2);
//String中的boolean t1.equals(t2)

	@Test
	public void test6() {
		BiPredicate<String,String> pre1 = (s1,s2) -> s1.equals(s2);
		System.out.println(pre1.test("abc","abc"));
	
		System.out.println("*******************");
		BiPredicate<String,String> pre2 = String :: equals;
		System.out.println(pre2.test("abc","abd"));
	}

// Function中的R apply(T t)
// Employee中的String getName();

	@Test
	public void test7() {
		Employee employee = new Employee(1001, "Jerry", 23, 6000);
	
	
		Function<Employee,String> func1 = e -> e.getName();
		System.out.println(func1.apply(employee));
	
		System.out.println("*******************");
	
		Function<Employee,String> func2 = Employee::getName;
		System.out.println(func2.apply(employee));
	}



# 四、构造器引用与数组引用
## 1.构造器引用格式
类名::new

## 2.构造器引用使用要求
和方法引用类似，函数式接口的抽象方法的形参列表和构造器的形参列表一致。抽象方法的返回值类型即为构造器所属的类的类型

## 3.构造器引用举例

	//Supplier中的T get()
	   //Employee的空参构造器：Employee()
	   @Test
	   public void test1(){
	
	       Supplier<Employee> sup = new Supplier<Employee>() {
	           @Override
	           public Employee get() {
	               return new Employee();
	           }
	       };
	       System.out.println("*******************");
	
	       Supplier<Employee>  sup1 = () -> new Employee();
	       System.out.println(sup1.get());
	
	       System.out.println("*******************");
	
	       Supplier<Employee>  sup2 = Employee :: new;
	       System.out.println(sup2.get());
	   }
	
	//Function中的R apply(T t)
	   @Test
	   public void test2(){
	       Function<Integer,Employee> func1 = id -> new Employee(id);
	       Employee employee = func1.apply(1001);
	       System.out.println(employee);
	
	       System.out.println("*******************");
	
	       Function<Integer,Employee> func2 = Employee :: new;
	       Employee employee1 = func2.apply(1002);
	       System.out.println(employee1);
	
	   }
	
	//BiFunction中的R apply(T t,U u)
	   @Test
	   public void test3(){
	       BiFunction<Integer,String,Employee> func1 = (id,name) -> new Employee(id,name);
	       System.out.println(func1.apply(1001,"Tom"));
	
	       System.out.println("*******************");
	
	       BiFunction<Integer,String,Employee> func2 = Employee :: new;
	       System.out.println(func2.apply(1002,"Tom"));
	
	   }

## 4.数组引用格式
数组类型[] :: new

## 5.数组引用举例

	//Function中的R apply(T t)
	@Test
	public void test4(){
	    Function<Integer,String[]> func1 = length -> new String[length];
	    String[] arr1 = func1.apply(5);
	    System.out.println(Arrays.toString(arr1));
	
	    System.out.println("*******************");
	
	    Function<Integer,String[]> func2 = String[] :: new;
	    String[] arr2 = func2.apply(10);
	    System.out.println(Arrays.toString(arr2));
	
	}

# 五、Stream API
## 1.Stream API的理解
>1.Stream关注的是对数据的运算，与CPU打交道
集合关注的是数据的存储，与内存打交道
>2.java8提供了一套api,使用这套api可以对内存中的数据进行过滤、排序、映射、归约等操作。类似于sql对数据库中表的相关操作。


## 2.注意点
>1.Stream 自己不会存储元素。
>2.Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
>3.Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。


## 3.Stream的使用流程
>1.Stream的实例化
>2.一系列的中间操作（过滤、映射、...)
>3.终止操作


## 4.使用流程的注意点
>1.一个中间操作链，对数据源的数据进行处理
>2.一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用


## 5.步骤一：Stream实例化

	//创建 Stream方式一：通过集合
	    @Test
	    public void test1(){
	        List<Employee> employees = EmployeeData.getEmployees();
	
	//        default Stream<E> stream() : 返回一个顺序流
	        Stream<Employee> stream = employees.stream();
	
	//        default Stream<E> parallelStream() : 返回一个并行流
	        Stream<Employee> parallelStream = employees.parallelStream();
	
	    }
	
	    //创建 Stream方式二：通过数组
	    @Test
	    public void test2(){
	        int[] arr = new int[]{1,2,3,4,5,6};
	        //调用Arrays类的static <T> Stream<T> stream(T[] array): 返回一个流
	        IntStream stream = Arrays.stream(arr);
	
	        Employee e1 = new Employee(1001,"Tom");
	        Employee e2 = new Employee(1002,"Jerry");
	        Employee[] arr1 = new Employee[]{e1,e2};
	        Stream<Employee> stream1 = Arrays.stream(arr1);
	
	    }
	    //创建 Stream方式三：通过Stream的of()
	    @Test
	    public void test3(){
	
	        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6);
	
	    }
	
	    //创建 Stream方式四：创建无限流
	    @Test
	    public void test4(){
	
	//      迭代
	//      public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f)
	        //遍历前10个偶数
	        Stream.iterate(0, t -> t + 2).limit(10).forEach(System.out::println);
	
	
	//      生成
	//      public static<T> Stream<T> generate(Supplier<T> s)
	        Stream.generate(Math::random).limit(10).forEach(System.out::println);
	
	    }

## 6.步骤二：中间操作
### (1)筛选与切片

方 法|描 述
:--:|:--:
filter(Predicate p)|接收 Lambda ， 从流中排除某些元素
distinct()|筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
limit(long maxSize)|截断流，使其元素不超过给定数量
skip(long n)|跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补

### (2)映射

方 法|描 述
:--:|:--:
map(Function f)|接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
mapToDouble(ToDoubleFunction f)|接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 DoubleStream。
mapToInt(ToIntFunction f)|接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 IntStream。
mapToLong(ToLongFunction f)|接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 LongStream。
flatMap(Function f)|接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流

### (3)排序

方 法|描 述
:--:|:--:
sorted()|产生一个新流，其中按自然顺序排序
sorted(Comparator com)|产生一个新流，其中按比较器顺序排序

## 7.步骤三：终止操作

### (1)匹配与查找

方 法|描 述
:--:|:--:
allMatch(Predicate p)|检查是否匹配所有元素
anyMatch(Predicate p)|检查是否至少匹配一个元素
noneMatch(Predicate p)|检查是否没有匹配所有元素
findFirst()|返回第一个元素
findAny()|返回当前流中的任意元素
count()|返回流中元素总数
max(Comparator c)|返回流中最大值
min(Comparator c)|返回流中最小值
forEach(Consumer c)|内部迭代(使用 Collection接口需要用户去做迭代，称为外部迭代。相反，Stream API 使用内部迭代——它帮你把迭代做了)

### (2)归约

方 法|描 述
:--:|:--:
reduce(T iden, BinaryOperator b)|可以将流中元素反复结合起来，得到一个值。返回 T
reduce(BinaryOperator b)|可以将流中元素反复结合起来，得到一个值。返回 Optional< T >

**备注：**map 和 reduce 的连接通常称为 map-reduce 模式，因 Google 
用它来进行网络搜索而出名。
### (3)收集

方 法|描 述
:--:|:--:
collect(Collector c)|将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法

>Collector 接口中方法的实现决定了如何对流执行收集的操作(如收集到 List、Set、Map)。
>另外， Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例，


**三个主要方法与实例如下表：**

方 法|返回类型|实 例|作 用
:--:|:--:|:--:|:--:
toList|List< T >|List<Employee> emps= list.stream().collect(Collectors.toList());|把流中元素收集到List
toSet|Set< T >|Set<Employee> emps= list.stream().collect(Collectors.toSet());|把流中元素收集到Set
toCollection|Collection< T >|Collection<Employee> emps =list.stream().collect(Collectors.toCollection(ArrayList::new));|把流中元素收集到创建的集合

# 六、java.util.Optional类的使用
## 1.理解：为了解决java中的空指针问题而生！
Optional<T> 类(java.util.Optional) 是一个容器类，它可以保存类型T的值，代表这个值存在。或者仅仅保存null，表示这个值不存在。原来用 null 表示一个值不存并且可以避免空指针异常在，现在 Optional 可以更好的表达这个概念。。

## 2.常用方法

	@Test
	    public void test1(){
	        //empty():创建的Optional对象内部的value = null
	        Optional<Object> op1 = Optional.empty();
	        if(!op1.isPresent()){//Optional封装的数据是否包含数据
	            System.out.println("数据为空");
	
	        }
	        System.out.println(op1);
	        System.out.println(op1.isPresent());
	        //如果Optional封装的数据value为空，则get()报错。否则，value不为空时，返回value.
	//        System.out.println(op1.get());
	
	    }
	
	    @Test
	    public void test2(){
	        String str = "hello";
	//        str = null;
	        //of(T t):封装数据t生成Optional对象。要求t非空，否则报错。
	        Optional<String> op1 = Optional.of(str);
	        //get()通常与of()方法搭配使用。用于获取内部的封装的数据value
	        String str1 = op1.get();
	        System.out.println(str1);
	
	    }
	
	    @Test
	    public void test3(){
	        String str = "beijing";
	        str = null;
	        //ofNullable(T t) ：封装数据t赋给Optional内部的value。不要求t非空
	        Optional<String> op1 = Optional.ofNullable(str);
	        //orElse(T t1):如果Optional内部的value非空，则返回此value值。如果
	        //value为空，则返回t1.
	        String str2 = op1.orElse("shanghai");
	
	        System.out.println(str2);//
	    }

## 3.典型练习

能保证如下的方法执行中不会出现空指针的异常。
//使用Optional类的getGirlName():

	public String getGirlName2(Boy boy){
	
	    Optional<Boy> boyOptional = Optional.ofNullable(boy);
	    //此时的boy1一定非空
	    Boy boy1 = boyOptional.orElse(new Boy(new Girl("迪丽热巴")));
	
	    Girl girl = boy1.getGirl();
	
	    Optional<Girl> girlOptional = Optional.ofNullable(girl);
	    //girl1一定非空
	    Girl girl1 = girlOptional.orElse(new Girl("古力娜扎"));
	
	    return girl1.getName();
	}
	
	@Test
	public void test5(){
	    Boy boy = null;
	    boy = new Boy();
	    boy = new Boy(new Girl("苍老师"));
	    String girlName = getGirlName2(boy);
	    System.out.println(girlName);
	}

