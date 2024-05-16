# 从0到1掌握SQL

## 0 环境搭建5.13

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



## 1 初识数据库5.15

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


## 1基础查找与排序5.16
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

**运算优先级**
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

![image](https://github.com/TranquilMaple/The-learning-journey-of-YF/assets/139969854/f4633217-51de-4e9c-8df0-cca98bafece5)

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

