---
title: python异常处理
date: 2019-05-04 10:59:22
tags:
- python
categories: knowledge
description: python3的异常处理
---

###  异常处理

python中使用`try ... except`来处理异常

try 语句的工作方式如下：

- 首先执行`try`子句中的语句。
- 若没有异常发送，则忽略`except`子句中的语句，程序正常运行。
- 若在`try`中发生了异常，那么try中的剩余部分将会忽略。如果发生的异常类型与某个`except`中指明的相符合，那么执行对应`except`中的语句
- 如果异常没有与任何的`except`匹配，那么传递给上层的`try`中。

一个`try`语句可以包含多个`except`, 但是至多只有一个会被执行。

一个`except`子句可以同时处理多个异常，这些异常放在括号里组成一个元组，例如：

```python
except (RuntimeError, TypeError, NameError):
    pass
```

最后的一个`except`可以忽略异常的名称，当做通配符使用，可以使用这种方法打印一个错误信息，然后再次把异常抛出。

```python
import sys

try:
    f = open('my.txt')
    l = f.readline()
    i = int(l.strip())
except OSError as e:
    print("OS error {}".format(e))
except ValueError:
    print("Could not convert data to an integer.")
except:
    print("Unexcepted error: ", sys.exc_info()[0])
    raise
```

`try except` 语句还有一个可选的`else`语句，放在所有的`except`之后。这个子句将在`try`没有发生任何异常的情况下执行。

```python
for arg in sys.argv[1:]:
    try:
        f = open(arg, 'r')
    except IOError:
        print('cannot open this file:', arg)
    else:
        print('{} has {} lines'.format(arg, len(f.readlines())))
        f.close()
```

使用`else`语句比把所有的语句放在`try`子句里面要好，这样可以避免一些意想不到的、而且`except`子句没有捕获的异常。

### 抛出异常

你可以使用`raise`语句来抛出一个指定的异常。即运行到这句话的时候，就会发生异常。

raise的参数必须是一个异常的实例或者是异常的类。

```python
try:
    raise NameError('Hello World')
except NameError:
    print('An excepton occurred!')
    raise
    
```

### 用户自定义异常类

用户自定义的异常，继承自`Exception`类。

```python
class MyError(Exception):
    def __init__(self, value):
        self.value = value
    def __str__(self):
        return repr(self.value)
try: 
    raise MyError(4*4)
except MyError as e: # 这是一个异常的实例，用于访问异常的属性
    print('My exception occurred, value: ', e.value)
    
```

### 定义清理行为

`try`语句中还有一个可选的`finally`语句，定义了无论在是否发生异常的情况下都会执行的清理行为。

无论`try`语句中是否有异常，`finally`子句都会执行。

如果一个异常在`try`子句中被抛出，但是没有任何的`except`把它截住，那么这个异常将会在`finally`语句执行后被抛出。

```python 
def divide(x, y):
    try:
        result = x/y
    except ZeroDivisionError:
        print('dividion by zero')
    else:
        print('result is ', result)
    finally:
        print("executing finally clause")
        
[in 1] divide(2, 1)
[out 1]result is 2.0
executing finally clause

[in 2] divide(2, 0)
[out 2]division by zero!
executing finally clause

[in 3]divide("2", "1")
[out 3]executing finally clause
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
  File "<stdin>", line 3, in divide
TypeError: unsupported operand type(s) for /: 'str' and 'str'
```

