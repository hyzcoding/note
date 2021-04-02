# 关系型数据库

## 简介

> 当前主流的关系型数据库有Postgresql、MySQL、Sybase、Oracle、MariaDB、SQLite等。

## 三大范式



## 数据库引擎

### MyISAM与InnoDB

| 条件     | MyISAM                                                       | InnoDB      |
| -------- | ------------------------------------------------------------ | ----------- |
| 适合场景 | 频繁的全表count语句，<br>查询频繁，增删改较少，<br>无事务需求 | 有事务需求  |
| 锁       | 表级锁                                                       | 行级锁      |
| 外键     | 不支持                                                       | 支持        |
| 全文索引 | 支持                                                         | 5.6以后支持 |





## 基本语法

- DCL 

```bash
mysql -D 数据库名 -h 主机名 -p 端口号 -u 用户名 -p < 脚本名称.sql

mysqladmin -u root -p password 新密码

create user 'user'@'192.168.%' IDENTIFIED BY 'password';
GRANT ALL/SELECT, INSERT/privileges  ON database.table TO 'user'@'192.168.%';
```



- DDL

```sql
create database my_database character set utf8; -- create database 数据库名 [其它选项];
use my_database; -- use 数据库名
create table mytable (
	id int unsigned not null auto_increment primary key,
    name varchar(10) not null,
    sex tinyint not null,
    tel char(12) null default "-"
); --create table 表名称(列声明)
```

- DML

```sql
insert into user (id,name,age) values (1,'aa',18);
update user set name='aaa' where id=1;
delete from user where id = 1;
select * from user left join emp on user.id=emp.id where user.age>10 sort by user.age desc;
```

- DDL

```sql
alter table 表名 add 列名 列数据类型 [after 插入位置];
alter table my_table add address char(60) after tel;

alter table 表名 change 列名称 列新名称 新数据类型;
alter table my_table change tel telphone char(15) [default "-"] not null;

alter table 表名 drop 列名称;
alter atble my_table drop address;

alter table 表名 rename 新表名;
alter table my_table rename students;

drop table 表名;
drop table students;

drop database 数据库名;
drop database my_database;
```



## 扩展

- #### 扩容

  - 垂直扩容/拆分：按照业务将表进行分类
  - 水平扩容/拆分：按照数据行的切分