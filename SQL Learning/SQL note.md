# 从0到1掌握SQL_学习笔记

跟着DataWhale学的, 太感谢了, 课程文档写的很好, 收益匪浅(～￣▽￣)～

【课程链接】
https://github.com/datawhalechina/wonderful-sql

------



## Chp0 环境搭建5.13

安装时参考了B站视频:[MySQL 8.0保姆级下载、安装及配置教程（我妈看了都能学会）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV12q4y1477i/?spm_id_from=333.880.my_history.page.click&vd_source=974d177e150408d11b265848b194f0ea)



我安装过程中遇到的问题:
问题1: mysql：ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost:3306' (10061)

解决办法参考[【已解决】mysql：ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost:3306' (10061) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/661201217?utm_id=0)

问题2:用navicat链接mysql时,无法链接

解决方法: 

step1 win+R

![image-20240515102128080](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/f1c2062c-3047-4947-9a13-3b5f8bf4e8d0)

step2 打开services.msc, 然后启动

![image-20240515102242340](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/e78cd4ec-7f10-4cda-83ca-8403b5db13ca)

------

------



## Chp1 初识数据库

查看现有数据库;

```mysql
show databases;
```

![image-20240514195547192](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/638a2925-c256-4908-b991-2f1cdc728253)




创建课程例子shop数据库;

```mysql
CREATE DATABASE shop;
```

![image-20240515003315997](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/637595ce-a24f-4fca-bf1c-0ccb18309923)





找一下在哪个文件夹;

```mysql
show variables like '%datadir%';
```

![image-20240515003349911](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/0432f15c-cfb0-4f34-827c-0bb5c11f6357)

![image-20240515003426113](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/089ae9e9-d971-40ad-b8ed-fec5c3eac010)



ok,下一步

创建本课程用到的商品表(shop);

```mysql
CREATE TABLE product
(product_id CHAR(4) NOT NULL,
 product_name VARCHAR(100) NOT NULL,
 product_type VARCHAR(32) NOT NULL,
 sale_price INTEGER ,
 purchase_price INTEGER ,
 regist_date DATE ,
 PRIMARY KEY (product_id));
```

但是遇到了错误

![image-20240515004305777](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/20986121-6e85-41b2-9c45-01988c4c4174)


参考了CSDN教程:[MySQL数据库中出现no database selected是什么意思？-CSDN博客](https://fanjufei.blog.csdn.net/article/details/108943943?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1-108943943-blog-84146918.235^v43^pc_blog_bottom_relevance_base9&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1-108943943-blog-84146918.235^v43^pc_blog_bottom_relevance_base9&utm_relevant_index=1)

需要针对某个具体的数据库,此处用:

```mysql
use shop;
```

再用CREATE TABLE语句创建表即可.

![image-20240515004822095](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/cf415e56-6e76-49bc-acec-d7181866fd37)


好耶!

![image-20240515005148107](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/819b5001-b4aa-4c28-8e8a-bb41459b29f9)


![image-20240515005209498](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/68f570bc-3648-4560-bddf-870c007f6bc3)




### 练习题

1.1编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），并为 regist_no （注册编号）列设置主键约束

![ch01.04习题1.png](https://github.com/datawhalechina/wonderful-sql/blob/main/img/ch01/ch01.04%E4%B9%A0%E9%A2%981.png?raw=true)

```mysql
CREATE TABLE Addressbook (
    regist_no INTEGER NOT NULL,
    name VARCHAR(128) NOT NULL,
    address VARCHAR(256) NOT NULL,
    tel_no CHAR(10),
    mail_address CHAR(20),
    PRIMARY KEY (regist_no)
);

```



1.2假设在创建练习1.1中的 Addressbook 表时忘记添加如下一列 postal_code （邮政编码）了，请编写 SQL 把此列添加到 Addressbook 表中。

列名 ： postal_code

数据类型 ：定长字符串类型（长度为 8）

约束 ：不能为 NULL

```mysql
alter table Addressbook add column postal_code varchar(8) not null ;
```



ps: 

*如果想指定插入列的位置

```mysql
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null after COLUMN_NAME ;
```

*如果想插入到第1列

```mysql
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null first ;
```



1.3请补充如下 SQL 语句来删除 Addressbook 表。

```mysql
(    ) table Addressbook;
```

```mysql
DROP TABLE Addressbook;
```



1.4是否可以编写 SQL 语句来恢复删除掉的 Addressbook 表？

不可以

说明要及时备份,小心谨慎,别误删了.....

------

补充:
原来可以在"查询"中批量写代码

![image-20240515103436600](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/b89b7947-b767-4f35-8b12-c3536abf86ad)

------

------




## Chp2 基础查找与排序



**SELECT 语句和WHERE语句**

注意这里一定要缩进
```mysql
SELECT <列名>, ……
  FROM <表名>
  WHERE <条件表达式>;
```

```mysql
SELECT product_name, product_type
  FROM product
  WHERE product_type = "衣服";
```
![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/3d2e2bf7-48ec-43dd-a250-881866d7ba06)
```mysql
SELECT product_name
  FROM product
  WHERE product_type = "衣服";
```
![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/cf896f85-3ff2-4c01-80e2-9f001b7edf79)

**注:运算优先级**

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/a2557a10-5a3d-4fd2-9809-ac53dca48786)


### 练习题Part1
2.1编写一条SQL语句，从`product(商品)`表中选取出“登记日期(`regist_date`)在2009年4月28日之后”的商品，查询结果要包含`product_name`和`regist_date`两列。

```mysql
SELECT product_name, regist_date
  FROM product
	WHERE regist_date >= '2009-04-28'
```

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/b2c009d0-b1ed-46ce-a34c-6cba2d63431a)

2.2请说出对product 表执行如下3条SELECT语句时的返回结果。

①

```mysql
SELECT *
  FROM product
  WHERE purchase_price = NULL;
```

返回purchase_price列为NULL的记录

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/4034c3b4-8c4b-47ce-9aab-5e90ea548187)

②

```mysql
SELECT *
  FROM product
  WHERE purchase_price <> NULL;
```

返回purchase_price列为NULL的记录

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/26987d24-4d0c-405a-bc1b-bc0c0d0d998a)


③

```mysql
SELECT *
  FROM product
  WHERE product_name > NULL;
```

返回purchase_price列为NULL的记录

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/63ee1660-4311-4216-b0e2-cd1c14beb10d)


why? NULL的真值结果既不为真，也不为假，因为并不知道这样一个值。

真<NULL(不确定)<假

2.3在2.2.3章节中的SELECT语句能够从 product 表中取出“销售单价（sale_price）比进货单价（purchase_price）高出500日元以上”的商品。请写出两条可以得到相同结果的SELECT语句。执行结果如下所示：

```mysql
-- 第一种
SELECT product_name, sale_price, purchase_price
  FROM product
	WHERE sale_price >= purchase_price + 500;
	
-- 第二种
SELECT product_name, sale_price, purchase_price
  FROM product
	WHERE NOT sale_price < purchase_price + 500;
```

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/2252bcde-f70b-482d-932b-c676d00c521b)


①

```mysql
SELECT product_name, sale_price, purchase_price
  FROM product
  WHERE purchase_price > 500;
```

②

```mysql
SELECT product_name, sale_price, purchase_price
  FROM product
  WHERE NOT purchase_price <= 500;
```

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/f8ff3d8e-0c1d-4ecc-8bfa-dbb343abd95d)

2.4请写出一条SELECT语句，从 product 表中选取出满足“销售单价打九折之后利润高于 100 日元的办公用品和厨房用具”条件的记录。查询结果要包括 product_name列、product_type 列以及销售单价打九折之后的利润（别名设定为 profit）。

提示：销售单价打九折，可以通过 sale_price 列的值乘以0.9获得，利润可以通过该值减去 purchase_price 列的值获得。

```mysql
-- 添加profit列
ALTER TABLE product ADD COLUMN profit DECIMAL(10, 2);

-- 计算并填充profit列的值
UPDATE product
SET profit = sale_price * 0.9 - purchase_price;

-- 查询profit大于100的产品并输出
SELECT product_name, product_type, profit
  FROM product
  WHERE profit > 100;
```

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/3b71e1dc-2222-4cc6-a87d-250a5d91d49c)


### 练习题Part2

2.5请指出下述SELECT语句中所有的语法错误。

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/c0a1fa66-d3ff-4bf2-ae83-30cf112a0b0e)

Ⅰ  GROUP BY 字段（product_type）与 SELECT 字段不同（product_id）***

Ⅱ  product_name可能有多个重复的, 需要使用DISTINCT函数

Ⅲ  GROUP BY的子句书写顺序有严格要求，不按要求会导致SQL无法正常执行，目前出现过的子句顺序为：1. SELECT ➡️ 2. FROM ➡️ 3. WHERE ➡️ 4. GROUP BY

Ⅳ  SUM函数用于对数值列进行求和,product_name为字符型,不可以用


2.6请编写一条SELECT语句，求出销售单价（ sale_price 列）合计值大于进货单价（ purchase_price 列）合计值1.5倍的商品种类。执行结果如下所示。

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/8868dd21-8f9a-4e19-93dc-41d8238bdc35)

```mysql
SELECT product_type, SUM(sale_price), SUM(purchase_price)
  FROM product
	GROUP BY product_type
	HAVING SUM(sale_price)
```

2.7此前我们曾经使用SELECT语句选取出了product（商品）表中的全部记录。当时我们使用了 ORDER BY 子句来指定排列顺序，但现在已经无法记起当时如何指定的了。请根据下列执行结果，思考 ORDER BY 子句的内容。

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/98bef8c4-1320-479c-abb3-4618a13ca0ef)

日期逆序很明显, 然后看其他的

```mysql
SELECT *
    FROM product
    ORDER BY - regist_date , sale_price;
```

------

------



## Chp3 复杂一点的查询

**视图和表**

表: 保存实际的数据

视图: "视图不是表，视图是虚表，视图依赖于表"



注: 在查询语句`SELECT product_name FROM view_product;`中操作的是一个视图



为什么有了表还需要视图?

1. 通过定义视图可以将频繁使用的SELECT语句保存以提高效率。
2. 通过定义视图可以使用户看到的数据更加清晰。
3. 通过定义视图可以不对外公开数据表全部字段，增强数据的保密性。
4. 通过定义视图可以降低数据的冗余。



**创建视图**

```mysql
-- 创建视图的基本语法
CREATE VIEW <视图名字>(<列1>,<列2>,...)AS <SELECT语句>
```

注: 在一般的DBMS中定义视图时, 不可以用ODRDER BY语句: 因为视图和表一样，**数据行都是没有顺序的**。





①在product表的基础上创建一个视图

![image-20240528233200080](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528233200080.png)

![image-20240528233359272](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528233359272.png)

②基于多表的视图

创建表shop_product

![image-20240528233741691](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528233741691.png)

![image-20240528233933377](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528233933377.png)

在 product 表和 shop_product 表的基础上创建视图

![image-20240528234144329](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528234144329.png)

![image-20240528234157948](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528234157948.png)

在这个视图的基础上进行查询

![image-20240528234348052](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528234348052.png)

修改视图`ALTER VIEW <视图名> AS <SELECT语句>`

![image-20240528235734462](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528235734462.png)

![image-20240528235827831](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240528235827831.png)

③更新视图

对于一个视图来说，如果包含以下结构的任意一种都是不可以被更新的：

- 聚合函数 SUM()、MIN()、MAX()、COUNT() 等。
- DISTINCT 关键字。
- GROUP BY 子句。
- HAVING 子句。
- UNION 或 UNION ALL 运算符。
- FROM 子句中包含多个表。



```mysql
-- 删除视图
DROP VIEW productsum;
-- 查看视图1
USE shop;
SHOW TABLE STATUS WHERE COMMENT = 'view'
-- 查看视图2
SELECT * FROM
information_schema.views 
WHERE TABLE_SCHEMA = 'shop'
```



### *子查询*

子查询指一个查询语句嵌套在另一个查询语句内部的查询

**嵌套子查询**

![image-20240529002127329](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529002127329.png)

**标量子查询**

![image-20240529002302877](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529002302877.png)

![image-20240529002327923](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529002327923.png)

------

### 练习题-第一部分

**3.1**创建出满足下述三个条件的视图（视图名称为 ViewPractice5_1）。使用 product（商品）表作为参照表，假设表中包含初始状态的 8 行数据。

- 条件 1：销售单价大于等于 1000 日元。
- 条件 2：登记日期是 2009 年 9 月 20 日。
- 条件 3：包含商品名称、销售单价和登记日期三列。

对该视图执行 SELECT 语句的结果如下所示。

```mysql
SELECT * FROM ViewPractice5_1;
```

执行结果

```mysql
product_name | sale_price | regist_date
--------------+------------+------------
T恤衫         | 　 1000    | 2009-09-20
菜刀          |    3000    | 2009-09-20
```

Solution:

```mysql
-- 3.1
CREATE VIEW ViewPractice5_1(product_name, sale_price, regist_date)
AS 
SELECT product_name, sale_price, regist_date
FROM product
WHERE sale_price>=1000 AND regist_date >= '2009-09-20';
```

![image-20240529003216359](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529003216359.png)

![image-20240529003227835](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529003227835.png)

**3.2**向习题一中创建的视图 `ViewPractice5_1` 中插入如下数据，会得到什么样的结果？为什么？

```mysql
INSERT INTO ViewPractice5_1 VALUES (' 刀子 ', 300, '2009-11-02');
```

![image-20240529003421499](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529003421499.png)

简单的来说就是格式不对, 不能正确插入

答案(好专业看不懂):

![image-20240529004000995](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529004000995.png)

**3.3**请根据如下结果编写 SELECT 语句，其中 sale_price_avg 列为全部商品的平均销售单价。

[![图片](https://github.com/datawhalechina/wonderful-sql/raw/main/img/ch03/ch03.10-1-sale_price_avg.png)](https://github.com/datawhalechina/wonderful-sql/blob/main/img/ch03/ch03.10-1-sale_price_avg.png)

我写的错误答案: 视图（View）是一个虚拟表，其内容由查询定义。它们不存储数据，而是在查询视图时动态生成数据。因此，你不能直接向视图中添加列，也不能更新视图中的数据，因为视图本身不包含数据。

![image-20240529005328062](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529005328062.png)

![image-20240529005415522](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529005415522.png)

**3.4**请根据习题一中的条件编写一条 SQL 语句，创建一幅包含如下数据的视图（名称为AvgPriceByType）。

[![图片](https://github.com/datawhalechina/wonderful-sql/raw/main/img/ch03/ch03.10-2-sale_price_avg_type.png)](https://github.com/datawhalechina/wonderful-sql/blob/main/img/ch03/ch03.10-2-sale_price_avg_type.png)

提示：其中的关键是 `sale_price_avg_type` 列。与习题三不同，这里需要计算出的 是各商品种类的平均销售单价。这与使用关联子查询所得到的结果相同。 也就是说，该列可以使用关联子查询进行创建。问题就是应该在什么地方使用这个关联子查询。

![image-20240529005825526](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529005825526.png)

![image-20240529005915157](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529005915157.png)

------

***各种各样的函数***

- ABS -- 绝对值

语法：`ABS( 数值 )`

ABS 函数用于计算一个数字的绝对值，表示一个数到原点的距离。

当 ABS 函数的参数为`NULL`时，返回值也是`NULL`。

- MOD -- 求余数

语法：`MOD( 被除数，除数 )`

MOD 是计算除法余数（求余）的函数，是 modulo 的缩写。小数没有余数的概念，只能对整数列求余数。

注意：主流的 DBMS 都支持 MOD 函数，只有SQL Server 不支持该函数，其使用`%`符号来计算余数。

- ROUND -- 四舍五入

语法：`ROUND( 对象数值，保留小数的位数 )`

ROUND 函数用来进行四舍五入操作。

注意：当参数 **保留小数的位数** 为变量时，可能会遇到错误，请谨慎使用变量。

------

- CONCAT -- 拼接

语法：`CONCAT(str1, str2, str3)`

MySQL中使用 CONCAT 函数进行拼接。

- LENGTH -- 字符串长度

语法：`LENGTH( 字符串 )`

- LOWER -- 小写转换

LOWER 函数只能针对英文字母使用，它会将参数中的字符串全都转换为小写。该函数不适用于英文字母以外的场合，不影响原本就是小写的字符。

类似的， UPPER 函数用于大写转换。

- REPLACE -- 字符串的替换

语法：`REPLACE( 对象字符串，替换前的字符串，替换后的字符串 )`

- SUBSTRING -- 字符串的截取

语法：`SUBSTRING （对象字符串 FROM 截取的起始位置 FOR 截取的字符数）`

使用 SUBSTRING 函数 可以截取出字符串中的一部分字符串。截取的起始位置从字符串最左侧开始计算，索引值起始为1。

**（扩展内容）SUBSTRING_INDEX -- 字符串按索引截取**

语法：`SUBSTRING_INDEX (原始字符串， 分隔符，n)`

该函数用来获取原始字符串按照分隔符分割后，第 n 个分隔符之前（或之后）的子字符串，支持正向和反向索引，索引起始值分别为 1 和 -1。

**（扩展内容）REPEAT -- 字符串按需重复多次**

语法：`REPEAT(string, number)`

该函数用来对特定字符实现按需重复。

------

- CURRENT_DATE -- 获取当前日期
- CURRENT_TIME -- 当前时间
- CURRENT_TIMESTAMP -- 当前日期和时间

- EXTRACT -- 截取日期元素

------

- CAST -- 类型转换

语法：`CAST（转换前的值 AS 想要转换的数据类型）`

需要特别注意的是，当要转换为整型时，需要指定为 SIGNED（有符号） 或者 UNSIGNED（无符号）

- COALESCE -- 将NULL转换为其他值

语法：`COALESCE(数据1，数据2，数据3……)`

COALESCE 是 SQL 特有的函数。该函数会返回可变参数 A 中左侧开始第 1个不是NULL的值。参数个数是可变的，因此可以根据需要无限增加。

在 SQL 语句中将 NULL 转换为其他值时就会用到转换函数。

------

### 练习题-第二部分

**3.5**判断:四则运算中含有 NULL 时（不进行特殊处理的情况下），运算结果是否必然会变为NULL ？

✅

![image-20240529011649088](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529011649088.png)



**3.6**对本章中使用的 `product`（商品）表执行如下 2 条 `SELECT` 语句，能够得到什么样的结果呢？

①

```mysql
SELECT product_name, purchase_price
  FROM product
 WHERE purchase_price NOT IN (500, 2800, 5000);
```

![image-20240529011837403](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529011837403.png)

②

```mysql
SELECT product_name, purchase_price
  FROM product
 WHERE purchase_price NOT IN (500, 2800, 5000, NULL);
```

![image-20240529011858834](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529011858834.png)

答案:



![image-20240529012703166](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529012703166.png)

⭐⭐⭐

**3.7**按照销售单价( `sale_price` )对练习 3.6 中的 `product`（商品）表中的商品进行如下分类。

- 低档商品：销售单价在1000日元以下（T恤衫、办公用品、叉子、擦菜板、 圆珠笔）
- 中档商品：销售单价在1001日元以上3000日元以下（菜刀）
- 高档商品：销售单价在3001日元以上（运动T恤、高压锅）

请编写出统计上述商品种类中所包含的商品数量的 SELECT 语句，结果如下所示。

执行结果

```mysql
low_price | mid_price | high_price
----------+-----------+------------
        5 |         1 |         2
```

![image-20240529013003403](C:\Users\linyifeng\AppData\Roaming\Typora\typora-user-images\image-20240529013003403.png)

------

------



## Chp4 集合运算

