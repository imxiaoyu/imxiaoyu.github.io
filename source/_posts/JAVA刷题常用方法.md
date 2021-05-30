---
title: 'JAVA刷题常用方法'
date: 2021/5/30 13:45:04 
tags:
	- JAVA
	- Leetcode
---
在LeetCode上刷题自己常用的java类方法，总结了一下：


<!-- more -->
# 一、String
## 1.方法
**常用方法：**

返回类型|用法|作用
:-:|:-:|:-:
int|length()|返回字符串的长度
char|charAt(int)|返回某索引处的字符
boolean|isEmpty()|判断是否是空字符串
String|toLowerCase()|使用默认语言环境，将 String 中的所字符转换为小写
String|toUpperCase()|使用默认语言环境，将 String 中的所字符转换为大写
String|trim()|返回字符串的副本，忽略前导空白和尾部空白
boolean|equals(Object)|比较字符串的内容是否相同
boolean|equalsIgnoreCase(String)|与equals方法类似，忽略大小写
String|concat(String)|将指定字符串连接到此字符串的结尾。 等价于用“+”
int|compareTo(String)|比较两个字符串的大小
String|substring(int beginIndex)|返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。
String|substring(int beginIndex, int endIndex)|返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(**不包含**)的一个子字符串。
boolean|endsWith(String suffix)|测试此字符串是否以指定的后缀结束
boolean|startsWith(String prefix)|测试此字符串是否以指定的前缀开始
boolean|startsWith(String prefix, int toffset)|测试此字符串从指定索引开始的子字符串是否以指定前缀开始
boolean|contains(CharSequence s)|当且仅当此字符串包含指定的 char 值序列时，返回 true
int|indexOf(String str)|返回指定子字符串在此字符串中第一次出现处的索引,未找到返回-1
int|indexOf(String str, int fromIndex)|返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始,未找到返回-1
int|lastIndexOf(String str)|返回指定子字符串在此字符串中最右边出现处的索引,未找到返回-1
int|lastIndexOf(String str, int fromIndex)|返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索,未找到返回-1


**替换：**

返回类型|用法|作用
:-:|:-:|:-:
String|replace(char oldChar, char newChar)|返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所 oldChar 得到的。
String|replace(CharSequence target, CharSequence replacement)|使用指定的字面值替换序列替换此字符串所匹配字面值目标序列的子字符串。
String|replaceAll(String regex, String replacement)|使用给定的replacement 替换此字符串所匹配给定的正则表达式的子字符串。
String|replaceFirst(String regex, String replacement)|使用给定的replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。

**匹配:**

返回类型|用法|作用
:-:|:-:|:-:
boolean|matches(String regex)|告知此字符串是否匹配给定的正则表达式。


**切片：**

返回类型|用法|作用
:-:|:-:|:-:
String[]|split(String regex)|根据给定正则表达式的匹配拆分此字符串。
String[]|split(String regex, int limit)|根据匹配给定的正则表达式来拆分此字符>串，最多不超过limit个，如果超过了，剩下的全部都放到最后一个元素中。

## 2.与其他类型转换
### (1)与基本数据类型、包装类之间的转换

>1.String --> 基本数据类型、包装类：调用包装类的静态方法 **parseXxx(str)**
>- int num = Integer.parseInt(str);
>
>2.基本数据类型、包装类 --> String:调用String重载的 **valueOf(xxx)**
>- String str = str.valueOf(num);



### (2)与字符数组之间的转换

>1.String --> char[]:调用String的**toCharArray()**
>- char[] charArray = str.toCharArray();
>
>2.char[] --> String:调用String的构造器
>- char[] arr = new char[]{'h','e','l','l','o'};
>- String str = new String(arr);


### (3)与StringBuffer、StringBuilder之间的转换

>1.String --> StringBuffer、StringBuilder:调用StringBuffer、StringBuilder构造器
>- StringBuffer sb = new StringBuffer(String str);
>
>2.StringBuffer、StringBuilder --> String:
>①调用**String构造器**；
>- String str = new String(StringBuffer sb);
>
>②StringBuffer、StringBuilder的**toString()**
>- String str = sb.toString();


# 二、StringBuffer
StringBuffer sb = new StringBuffer(String str);
## 1.方法
>1.增：append(xxx)
>2.删：delete(int start,int end)
>3.改：setCharAt(int n, char ch) / replace(int start, int end, String str)
>4.查：charAt(int n )
>5.插：insert(int offset, xxx)
>6.长度：length();
>7.遍历：for() + charAt() / toString()


# 三、ArrayList(可以重复),Set（不能重复）
List<Object> arrayList = new ArrayList<>();
Set<Object> set = new HashSet<>();
## 1.方法
>1.增：add(Object obj)
>2.删：remove(int index) / remove(Object obj)
>3.改：set(int index, Object ele)
>4.查：get(int index)
>5.插：add(int index, Object ele)
>6.长度：size()
>7.清空：clear();
>8.是否包含：contains(Object obj)
>9.是否为空：isEmpty()
>10.遍历：
>- ① Iterator迭代器方式
>- ② 增强for循环
>- ③ 普通的循环


	add(Object obj),
	addAll(Collection coll),
	size(),
	isEmpty(),
	clear();

	contains(Object obj),
	containsAll(Collection coll),
	remove(Object obj),
	removeAll(Collection coll),
	retainsAll(Collection coll),
	equals(Object obj);

	hasCode(),
	toArray(),
	iterator();


# 四、HashMap:底层<Set,List>
## 1.方法
>1.添加：put(Object key,Object value)
>2.删除：remove(Object key)
>3.修改：put(Object key,Object value)
>4.查询：get(Object key)
>5.长度：size()
>6.遍历：keySet() / values() / entrySet()
>7.containsKey(Object key) 包含返回true，否则false
>8.getOrDefault(Object key, int 0) 得到key对应的value，如果不存在返回自己设定的默认值

## 2.遍历
### (1)通过键值对entrySet()

	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	//键值对entrySet
	for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
		System.out.println("Key = " + entry.getKey() + ", Value = " + entry.getValue());
	}

### (2)通过keySet()和values()
		
	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	//迭代key,
	for (Integer key : map.keySet()) {
		System.out.println("Key = " + key);
	}
 
	//迭代value
	for (Integer value : map.values()) {
		System.out.println("Value = " + value);
	}
### (3)使用带泛型的迭代器进行遍历

	Map<Integer, Integer> map = new HashMap<Integer, Integer>();

	Iterator<Map.Entry<Integer, Integer>> entries = map.entrySet().iterator();
	while (entries.hasNext()) {
		Map.Entry<Integer, Integer> entry = entries.next();
		System.out.println("Key = " + entry.getKey() + ", Value = " + entry.getValue());
	}
### (4)使用不带泛型的迭代器进行遍历

	Map map = new HashMap();

	Iterator<Map.Entry> entries = map.entrySet().iterator();
	while (entries.hasNext()) {
		Map.Entry entry = (Map.Entry) entries.next();
		Integer key = (Integer) entry.getKey();
		Integer value = (Integer) entry.getValue();
		System.out.println("Key = " + key + ", Value = " + value);
	}

### (5)通过Java8 Lambda表达式遍历

	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
		
	map.forEach((k, v) -> System.out.println("key: " + k + " value:" + v));

# 五、PriorityQueue
## 1.方法
返回类型|用法|作用
:-:|:-:|:-:
boolean|**add(E e)**|将指定的元素插入此优先级队列。
void|clear()|从此优先级队列中移除所有元素。
Comparator<? super E>|**comparator()**|返回用来对此队列中的元素进行排序的比较器；如果此队列根据其元素的自然顺序进行排序，则返回 null。
boolean|**contains(Object o)**|如果此队列包含指定的元素，则返回 true。
Iterator<E>|iterator()|返回在此队列中的元素上进行迭代的迭代器。
boolean|offer(E e)|将指定的元素插入此优先级队列。
E|**peek()**|获取但**不移除**此队列的头；如果此队列为空，则返回 null。
E|**poll()**|获取并**移除**此队列的头，如果此队列为空，则返回 null。
boolean|**remove(Object o)**|从此队列中移除指定元素的单个实例（如果存在）。
int|**size()**|返回此 collection 中的元素数。
Object[]|toArray()|返回一个包含此队列所有元素的数组。
<T> T[]|toArray(T[] a)|返回一个包含此队列所有元素的数组；返回数组的运行时类型是指定数组的类型。

从类 java.util.AbstractQueue 继承的方法：
addAll, element, remove


从类 java.util.AbstractCollection 继承的方法：
containsAll, isEmpty, removeAll, retainAll, toString


从类 java.lang.Object 继承的方法：
clone, equals, finalize, getClass, hashCode, notify, notifyAll, wait, wait, wait
 

从接口 java.util.Collection 继承的方法：
containsAll, equals, hashCode, isEmpty, removeAll, retainAll
## 2.用法

	PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>(); //小顶堆，默认容量为11
	PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(new Comparator<Integer>(){ //大顶堆，容量11
	    @Override
	    public int compare(Integer i1,Integer i2){
	        return i2-i1;
	    }
	});