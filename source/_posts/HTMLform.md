---
title: HTML表单
date: 2018-11-21 19:52:54
tags:
- HTML
categories:
- knowledge
description: 表单操作
---


# HTML表单
HTML表单用于收集不同类型的用户输入
<form\>元素定义HTML表单，表单中包含多个不同类型的表单元素，input、复选框、单选按钮、提交按钮等等。

如下是一个简易的表单示例：
```HTML
<form>
  First name:<br>
  <input type="text" name="firstname">
  Last name:<br>
  <input type="text" name="lastname">
</form>
```
## 表单属性
### action属性
action属性定义了在提交表单时执行的动作，向服务器提交表单的通常做法是使用提交按钮，通常，表单会被提交到web服务器上的代码处理，如：
```html
<form action="action_page.php">
```
注：如果忽略action属性，则action属性会被设置成当前页面。
### method属性
method属性规定在提交表单时使用的HTTP方法，GET或者POST，默认设置为GET。
```html
<form action="action_page.php" method="get"></form>
<form action="action_page.php" method="post">
```
要分清get和post的使用条件。
### 其他属性
```HTML
<form accept-charset="" action="" autocomplete="" enctype="" method="" name="" novalidate="" target="">
```
target规定action属性中地址的目标（默认为_self）， 可选_blank, \_parent, \_self, \_top。

# HTML表单元素

## <input\> 元素
input是最重要的表单元素，它可以根据type属性的赋值具有多种形态。
## <select\>元素
定义下拉列表
```HTML
<select name="cars">
  <option value="audi">Audi</option>
  <option value="benze" >Benze</option>
  <option value="BMW">BMW</option>
  <option value="Porsche" selected>Porsche</option>
</select>
```
## <textarea\>元素



每个输入字段必须设置一个name属性
