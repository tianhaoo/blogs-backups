---
title: php登录
date: 2018-01-20 00:39:50
categories:
- 教程
tags:
  - php
---

实现一个纯前端的静态登陆界面



#### 先上代码

如果你已经装好了wamp环境，那么你只需要在htdocs文件夹下创建一个名为form1.html的文件，把这些代码写进去就好了。

*(我的htdocs文件夹路径为C:\Bitnami\wampstack-7.1.8-0\apache2\htdocs视自己情况而定)*
<!-- more -->
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

然后在浏览器里键入`http://localhost/form1.html`
*(这里的from1.html是因为我上文创建这个文件为form1.html,具体看自己怎么命名的)*

*接下来是一步一步的解释*

#### 先是第一行

```
<!DOCTYPE html>
```

* `<!DOCTYPE>` 声明必须是 HTML 文档的第一行，位于 <html> 标签之前。
* `<!DOCTYPE>` 声明不是 HTML 标签；它是指示 web 浏览器关于页面使用哪个 HTML 版本进行编写的指令。

*（其实要是没有特殊要求，很浏览多器不写也行, 而且还有更复杂的写法）*

[more](http://www.w3school.com.cn/tags/tag_doctype.asp)


#### head标签

```
<head>
    <meta charset="utf-8">
    <title>simpleForm</title>
</head>
```

* head 标签用于定义文档的头部，它是所有头部元素的容器。head中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。*(虽然你们现在看不懂什么意思）*
* 文档的头部描述了文档的各种属性和信息，包括文档的标题、在 Web 中的位置以及和其他文档的关系等。绝大多数文档头部包含的数据都不会真正作为内容显示给读者。
* 下面这些标签可用在 head 部分：link, meta, script, style, title

#### meta标签
```
<meta charset="utf-8">
```

* title定义文档的标题，它是 head 部分中唯一必需的元素。


#### body标签

```
<body>
    <form action="" name="" type="" method="">
        名字:<input type="text" name="name">
        性别:<input type="text" name="password">
        <input type="submit" value="提交">
    </form>
</body>
```

* body 元素定义文档的主体。
* body 元素包含文档的所有内容（比如文本、超链接、图像、表格和列表等等。）

#### input标签

```
<input type="text" name="name">
<input type="text" name="password">
<input type="submit" value="提交">
```

* input 标签用于搜集用户信息。
* 在input标签里，根据不同的 type 属性值，输入字段拥有很多种形式。常见的几种属性如下
    + text    定义常规文本输入。
    + radio   定义单选按钮输入（选择多个选择之一）
    + checkbox 定义复选框
    + submit  定义提交按钮（提交表单）
    + [more](http://www.w3school.com.cn/tags/tag_input.asp)


#### form标签

```
<form name="" action="" method="" target="" >
    名字:<input type="text" name="name">
    性别:<input type="text" name="password">
    <input type="submit" value="提交">
</form>
```

- form 标签用于为用户输入创建 HTML 表单。*（由上文提到的submit按钮来提交表单）*
- 表单能够包含 input 元素，比如文本字段、复选框、单选框、提交按钮等等。*(如上文所说)*
- 表单还可以包含 menus、textarea、fieldset、legend 和 label 元素。
- 表单用于向服务器传输数据。
- 表单有这些常用的属性如下
    + action  规定当提交表单时向何处发送表单数据。
    + method  规定用于发送 form-data 的 HTTP 方法。
    + name    规定表单的名称。
    + target  规定在何处打开 action URL。
    + [more](http://www.w3school.com.cn/tags/tag_form.asp)

*(上面的body标签里面没有定义这些属性，默认是空，具体的值可以在上面的more里看到)*

*好了到现在为止就完成了一个登陆页面，但是点了提交之后并没有任何反应，提交的数据都没了。那是因为我们没有写后端的操作（action属性），下一篇博客将会是关于form表单的后端操作.*
