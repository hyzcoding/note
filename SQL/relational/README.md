# 关系型数据库

## 简介

> 当前主流的关系型数据库有Postgresql、MySQL、Sybase、Oracle、MariaDB、SQLite等。

## 三大范式

- **第一范式(确保每列保持原子性)**
- **第二范式(确保表中的每列都和主键相关)**
- **第三范式(确保每列都和主键列直接相关,而不是间接相关)**

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

- **oracle与其他数据库的语法区别**

  oracle数据类型是number, 字符串使用varchar2

  没有if exist判断函数

  时间处理存储不同

- **调优**

  explain

  > - select_type
  >
  > - table
  >
  > - type
  >
  >   常用的类型有： **ALL, index, range, ref, eq_ref, const, system, NULL（从左到右，性能从差到好）**
  >
  > - possible_keys
  >
  >   MySQL能使用哪个索引在表中找到记录，查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用
  >
  > - key
  >
  >   key列显示MySQL实际决定使用的键（索引）
  >
  > - key_len
  >
  >   表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度（key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的）
  >
  > - ref
  >
  >   表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值
  >
  > - rows
  >
  >   表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数
  >
  > - Extra
  >
  >   **包含MySQL解决查询的详细信息**
  >
  >   Using where:列数据是从仅仅使用了索引中的信息而没有读取实际的行动的表返回的，这发生在对表的全部的请求列都是同一个索引的部分的时候，表示mysql服务器将在存储引擎检索行后再进行过滤
  >
  >   Using temporary：表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询
  >
  >   Using filesort：MySQL中无法利用索引完成的排序操作称为“文件排序”
  >
  >   (如果出现以上的两种的Using temporary和Using filesort说明效率低)
  >
  >   Using join buffer：改值强调了在获取连接条件时没有使用索引，并且需要连接缓冲区来存储中间结果。如果出现了这个值，那应该注意，根据查询的具体情况可能需要添加索引来改进能。
  >
  >   Impossible where：这个值强调了where语句会导致没有符合条件的行。
  >
  >   Select tables optimized away：这个值意味着仅通过使用索引，优化器可能仅从聚合函数结果中返回一行

- #### 扩容

  - 垂直扩容/拆分：按照业务将表进行分类
  - 水平扩容/拆分：按照数据行的切分