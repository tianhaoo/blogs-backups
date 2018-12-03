---
title: php-mysql
date: 2018-01-20 00:40:12
categories:
- 教程
tags:
  - php
---

一个web应用总是少不了数据库

以最简单的mysql为例，了解php中mysql的用法



### 什么是 MySQL？

* MySQL 是一种数据库。数据库定义了存储信息的结构。

* 在数据库(database)中，存在着一些表(table)，表中有一些字段(columns)。

### 如何使用MySQL

* 如果使用了wamp, 则在`×××/mysql/bin/`目录下打开命令行，或者可以[添加mysql到环境变量](https://jingyan.baidu.com/article/e4d08ffdd5f6670fd2f60d2f.html)，就可以在全局使用MySQL。
<!--more-->
* 登录MySQL

  `mysql -u root -p`

  输入安装mysql时的密码, 将会出现类似下面的提示

  ```
  Enter password:
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 5
  Server version: 5.7.20-0ubuntu0.16.04.1 (Ubuntu)

  Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

  mysql>
  ```

  [忘记密码](https://jingyan.baidu.com/article/495ba841ef412d38b30edeb2.html)

* 查看所有的数据库

  `show databases;`              #　别忘了分号

  结果

  ```
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  4 rows in set (0.00 sec)

  mysql>
  ```

* 选定一个数据库

  `use mysql`  

  结果

  ```
  mysql> use mysql
  Database changed
  ```

* 查看所有的表

  `show tables;`

  结果

  ```
  mysql> show tables;
  +---------------------------+
  | Tables_in_mysql           |
  +---------------------------+
  | columns_priv              |
  | db                        |
  | engine_cost               |
  | event                     |
  | func                      |
  | general_log               |
  | gtid_executed             |
  | help_category             |
  | help_keyword              |
  | help_relation             |
  | help_topic                |
  | innodb_index_stats        |
  | innodb_table_stats        |
  | ndb_binlog_index          |
  | plugin                    |
  | proc                      |
  | procs_priv                |
  | proxies_priv              |
  | server_cost               |
  | servers                   |
  | slave_master_info         |
  | slave_relay_log_info      |
  | slave_worker_info         |
  | slow_log                  |
  | tables_priv               |
  | time_zone                 |
  | time_zone_leap_second     |
  | time_zone_name            |
  | time_zone_transition      |
  | time_zone_transition_type |
  | user                      |
  +---------------------------+
  31 rows in set (0.00 sec)
  ```
  // 这个数据库里面表有点多。。

* 查看表里面所有的字段

  `show columns from help_topic;`

  结果

  ```
  mysql> show columns from help_topic;
  +------------------+----------------------+------+-----+---------+-------+
  | Field            | Type                 | Null | Key | Default | Extra |
  +------------------+----------------------+------+-----+---------+-------+
  | help_topic_id    | int(10) unsigned     | NO   | PRI | NULL    |       |
  | name             | char(64)             | NO   | UNI | NULL    |       |
  | help_category_id | smallint(5) unsigned | NO   |     | NULL    |       |
  | description      | text                 | NO   |     | NULL    |       |
  | example          | text                 | NO   |     | NULL    |       |
  | url              | text                 | NO   |     | NULL    |       |
  +------------------+----------------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)
  ```

* 显示表里面的所有信息

  `select * from help_topic;`

  这个太长了，结果就不贴了。

  [Learn More](http://www.w3school.com.cn/php/php_mysql_intro.asp)

  所以数据库语法还是很简单的，但是用起来每次都得输入这么长一堆命令，就太麻烦了，好在我们并不需要经常手动去操作数据库，这就要用到我们的php连接mysql操作了

### php 连接mysql

##### 这是我们之前写的例子

  > form.html
  ```
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>my-form</title>
  </head>
  <body>
      <form action="form-action.php" method="get">
          用户名:<input type="text" name="name">
          密码:<input type="text" name="password">
          <input type="submit" value="提交">
      </form>
  </body>
  </html>
  ```
  > form_action.php
  ```
  你好，<?php echo $_GET['name']; ?>。
  你的密码是 <?php echo $_GET['password']; ?>。
  ```

##### 现在我们在form_action里面添加操作，把用户输入的用户名密码放到数据库里

1. 首先我们得有个数据库
  - 按照之前的操作在命令行里登录mysql
  - 创建一个名为my-form-db的数据库 `create database my-form-db;`
    ```
    mysql> create database my_form_db;
    Query OK, 1 row affected (0.00 sec)
    ```
  - 切换数据库
    ```
    mysql> use my_form_db;
    Database changed
    ```
  - 创建数据表
    ```
    mysql> create table user_info(
        -> user_id int not null auto_increment,
        -> user_name varchar(100) not null,
        -> user_passwd varchar(100) not null,
        -> sub_time date,
        -> primary key(user_id)
        -> )engine=innodb default charset=utf8;
    Query OK, 0 rows affected (0.01 sec)
    // mysql虽然不区分大小写，但习惯上将关键字大写，别学我，小写不是个好习惯。
    ```
  - 你可以查看你刚刚创建的数据表有哪些字段
    ```
    mysql> show tables;
    +----------------------+
    | Tables_in_my_form_db |
    +----------------------+
    | user_info            |
    +----------------------+
    1 row in set (0.01 sec)

    mysql> show columns from user_info;
    +-------------+--------------+------+-----+---------+----------------+
    | Field       | Type         | Null | Key | Default | Extra          |
    +-------------+--------------+------+-----+---------+----------------+
    | user_id     | int(11)      | NO   | PRI | NULL    | auto_increment |
    | user_name   | varchar(100) | NO   |     | NULL    |                |
    | user_passwd | varchar(100) | NO   |     | NULL    |                |
    | sub_time    | date         | YES  |     | NULL    |                |
    +-------------+--------------+------+-----+---------+----------------+
    4 rows in set (0.01 sec)
    ```
  - [更多关于创建数据库的操作](http://www.runoob.com/mysql/mysql-create-tables.html)

2. 在form_action里面编写代码，插入用户的数据
  - 首先我们得连接已经创建的数据库,就要用到[mysqli_connect()](http://www.runoob.com/php/func-mysqli-connect.html)这个函数，别忘了[开启mysqli扩展](https://www.cnblogs.com/zhdevelop/archive/2013/03/17/2964226.html),本例用法如下
    ```php
    <?php
    $db_host='127.0.0.1';
    $db_user='root';
    $db_psw='your_password';
    $db_name='my_form_db';

    $conn = mysqli_connect($db_host,$db_user,$db_psw, $db_name) or die('数据库连接失败');

    if(!$conn) {
        echo "连接失败<br>";
    }
    else{
        echo "连接成功<br>";
    }
    ```

  - 其次我们要插入用户名和密码以及时间
    ```php
    <?php
    $db_host='127.0.0.1';
    $db_user='root';
    $db_psw='your_password';
    $db_name='my_form_db';
    $db_charset='utf8';

    $sql = "insert into user_info (user_name, user_passwd, sub_time) values ('$my_name', '$my_password', now())";

    $conn = mysqli_connect($db_host,$db_user,$db_psw, $db_name) or die('数据库连接失败');

    if(!$conn) {
        echo "连接失败<br>";
    }
    else{
        echo "连接成功<br>";
    }

    $conn->set_charset('utf8'); //设置查询结果编码

    if ($conn->query($sql) === TRUE) {
        echo "新记录插入成功";
    } else {
        echo "Error:" . $sql . "<br>" . $conn->error;
    }

    $conn->close();
    ```

    - 到目前为止就完成了php与mysql之间的通信，我们可以去数据库里面看看我们都插入了什么数据。
    ```
    mysql> select * from user_info;
    +---------+-----------------+-----------------------+------------+
    | user_id | user_name       | user_passwd           | sub_time   |
    +---------+-----------------+-----------------------+------------+
    |       1 | Jack            | Ross                  | 2018-01-20 |
    |       2 | my_name         | my_password           | 2018-01-20 |
    |       3 | 我和你          | 心连心                | 2018-01-20 |
    |       4 | 123456          | adsfl                 | 2018-01-20 |
    |       5 | 跟着我左手      | 右手一个慢动作        | 2018-01-20 |
    +---------+-----------------+-----------------------+------------+
    5 rows in set (0.00 sec)

    mysql>
    ```

    - 在页面里面读取并且显示数据库里的内容
    ```php
    <?php

    $my_name = $_POST['name'];
    $my_password = $_POST['password'];

    echo '你好'.$my_name."<br>".'你的密码是'.$my_password."<br>";

    // 定义一些变量
    $db_host='127.0.0.1';
    $db_user='root';
    $db_psw='your_password';
    $db_name='my_form_db';
    $db_charset='utf8';

    $sql = "insert into user_info (user_name, user_passwd, sub_time) values ('$my_name', '$my_password', now())";
    $sql2 = "select * from user_info";


    // 连接数据库
    $conn = mysqli_connect($db_host,$db_user,$db_psw, $db_name) or die('数据库连接失败');
    $conn->set_charset('utf8'); //设置查询结果编码

    if(!$conn) {
        echo "连接失败<br>";
    }
    else{
        echo "连接成功<br>";
    }


    // 插入数据
    if ($conn->query($sql) === TRUE) {
        echo "新记录插入成功<br>";
    } else {
        echo "Error:" . $sql . "<br>" . $conn->error;
    }

    // 查询数据
    $result = $conn->query($sql2);

    if ($result->num_rows > 0) {
        echo "查询结果如下<br>";
        while($row = $result->fetch_assoc()) {
            echo "id: " . $row["user_id"]. "----名字:" . $row["user_name"]. "----密码:" . $row["user_passwd"]. "----提交时间:". $row["sub_time"] . "<br>";
        }
    } else {
        echo "数据库里没有记录<br>";
    }

    $conn->close();
    ```


##### 推荐阅读
* [菜鸟教程](http://www.runoob.com/mysql/mysql-tutorial.html)
* [mysql_connect()和mysqli_connect(）的区别](http://blog.csdn.net/kimbing/article/details/52894099)
* [另一篇相似的博客](http://blog.csdn.net/u010837612/article/details/51532165)
