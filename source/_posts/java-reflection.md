---
title: Java 反射机制
date: 2021-08-16 11:25:11
tags:
  - Java
categories: 基础知识
description: 介绍Java的反射知识
---

### 为什么需要反射机制

Java反射机制是：在**运行状态**中，对于任意一个类，都能够知道这个类的所有**属性和方法**；对于任意一个对象，都能够**调用它的任意一个方法**。这种动态获取的信息以及动态调用对象的方法的功能称为反射机制。

所以如果能够在运行时拿到Class对象，就可以生成java对象并进行调用。

反射赋予了jvm动态编译的能力。动态编译可以最大限度的体现Java的灵活性（多态）。否则类的元信息只能通过静态编译的形式实现（在编译期确定类型，绑定对象），而不能实现动态编译（在运行期确定类型，绑定对象）。也就是说在编译以后，程序在运行时的行为就是固定的了，如果要在运行时改变程序的行为，就需要动态编译，在Java中就需要反射机制。

- 情景一： 有的类无法使用 new 一个对象的方式来实例化。
- 情景二：有的类可以在用到时再动态加载到jvm中，这样可以减少jvm的启动时间，同时更重要的是可以动态的加载需要的对象（多态）。
- 情景三：避免将程序写死到代码中。

反射的缺点：

- 性能开销：由于反射涉及动态解析的类型，因此无法执行某些 Java 虚拟机优化。
- 破坏封装性： 反射调用方法时可以忽略权限检查，因此可能会破坏封装性而导致安全问题。
- 内部曝光： 由于反射允许代码执行在非反射代码中非法的操作，例如访问私有字段和方法，所以反射的使用可能会导致意想不到的副作用，这可能会导致代码功能失常并可能破坏可移植性。

### 代码实现

首先构造一个类 Person 来实现反射，这个类是我们要通过反射来调用的类

```java
package com.gao.reflect;

public class Person {
    private String name;
    int age;
    public String address;

    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
    
    public  Person(){
    }
    
    private Person(String name){
        this.name = name;
    }

    public Person(String name, int age, String address){
        this.name = name;
        this.age = age;
        this.address = address;
    }
    
    public void func1(){
        System.out.println("public func1");
    }
    
    private void func2(int x){
        System.out.println("private fun2");
    }
    
    protected String func3(int x, String y){
        System.out.println("protected func3");
        return y;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                '}';
    }
}

```



#### 1. 获取class 对象

应用反射的第一步是要拿到该类的静态属性`class` 然后通过拿到类的方法和属性。

共有三种方法获取：通过实例的方法、通过类的静态属性、通过字节码

```java
public class MyReflection {
    public static void main(String[] args) {
        Person person = new Person();
        
        Class<? extends Person> c_by_obj = person.getClass(); //方法1
        Class<Person> c_by_class = Person.class; // 方法2
        Class<?> c_by_name = Class.forName("com.gao.reflect.Person");//方法3
        System.out.println(c_by_class==c_by_name);
    }
}
```

#### 2. 获取构造函数并创建对象

之后我们都将通过字节码文件，来获取 class 对象。

可以由两种划分：

- 私有构造函数/公有构造函数
- 单个构造函数/构造函数集合

在获取单个带参构造函数的时候，需要指定参数个数和数据类型对应的字节码文件对象（Class）。

对于私有方法还可以通过`.setAccessible(true)` 来访问。

通过反射调用构造函数，是把实例对象和参数交给**函数对象**的`invoke` 来做

```java

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

//通过Class实例获取class信息的方法称为反射（Reflection）。
public class MyReflection02 {
    public static void main(String[] args) {
        //获取构造方法
        Class<?> cls = Class.forName("com.gao.pojo.reflect.Person");
        //返回包含构造对象数组（公共的）
        Constructor<?>[] cons =  cls.getConstructors();
        //返回包含构造对象数组（所有的）
        Constructor<?>[] cons_all =  cls.getDeclaredConstructors();
        System.out.println(cons_all);

        //返回指定的公共构造函数
        // 参数：表示要获取的构造方法的参数个数和数据类型对应的字节码文件对象（Class）
        Constructor<?> con1 = cls.getConstructor(); // 无参构造函数
        //创建对象
        Object p = (Person) con1.newInstance(); //Person p = new Person();


        //获取 带参构造方法    private Person(String name, int age, String address)
        Constructor<?> con2 = cls.getDeclaredConstructor(String.class, int.class, String.class);
        Object obj =  con2.newInstance("name", 30, "xian");
        System.out.println(obj);

        //私有构造方法不能被在外部访问
        //暴力反射：

        Constructor<?> con3 = cls.getDeclaredConstructor(String.class);
        con3.setAccessible(true);  // 暴力反射访问私有构造方法
        Object ob = con3.newInstance("name");
    }
}

```

#### 3. 获取属性并赋值

属性可以直接通过名字直接访问。

通过反射给属性赋值，是把对象和属性值交给 属性域的 `set`。

```java
mport java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;

//通过Class实例获取class信息的方法称为反射（Reflection）。
public class MyReflection03 {
    public static void main(String[] args) {
        //获取类对象
        Class<?> cls = Class.forName("com.gao.pojo.reflect.Person");

        Field[] fields = cls.getFields();
        Field[] all_fields = cls.getDeclaredFields();

        //单个的属性
        Field address_field = cls.getField("address");

        Constructor<?> con = cls.getConstructor();
        Object obj =  con.newInstance();


        //给成员变量的属性赋值
        address_field.set(obj, "xian");
        System.out.println(obj);
    }
}
```

#### 4. 访问成员方法

同构造方法一样，也是通过 `invoke` 的函数来调用反射得到的成员方法。

对于有返回值的方法，`invoke` 会返回函数调用的返回值。

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

//通过Class实例获取class信息的方法称为反射（Reflection）。
public class MyReflection04 {
    public static void main(String[] args) {
        //获取类对象
        Class<?> cls = Class.forName("com.gao.pojo.reflect.Person");
        Constructor<?> con = cls.getConstructor();
        Object obj = con.newInstance();

        Method[] methods = cls.getMethods(); // 所有公共方法和继承的方法
        Method[] methods1 = cls.getDeclaredMethods(); // 私有方法（不包括继承的方法）

        // 无参数的方法

        Method func1 = cls.getMethod("func1");
        func1.invoke(obj);

        //有参的方法
        Method fun2 = cls.getDeclaredMethod("func2", int.class);
        fun2.setAccessible(true);
        fun2.invoke(obj, 3);

        //带返回
        //参数 是参数类型
        Method fun3 = cls.getMethod("func3", int.class, String.class);
        fun3.setAccessible(true);
        Object o = fun3.invoke(obj, 30, "tianjin");
        System.out.println(o);
    }
}

```

#### 5. 越过泛型检查

```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.ArrayList;

//通过Class实例获取class信息的方法称为反射（Reflection）。
public class MyReflection05 {
    public static void main(String[] args) {

        //越过泛型检查
        ArrayList<Integer> list = new ArrayList<>();
        Class cls = list.getClass();
        Method mthd = cls.getMethod("add", Object.class);
        mthd.invoke(list, "world");
        System.out.println(list );
    }
}
```



**参考链接**：

[为什么需要Java反射](https://blog.csdn.net/tongdanping/article/details/103252352)