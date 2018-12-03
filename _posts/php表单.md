---
title: php表单
date: 2018-01-20 00:39:38
categories:
- 教程
tags:
  - php
---

上一篇博客实现了一个前端的登陆页面，提供了填写用户名和密码的地方，

遗憾的是点击提交后所填写的用户名和密码都无处可寻，

这次写一个php文件，拿出用户提交的数据。



### 这是上次写的前端登陆页面

form1.html
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>simpleForm</title>
    </head>
    <body>
        <form>
            用户名:<input type="text" name="name">
            密码:<input type="text" name="password">
            <input type="submit" value="提交">
        </form>
    </body>
</html>
```
<!-- more -->
首先在同级目录下创建一个form_action.php文件， 在其中加上如下内容
```
<?php
echo "Here you are! Now you are in form_action.php";
?>
```
[echo, print, var_dump()的用法](http://www.runoob.com/php/php-echo-print.html)

<!--more-->

在form1.html里面给form标签加上action属性
```
<form action="form_action.php">
```

保存后再次点击“提交”，发现屏幕上显示了一段你让它显示的话。

这就说明，**action属性是用来指明用户点了提交后，将要执行什么操作**

### 两个超全局变量

上面的例子只是简单的显示了一段话，那么怎么把用户提交的数据提取出来呢？很简单，php有两个[超全局变量](http://www.w3school.com.cn/php/php_superglobals.asp)分别叫$_GET和$_POST，那么又不得不提form的另一个属性method.

上文讲到method是用来规定用于发送 form-data 的 HTTP 方法的属性,可以自己试着将刚才的例子里分别加上method="post"或者method="get",中间别忘了空格。

发现两者的不同在于，get方法的url很长，post方法url不变。就是因为GET是通过URL参数传递到当前脚本的变量数组。POST是通过HTTP POST传递到本当前脚的变量数组。

所以如果使用get方法那么超全局变量就是$_GET，post方法超全局变量就是$_POST,那么我们可以在form_action.php里面写入如下代码提取出用户提交的数据，取决于form的action是何属性，默认是get

```
// 使用get方法
你好，<?php echo $_GET['name']; ?>。
你的密码是 <?php echo $_GET['password']; ?>。

// 使用post方法
你好，<?php echo $_POST['name']; ?>。
你的密码是 <?php echo $_POST['password']; ?> 。
```
这样就能做到将前端的数据传到后端。大多数用php写的网站也都是这样获取用户信息的，最多加一些其他的操作，原理都是一样的。


### GET vs. POST

* GET 和 POST 都创建数组（例如，array( key => value, key2 => value2, key3 => value3, ...)）。此数组包含键/值对，其中的键是表单控件的名称，而值是来自用户的输入数据。
* GET 和 POST 被视作 $_GET 和 $_POST。它们是超全局变量，这意味着对它们的访问无需考虑作用域 - 无需任何特殊代码，您能够从任何函数、类或文件访问它们。
* $_GET 是通过 URL 参数传递到当前脚本的变量数组。
* $_POST 是通过 HTTP POST 传递到当前脚本的变量数组。

### 何时使用 GET？

* 通过 GET 方法从表单发送的信息对任何人都是可见的（所有变量名和值都显示在 URL 中）。GET 对所发送信息的数量也有限制。限制在大于 2000 个字符。不过，由于变量显示在 URL 中，把页面添加到书签中也更为方便。
* GET 可用于发送非敏感的数据。
* 绝不能使用 GET 来发送密码或其他敏感信息！

### 何时使用 POST？

* 通过 POST 方法从表单发送的信息对其他人是不可见的（所有名称/值会被嵌入 HTTP 请求的主体中），并且对所发送信息的数量也无限制。
* 此外 POST 支持高阶功能，比如在向服务器上传文件时进行 multi-part 二进制输入。
* 不过，由于变量未显示在 URL 中，也就无法将页面添加到书签。
* 提示：开发者偏爱 POST 来发送表单数据。

### 关于表单的更多操作

* [表单验证](http://www.w3school.com.cn/php/php_form_validation.asp)
* [表单必填](http://www.w3school.com.cn/php/php_form_required.asp)
* [验证URL/E-mail](http://www.w3school.com.cn/php/php_form_url_email.asp)
