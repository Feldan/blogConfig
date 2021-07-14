---
title: 记录Mysql命令
date: 2020-08-16 00:05:50
tags: mysql
categories: 
- 学习
- 数据库
---
# Mysql命令

1. 显示数据库列表
   - `show databases;`   
2. 显示库中的数据表： 
   - `use mysql;`  #选择使用数据库 
   - `show tables;`    
3. 显示数据表的结构： 
   - `describe 表名;`  
4. 建库：   
   - `create database 库名;` 
5. 建表：  
   - `use 库名;`   
   - `create table 表名 (字段设定列表);`  
6. 删库和删表: 
   - `drop database 库名;` 
   - `drop table 表名;`
7. 将表中记录清空 
   - `delete from 表名;`  
8. 显示表中的记录： 
   - `select * from 表名`



# 以下为创建MySQL数据表的SQL通用语法：

`CREATE TABLE table_name (column_name column_type);`  

以下例子中我们将在 33 数据库中创建数据表33：

```MYSQL
mysql> CREATE TABLE itv_list (
    -> department int unsigned,
    -> num int unsigned,
    -> comment varchar(128),
    -> code char(5),
    -> begin timestamp,
    -> creator int unsigned
    -> )CHARSET=utf8mb4;
Query OK, 0 rows affected (0.94 sec)
```