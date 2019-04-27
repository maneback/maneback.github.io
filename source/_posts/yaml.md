---
title: yaml
date: 2018-11-10 23:11:35
tags:
categories: knowledge
---

YAML Ain't a Markup Language.

<!--more-->

# 0. 简介

Yaml是用来编写配置文件的，对比同样是用来编写配置文件的JSON，Yaml非常简洁和强大，也远比JSON方便。

YAML的定义即为“YAML Ain't a Markup Language.”的递归缩写。YAML是以数据为设计语言的重点的，而不是像XML以标记为重点。也正是这种设计理念，使它成为了一种可读性高，易于表达数据序列的编程语言。



# 1. 语法规则

YAML的语法规则非常简单，只要记住以下四条：

> - YAML是大小写敏感的
> - YAML使用缩进来表示层级关系
> - YAML只允许使用空格缩进，禁止使用Tab键
> - 缩进的空格数目没有限制，只要保持相同层级的元素对齐即可。

YAML支持的数据结构有以下四种。

> - 对象： 即一个键值对的集合，用冒号表示
> - 数组： 一组按次序排列的值
> - 纯量： 单个的、不可再分的值，即String， Int 等类型的数值。

**还要注意的一点是，冒号后面一定要加一个空格。**

YAML的数据类型非常灵活，如： 键值对的值既可以是 **纯量** 类型，又可以是 **数组类型** ，甚至是 **对象**； 同样，数组中的每一个元素也都可以是 **纯量** 或者 **对象**

下面将分别介绍着三种数据结构。

# 2. 对象

对象是一组键值对，使用冒号来表示

注意冒号后面一定要加空格。

```yaml
name: jason
```



# 3. 数组

数组由`-` 标识，一系列元素构成一个数组

```yaml
- yellow
- blue
- green
```

# 4. 复合结构

正如我前面所说的，数组的每一个元素都可以是一个键值对，对象的value 也可以是一个数组

```yaml
color:
  - yellow
  - blue
  - green
website:
  baidu: baidu.com
  google: google.com
  apple: apple.com
```

它表示的数据结构：

```python
{
    color: [yellow, blue, green],
    website:{
        baidu: baidu.com,
        google: google.com,
        apple: apple.com
    }
}
```

# 5. 纯量

纯量是最基本的。不可再分的值。以下数据类型都属于纯量

```
- 字符串
- 布尔值
- 整数
- 浮点数
- Null
- 时间
- 日期

```

# 6. 字符串

字符串是最常见的一种数据类型。

字符串默认不使用引号表示。

```yaml
str: this_is_a_string
```

如果字符串之中包含了空格或者特殊字符，就需要引号

```yaml
str: 'he says: no'
```

单引号和双引号都可以使用，但是 **双引号不会对特殊字符转义**。

如果单引号之中还有单引号，必须使用两个单引号转义

```yaml
s1: 'this is a\n string'         # 'this is a\\n string"
s2: "this is a\n string"         # "this is a\n string"
s3: "labor''s day"               # labor\'s day
```

字符串还可以写成多行，从第二行开始， **必须有一个单空格缩进**， 换行符将会被转成空格

```yaml
str: this 
 is
 a string
 # this is a string
```

多行字符串还可以使用`|` 保留换行， 也可以使用`>` 折叠换行

```yaml
this : |
  FOO
  BAR
that: >
  FOO
  BAR
#  {this: FOO\nBAR}
#  {that: FOOBAR}
```

