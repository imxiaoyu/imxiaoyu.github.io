---
title: 'JAVA泛型'
date: 2021/5/1 18:40:05 
tags:
	- JAVA
	- JAVA基础
---
# 一、泛型的理解
## 1.泛型的概念
>1.所谓泛型，就是允许在定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返
>2.回值及参数类型。这个类型参数将在使用时（例如，继承或实现这个接口，用这个类型声明变量、
>3.创建对象时确定（即传入实际的类型参数，也称为类型实参）。


<!-- more -->
## 2.泛型的引入背景
>集合容器类在设计阶段/声明阶段不能确定这个容器到底实际存的是什么类型的对象，所以在JDK1.5之前只能把元素类型设计为Object，JDK1.5之后使用泛型来解决。因为这个时候除了元素的类型不确定，其他的部分是确定的，例如关于这个元素如何保存，如何管理等是确定的，因此此时把元素的类型设计成一个参数，这个类型参数叫做泛型。Collection<E>，List<E>，ArrayList<E>   这个<E>就是类型参数，即泛型。


# 二、泛型在集合中的使用

## 1.在集合中使用泛型之前的例子
	@Test
	    public void test1(){
	        ArrayList list = new ArrayList();
	        //需求：存放学生的成绩
	        list.add(78);
	        list.add(76);
	        list.add(89);
	        list.add(88);
	        //问题一：类型不安全
	//        list.add("Tom");
	
	        for(Object score : list){
	            //问题二：强转时，可能出现ClassCastException
	            int stuScore = (Integer) score;
	
	            System.out.println(stuScore);
	
	        }
	
	    }


## 2.在集合中使用泛型例子1

	@Test
	    public void test2(){
	       ArrayList<Integer> list =  new ArrayList<Integer>();
	
	        list.add(78);
	        list.add(87);
	        list.add(99);
	        list.add(65);
	        //编译时，就会进行类型检查，保证数据的安全
	//        list.add("Tom");
	
	        //方式一：
	//        for(Integer score : list){
	//            //避免了强转操作
	//            int stuScore = score;
	//
	//            System.out.println(stuScore);
	//
	//        }
	        //方式二：
	        Iterator<Integer> iterator = list.iterator();
	        while(iterator.hasNext()){
	            int stuScore = iterator.next();
	            System.out.println(stuScore);
	        }
	
	    }

    



## 3.在集合中使用泛型例子2

**在集合中使用泛型的情况：以HashMap为例**

	    @Test
	    public void test3(){
	//        Map<String,Integer> map = new HashMap<String,Integer>();
	        //jdk7新特性：类型推断
	        Map<String,Integer> map = new HashMap<>();
	
	        map.put("Tom",87);
	        map.put("Jerry",87);
	        map.put("Jack",67);
	
	//        map.put(123,"ABC");
	        //泛型的嵌套
	        Set<Map.Entry<String,Integer>> entry = map.entrySet();
	        Iterator<Map.Entry<String, Integer>> iterator = entry.iterator();
	
	        while(iterator.hasNext()){
	            Map.Entry<String, Integer> e = iterator.next();
	            String key = e.getKey();
	            Integer value = e.getValue();
	            System.out.println(key + "----" + value);
	        }
	
	    }


## 4.集合中使用泛型总结

>1.集合接口或集合类在jdk5.0时都修改为带泛型的结构。
>2.在实例化集合类时，可以指明具体的泛型类型
>3.指明完以后，在集合类或接口中凡是定义类或接口时，内部结构（比如：方法、构造器、属性等）使用到类的泛型的位置，都指定为实例化的泛型类型。
>- 比如：add(E e)  --->实例化以后：add(Integer e)
>
>4.注意点：泛型的类型必须是类，不能是基本数据类型。需要用到基本数据类型的位置，拿包装类替换
>5.如果实例化时，没指明泛型的类型。默认类型为java.lang.Object类型。


# 三、自定义泛型类、泛型接口、泛型方法
## 1.举例

### (1)Order.java
	
	public class Order<T> {
	
	    String orderName;
	    int orderId;
	
	    //类的内部结构就可以使用类的泛型
	
	    T orderT;
	
	    public Order(){
	        //编译不通过
	//        T[] arr = new T[10];
	        //编译通过
	        T[] arr = (T[]) new Object[10];
	    }
	
	    public Order(String orderName,int orderId,T orderT){
	        this.orderName = orderName;
	        this.orderId = orderId;
	        this.orderT = orderT;
	    }
	
	    //如下的个方法都不是泛型方法
	    public T getOrderT(){
	        return orderT;
	    }
	
	    public void setOrderT(T orderT){
	        this.orderT = orderT;
	    }
	
	    @Override
	    public String toString() {
	        return "Order{" +
	                "orderName='" + orderName + '\'' +
	                ", orderId=" + orderId +
	                ", orderT=" + orderT +
	                '}';
	    }
	    //静态方法中不能使用类的泛型。
	//    public static void show(T orderT){
	//        System.out.println(orderT);
	//    }
	
	    public void show(){
	        //编译不通过
	//        try{
	//
	//
	//        }catch(T t){
	//
	//        }
	
	    }
	
	    //泛型方法：在方法中出现了泛型的结构，泛型参数与类的泛型参数没任何关系。
	    //换句话说，泛型方法所属的类是不是泛型类都没关系。
	    //泛型方法，可以声明为静态的。原因：泛型参数是在调用方法时确定的。并非在实例化类时确定。
	    public static <E>  List<E> copyFromArrayToList(E[] arr){
	
	        ArrayList<E> list = new ArrayList<>();
	
	        for(E e : arr){
	            list.add(e);
	        }
	        return list;
	
	    }
	}


### (2)SubOrder.java

	public class SubOrder extends Order<Integer> {//SubOrder:不是泛型类
	
	
	    public static <E> List<E> copyFromArrayToList(E[] arr){
	
	        ArrayList<E> list = new ArrayList<>();
	
	        for(E e : arr){
	            list.add(e);
	        }
	        return list;
	
	    }
	
	
	}
	
	
	//实例化时，如下的代码是错误的
	SubOrder<Integer> o = new SubOrder<>();
	
	【SubOrder1.java】
	public class SubOrder1<T> extends Order<T> {//SubOrder1<T>:仍然是泛型类
	
	}


**测试**

	@Test
	    public void test1(){
	        //如果定义了泛型类，实例化没指明类的泛型，则认为此泛型类型为Object类型
	        //要求：如果大家定义了类是带泛型的，建议在实例化时要指明类的泛型。
	        Order order = new Order();
	        order.setOrderT(123);
	        order.setOrderT("ABC");
	
	        //建议：实例化时指明类的泛型
	        Order<String> order1 = new Order<String>("orderAA",1001,"order:AA");
	
	        order1.setOrderT("AA:hello");
	
	    }
	
	    @Test
	    public void test2(){
	        SubOrder sub1 = new SubOrder();
	        //由于子类在继承带泛型的父类时，指明了泛型类型。则实例化子类对象时，不再需要指明泛型。
	        sub1.setOrderT(1122);
	
	        SubOrder1<String> sub2 = new SubOrder1<>();
	        sub2.setOrderT("order2...");
	    }
	
	    @Test
	    public void test3(){
	
	        ArrayList<String> list1 = null;
	        ArrayList<Integer> list2 = new ArrayList<Integer>();
	        //泛型不同的引用不能相互赋值。
	//        list1 = list2;
	
	        Person p1 = null;
	        Person p2 = null;
	        p1 = p2;
	
	
	    }
	
	    //测试泛型方法
	    @Test
	    public void test4(){
	        Order<String> order = new Order<>();
	        Integer[] arr = new Integer[]{1,2,3,4};
	        //泛型方法在调用时，指明泛型参数的类型。
	        List<Integer> list = order.copyFromArrayToList(arr);
	
	        System.out.println(list);
	    }

## 2.注意点

>1泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。比如:<E1,E2,E3>
>2.泛型类的构造器如下: public GenericClass(){}
而下面是错误的: public GenericClass<E>(){}
>3.实例化后，操作原来泛型位置的结构必须与指定的泛型类型一致。
>4.泛型不同的引用不能相互赋值。
>- 尽管在编译时ArrayList<String>和ArrayList<Integer>是两种类型，但是，在运行时只有一个ArrayList被加载到JVM中。
>
>5.泛型如果不指定，将被擦除，泛型对应的类型均按照Object处理，但不等价于Object。
>- **经验:**泛型要使用一路都用。要不用，一路都不要用。
>
>6.如果泛型结构是一个接口或抽象类，则不可创建泛型类的对象。
>7.jdk1.7，泛型的简化操作:ArrayList<Fruit> flist = new ArrayList<>();
>8.泛型的指定中不能使用基本数据类型，可以使用包装类替换。
>9.在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的类型、非静态方法的参数类型、非静态方法的返回值类型。但在静态方法中不能使用类的泛型。
>10.异常类不能是泛型的
>11.不能使用new E[]。但是可以:E[]elements =(E[])new Object[capacity];
>- **参考:** ArrayList源码中声明: Object[] elementData，而非泛型参数类型数组。
>
>12.父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型:
>- 子类不保留父类的泛型:按需实现
>没有类型擦除
具体类型
>- 子类保留父类的泛型:泛型子类
>全部保留
>部分保留
>
>**结论:**子类必须是“富二代”，子类除了指定或保留父类的泛型，还可以增加自己的泛型

## 3.应用场景举例
### (1)DAO.java

定义了操作数据库中的表的通用操作。   ORM思想(数据库中的表和Java中的类对应)

	public class DAO<T> {//表的共性操作的DAO
	
	    //添加一条记录
	    public void add(T t){
	
	    }
	
	    //删除一条记录
	    public boolean remove(int index){
	
	        return false;
	    }
	
	    //修改一条记录
	    public void update(int index,T t){
	
	    }
	
	    //查询一条记录
	    public T getIndex(int index){
	
	        return null;
	    }
	
	    //查询多条记录
	    public List<T> getForList(int index){
	
	        return null;
	    }
	
	    //泛型方法
	    //举例：获取表中一共有多少条记录？获取最大的员工入职时间？
	    public <E> E getValue(){
	
	        return null;
	    }
	
	}

### (2)CustomerDAO.java

	public class CustomerDAO extends DAO<Customer>{//只能操作某一个表的DAO
	}

### (3)StudentDAO.java

	public class StudentDAO extends DAO<Student> {//只能操作某一个表的DAO
	}


# 四、泛型在继承上的体现

**泛型在继承方面的体现**

	虽然类A是类B的父类，但是G<A> 和G<B>二者不具备子父类关系，二者是并列关系。
	
	补充：类A是类B的父类，A<G> 是 B<G> 的父类
	
	     
	    @Test
	    public void test1(){
	
	        Object obj = null;
	        String str = null;
	        obj = str;
	
	        Object[] arr1 = null;
	        String[] arr2 = null;
	        arr1 = arr2;
	        //编译不通过
	//        Date date = new Date();
	//        str = date;
	        List<Object> list1 = null;
	        List<String> list2 = new ArrayList<String>();
	        //此时的list1和list2的类型不具子父类关系
	        //编译不通过
	//        list1 = list2;
	        /*
	        反证法：
	        假设list1 = list2;
	           list1.add(123);导致混入非String的数据。出错。
	
	         */
	
	        show(list1);
	        show1(list2);
	
	    }
	
	
	
	    public void show1(List<String> list){
	
	    }
	
	    public void show(List<Object> list){
	
	    }
	
	    @Test
	    public void test2(){
	
	        AbstractList<String> list1 = null;
	        List<String> list2 = null;
	        ArrayList<String> list3 = null;
	
	        list1 = list3;
	        list2 = list3;
	
	        List<String> list4 = new ArrayList<>();
	
	    }

# 五、通配符

## 1.通配符的使用

	通配符：?
	
	类A是类B的父类，G<A>和G<B>是没关系的，二者共同的父类是：G<?

	

	    @Test
	    public void test3(){
	        List<Object> list1 = null;
	        List<String> list2 = null;
	
	        List<?> list = null;
	
	        list = list1;
	        list = list2;
	        //编译通过
	//        print(list1);
	//        print(list2);
	
	
	        //
	        List<String> list3 = new ArrayList<>();
	        list3.add("AA");
	        list3.add("BB");
	        list3.add("CC");
	        list = list3;
	        //添加(写入)：对于List<?>就不能向其内部添加数据。
	        //除了添加null之外。
	//        list.add("DD");
	//        list.add('?');
	
	        list.add(null);
	
	        //获取(读取)：允许读取数据，读取的数据类型为Object。
	        Object o = list.get(0);
	        System.out.println(o);
	
	
	    }
	
	    public void print(List<?> list){
	        Iterator<?> iterator = list.iterator();
	        while(iterator.hasNext()){
	            Object obj = iterator.next();
	            System.out.println(obj);
	        }
	    }

## 2.涉及通配符的集合的数据的写入和读取

见上	

## 3.有限制条件的通配符的使用

	    限制条件的通配符的使用。
	        ? extends A:
	                G<? extends A> 可以作为G<A>和G<B>的父类，其中B是A的子类
	
	        ? super A:
	                G<? super A> 可以作为G<A>和G<B>的父类，其中B是A的父类
	
	     */
	    @Test
	    public void test4(){
	
	        List<? extends Person> list1 = null;
	        List<? super Person> list2 = null;
	
	        List<Student> list3 = new ArrayList<Student>();
	        List<Person> list4 = new ArrayList<Person>();
	        List<Object> list5 = new ArrayList<Object>();
	
	        list1 = list3;
	        list1 = list4;
	//        list1 = list5;
	
	//        list2 = list3;
	        list2 = list4;
	        list2 = list5;
	
	        //读取数据：
	        list1 = list3;
	        Person p = list1.get(0);
	        //编译不通过
	        //Student s = list1.get(0);
	
	        list2 = list4;
	        Object obj = list2.get(0);
	        ////编译不通过
	//        Person obj = list2.get(0);
	
	        //写入数据：
	        //编译不通过
	//        list1.add(new Student());
	
	        //编译通过
	        list2.add(new Person());
	        list2.add(new Student());
	
	    }	 

