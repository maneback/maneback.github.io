---
title: Java 集合
date: 2021-08-16 10:59:10
tags:
  - Java
categories: 基础知识
description: Java 集合的使用方式
---

集合提供了一个对象来在内部容纳若干其他的 Java 对象，并提供相应的访问接口。Java 的`java.util` 包主要提供了一下三种不同类型的集合：

- `List` 有序列表的集合，可按插入顺序访问。，
- `Set` 确保不可以插入重复元素的集合。
- `Map` 通过 `key-value` 快速查找元素的集合。

Java的集合设计有两个特点：一是支持泛型，可以限制插入到集合中元素的数据类型；二是实现了接口与类的分离，以上三种均为接口而非具体的实现类。并且可以通过统一的迭代器方式进行访问。

**我们尽量以接口而非具体的实现类来访问元素。**

**重要** 以下遗留类不应该被继续使用：

`Hashtable`, `Vector`, `Stack`, `Enumeration<E>`.

### List

需要实现`equals` 方法。

```java
List<Integer> array = new ArrayList<Integer>();
List<Integer> array2 = new LinkedList<Integer>();
List<Integer> array = List.of(1,2,3);
array.add(1);
array.add(1, 2); //(index, elem)
array.remove(1);//(elem)
int elem = array.get(1)//(index)
int n = array.size();
```

### map

- `HashMap` 的 `key`必须实现`equals()` 和 `hashCode()` 方法。
- 有序 map 必须实现`Comparable` 接口；或者在声明时传入一个`Comparator` 接口，里面声明`compare` 函数，

```java
//无序map
Map<String, Integer> mapper = new HashMap<>();
mapper.put(')', '(');
mapper.containsKey(ch);
mapper.get(key);
//遍历
for (String key : mapper.keySet());
for (Map.Entry<String, Integer> entry : mapper.entrySet());
for(String value: mapper.valueSet());
//有序map
Map<String, Integer> order_map = new TreeMap<>();

//基础的使用map计数出现次数
Map<Integer, Integer> mapper1 = new HashMap<Integer, Integer>();
        for(int num: arr){
            mapper1.put(num, occur.getOrDefault(num, 0) + 1);
        }
```

### Set 

- 只会存储 `key` 的值。

- 需要正确实现`equals()`和`hashCode()`方法。
- 有序的Set还要实现`Comparable` 接口； 或者在声明时传入一个`Comparator` 接口，里面声明`compare` 函数，

```java
//无序Set
Set<String> set = new HashSet<>();
set.add("123");
set.contains("123");
set.remove("123");
int n = set.size();
//有序

Set<String> set = new TreeSet<>();

```



### Queue

- 实现类为 `LinkedList`

```java
Queue<int> que = new LinkedList<int>();
int n = que.size();
//throw exception
que.add();
que.remove();
que.element();

//return false or null
que.offer();
que.poll();
que.peel();
```

注意有两套方法，对应不同的情况。

### PriorityQueue

- 放入的元素需要实现`Comparable`，或者提供一个`Comparator`对象来判断两个元素的顺序。

```java
// 实现类与接口分离
Queue<int> que = new PriorityQueue<int>();

//throw exception
que.add();
que.remove();
que.element();

//return false or null
que.offer();
que.poll();
que.peek();
```

### Deque

- deque 是用 LinkedList 实现的接口，赋予了不同的功能。

```java
Deque<int> que = new LinkedList<int>();

//throw exception
que.addLast(); //addFirst
que.removeLast(); //removeFirst
que.getLast(); // getFirst

//return false or null
que.offerLast(); //offerFirst
que.pollLast(); //pollLast
que.peekLast(); //peekLast
```

### Stack

- stack 是用Deque接口来模拟的，原来的stack被占用。

```java
Deque<Character> stack = new LinkedList<>();
stack.isEmpty();
stack.pop();
stack.push(ch);
stack.peek()
```



#### 继承关系

橙色为类，蓝色为接口。

`List`， `Queue`， `Set `类

<img src="https://gitee.com/MyTypora/typorapic/raw/master/img/20210816132257.png" alt="image-20210816132257321" style="zoom:80%;" />

--------



`Map` 类

<img src="https://gitee.com/MyTypora/typorapic/raw/master/img/20210816164347.png" alt="image-20210816164347532" style="zoom:80%;" />

**参考链接**

- [菜鸟教程](https://www.runoob.com/java/java-tutorial.html)