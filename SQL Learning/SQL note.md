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


## 1基础查找与排序
