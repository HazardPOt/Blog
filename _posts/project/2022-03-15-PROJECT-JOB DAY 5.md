# PROJECT-JOB DAY 5
## Java基础
复习：https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/toc/JAVA_BASE.md


复习：https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/toc/COLLECTION.md
## 计网基础
复习：
### 了解SQL
数据库、表、列、行、主键的概念
### 检索数据
SELECT语句
* 检索单个列
> SELECT prod_name
> FROM Prodects;
* 检索多个列
> SELECT prod_id, prod_name, prod_price 
> FROM Prodects;
* 检索所有列
> SELECT * 
> FROM Prodects;
* 限制结果
> SELECT TOP 5 prod_name
> FROM Prodects;
### 排序检索数据
* 单一排序
> SELECT prod_name
> FROM Prodects
> ORDERED BY prod_name;
* 多个列排序
> SELECT prod_id, prod_name, prod_price
> FROM Prodects
> ORDERED BY prod_price, prod_name;

先按照price排列，再按name排列
* 降序排列
> SELECT prod_name
> FROM Prodects
> ORDERED BY prod_name DESC;

关键字 **DESC 降序排列**  **ASC 升序排列**
### 过滤数据
使用**WHERE**语句，限定过滤条件
> SELECT prod_name, prod_price
> FROM Prodects
> WHERE prod_price = 3.49;

只返回price = 3.49的行
注意：**WHERE**字句的位置应在**ORDERED BY**语句**之前**

注意：**单引号**用来限定**字符串**
> SELECT vend_id, prod_name
> FROM Prodects
> WHERE vend_id **!=** 'DLL01';

* 范围值检查
> SELECT vend_id, prod_name
> FROM Prodects
> WHERE vend_id **BETWEEN** 5 **AND** 10;

### 高级数据过滤
**AND = 并且
OR = 或者**
* 组合WHERE字句
> SELECT prod_name, prod_price
> FROM Prodects
> WHERE **(vend_id == 'DLL01' OR vend_id == 'BRS01')** AND prod_price >= 10;
* IN操作符
> SELECT prod_name, prod_price
> FROM Prodects
> WHERE vend_id **IN** ('DLL01', 'BRS01');

用IN来指定条件范围，和OR具有相同的功能，但是比OR有优点。优点有：语法清晰，顺序容易管理，比OR的执行速度快，**可以包含其他SELECT语句**
* NOT语句
> SELECT prod_name, prod_price
> FROM Prodects
> WHERE vend_id NOT = 'DLL01'
> ORDERED BY prod_price;

NOT用来**匹配DLL01之外**的所有东西


### 用通配符进行过滤
* LIKE操作符（模糊查询）
> SELECT prod_name, prod_price
> FROM Prodects
> WHERE prod_name **LIKE** 'FISH%'

* 百分号（%）通配符
’FISH%‘的意思是**查找字符串开头为’FISH‘之后的任意字符**，**不管它有多少字符**
> SELECT prod_name, prod_price
> FROM Prodects
> WHERE prod_name **LIKE** '%FISH%'

’%FISH%‘表示匹配**任何位置上包含文本FISH的值**。
* 下划线（_）通配符

> SELECT prod_name, prod_price
> FROM Prodects
> WHERE prod_name **LIKE** '__inch teddy bear'

意思是查找包含两个字符 + inch teddy bear的名字

若
> SELECT prod_name, prod_price
> FROM Prodects
> WHERE prod_name **LIKE** '%inch teddy bear'


则查找包含任意字符 + inch teddy bear的名字
* 使用通配符的技巧
通配符搜索一般会比前文讨论的搜索耗费更多时间，所以**不要过度使用通配符**，如使用也尽量把他们**用在搜索模式的末尾**，若放在开始处，搜索起来会比较慢
### JAVA 豆瓣爬虫 DAY 2
存在问题 Spring配置文件出错
### 剑指38 39 40

