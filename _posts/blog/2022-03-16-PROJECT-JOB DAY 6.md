---
layout:     post
title:     PROJECT-JOB DAY6
description:     Project JOB
date:     2022-03-16
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: project
tags:     
    -   
        -   

    -   

---
## Java基础
复习：https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/toc/JAVA_BASE.md


复习：https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/toc/COLLECTION.md
## 计网基础
复习：https://github.com/wolverinn/Waking-Up/blob/master/Computer%20Network.md

## SQL
### 创建计算字段
> SELECT **RERIM**(vend_name) , '(' + RTRIM(vend_country) + ')' AS vend_title
> FROM Vendors
> ORDER BY vend_name

> SELECT prod_id, quantity, item_price, **quantity * item_price** AS expanded_price
> FROM OrderItems
> WHERE order_num = 200008;

### 使用函数处理数据
常见函数：**LEFT()**, **LENGTH()**, **LOWER()**, **LIRIM()**, **RIGHT()**, **RTRIM()**, **SUBSTR()或SUBSTRING()**, **SOUNDEX()**, **UPPER()**
时间函数 **DATA_FORMAT()**
按时间范围查找： WHERE DATE_FORMAT(Time, '%Y%M%D') BETWEEN ‘xxxx-xx-xx’ AND 'xxxx-xx-xx'
### 汇总数据
聚集函数：**AVG()**, **COUNT()**, **MAX()**, **MIN()**, **SUM()**
去重可以用**DISTINCT**参数，like: **SELECT AVG(DISTINCT prod_price) AS avg_price**
> SECECT COUNT(*) AS num_item,
> MIN(prod_price) AS price_min,
> MAX(prod_price) AS price_avg
FROM Products

### 分组数据
GROUP BY
HAVING：**不可以用HAVING修饰别名**
GROUP BY 在 WHERE 之前，在 ORDER BY 之后
一般来说 GROUP BY 后面都会跟着 ORDER BY
> SELECT order_num, COUNT( * ) AS item
> FROM OrderItems
> GROUP BY order_num
> HAVING COUNT( * ) >= 3
> ORDER BY item, order_num

### 使用子查询
是嵌套在其他查询中的查询
> SELECT cust_name, cust_state, **(SECECT COUNT( * ) FROM Orders WHERE Orders.cust_id = Customers.cust_id)** AS orders
> FROM Customers
> ORDER BY cust_name

### 联结表
#### 等价联结
> SELECT vend_name, prod_name, prod_price
> FROM Vendors, Products
> WHERE Vendors.vend_id = Product.vend_id

#### 内联结
> SELECT vend_name, prod_name, prod_price
> FROM Vendors
> INNER JOIN Products ON Vendors.vend_id = Products.vend_id

## 剑指 42 45 
