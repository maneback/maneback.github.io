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

`ArrayList `基于动态数组实现，支持随机访问。`LinkedList`：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插⼊和删除元素。

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

### Map

- `HashMap` 的 `key`必须实现`equals()` 和 `hashCode()` 方法。
- 有序 map 必须实现`Comparable` 接口；或者在声明时传入一个`Comparator` 接口，里面声明`compare` 函数
- 无序的 `HashMap`基于哈希表实现， 有序的  `TreeMap`基于红⿊树实现 。

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
- 有序的Set还要实现`Comparable` 接口； 或者在声明时传入一个`Comparator` 接口，里面声明`compare` 函数
- `HashSet `基于哈希表实现， `TreeSet `基于红黑树实现。

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
- PriorityQueue：基于堆结构实现

```java
// 实现类与接口分离
Queue<Integer> que = new PriorityQueue<Integer>();

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

- deque 是用 `LinkedList `实现的接口，赋予了不同的功能。

```java
Deque<Integer> que = new LinkedList<Integer>();

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

- stack 是用`Deque`接口来模拟的，原来的stack被不推荐使用。

```java
Deque<Character> stack = new LinkedList<>();
stack.isEmpty();
stack.pop();
stack.push(ch);
stack.peek()
```

### 并发容器

####  ConcurrentHashMap

`ConcurrentHashMap ` 是线程安全的 Map。

`ConcurrentHashMap `采⽤锁分段技术，将整个 `Hash `数组分成几个小的 segment，在每个 `segment `分别上锁。在插⼊元素的时候就需要先找到应该插⼊到哪⼀个⽚`segment`，然后对该 `segment `上锁并操作。这样做明显减小了锁的粒度。

`HashTable` 同样是线程安全的，然而其使用了`synchronized` 关键字对 `get` 和`put` 等操作进行加锁，也就是每一次操作都会锁住整个 `Hash `表，每个线程独占哈希表，效率低下。

#### CopyOnWriteArrayList

线程安全的 List。所有可变操作（`add`，`set `等等）都是通过创建底层数组的新副本来实现的。当需要修改的时并不直接修改原有内容，⽽是对原有数据进⾏⼀次复制，将修改的内容写⼊副本。写完之后，再将修改完的副本替换原来的数据，这样就可以保证写操作不会影响读操作了。读取操作没有任何同步控制和锁操作，因此可以保证数据安全。

此外，写⼊操作在添加集合的时候加了锁，保证了同步，避免了多线程写的时候会复制出多个副本。

#### BlockingQueue

提供了可阻塞的插入和移除方法。当容器队列已满时，插入线程会被阻塞，直到队列有新的空余位置产生；当队列为空时，移除线程会被阻塞，直到队列中有新的元素。

#### ConcurrentLinkedQueue

高效的并发队列，基于链表实现。是一个非阻塞队列

### 继承关系

橙色为类，蓝色为接口。

`List`， `Queue`， `Set `类

<img src="https://gitee.com/MyTypora/typorapic/raw/master/img/20210824100823.png" alt="image-20210824100823589" style="zoom:80%;" />

--------



`Map` 类

<img src="https://gitee.com/MyTypora/typorapic/raw/master/img/20210816164347.png" alt="image-20210816164347532" style="zoom:80%;" />

**参考链接**

- [菜鸟教程](https://www.runoob.com/java/java-tutorial.html)