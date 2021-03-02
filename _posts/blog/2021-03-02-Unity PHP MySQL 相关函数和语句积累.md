---
layout:     post
title:     Unity PHP MySQL 相关函数和语句积累
description:     Unity PHP MySQL 相关函数和语句积累
date:     2021-03-02
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

​	由于半道出家，没有从头开始系统学习PHP，所以将用到的PHP函数在此积累，加深记忆。

### mysqli_connect() 函数

打开一个到 MySQL 服务器的新的连接：

```
$myHandler=mysqli_connect("localhost","root","Woshitudou0205","myscoresdb");
if(mysqli_connect_errno())//mysqli_connect_errno() 返回最近调用函数的最后一个错误代码：
{
    echo mysqli_connect_error();
    die();
    exit(0);
}
```

### mysqli_query() 函数

执行针对数据库的查询：

```
//保证编码为UTF8
mysqli_query($myHandler,"set names utf8");
//数据库插入数据后执行查询
mysqli_query($myHandler,$sql) ;
```

### mysqli_real_escape_string() 函数

转义在 SQL 语句中使用的字符串中的特殊字符：

```
//读入Unity传来的用户名和分数，使用mysql_real_escape_string校验用户名的合法性（防止SQL注入）
$UserID=mysqli_real_escape_string($myHandler,$_POST["name"]);
```

### mysqli_close() 函数

关闭数据库

```
mysqli_close($myHandler);
```

### mysqli_num_rows() 函数

返回结果集中行的数量：

```
$num_result=mysqli_num_rows($result);
```

### mysqli_fetch_array() 函数

从结果集中取得一行作为数字数组或关联数组：

```
$row=mysqli_fetch_array($result,MYSQLI_ASSOC);//关联数组
```

### mysqli_free_result() 函数

从结果集中取得行，然后释放结果内存：

```
mysqli_free_result($result);
```

### SQL INSERT INTO 语句

INSERT INTO 语句用于向表格中插入新的行：

```
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

[![6F4OKJ.png](https://s3.ax1x.com/2021/03/02/6F4OKJ.png)](https://imgtu.com/i/6F4OKJ)

### SQL SELECT 语句

SELECT 语句用于从表中选取数据。

结果被存储在一个结果表中（称为结果集）。

```
SELECT 列名称 FROM 表名称
SELECT * FROM 表名称
```

```
requestSQL="SELECT * FROM hiscores ORDER by score DESC LIMIT 20";//查询分数最高的20个
```