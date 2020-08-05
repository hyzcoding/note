# 关系型数据库

## 简介

> 当前主流的关系型数据库有Postgresql、MySQL、Sybase、Oracle、MariaDB、SQLite等。

## 三大范式



## 数据库引擎

### MyISAM与InnoDB

| 条件     | MyISAM                                                       | InnoDB      |
| -------- | ------------------------------------------------------------ | ----------- |
| 适合场景 | 频繁的全表count语句，<br/>查询频繁，增删改较少，<br/>无事务需求 | 有事务需求  |
| 锁       | 表级锁                                                       | 行级锁      |
| 外键     | 不支持                                                       | 支持        |
| 全文索引 | 支持                                                         | 5.6以后支持 |





## 基本语法
```sql
insert into user (id,name,age) values (1,'aa',18);
update user set name='aaa' where id=1;
delete from user where id = 1;
select * from user left join emp on user.id=emp.id where user.age>10 sort by user.age desc;
```



## 扩展

- #### 扩容

  - 垂直扩容/拆分：按照业务将表进行分类
  - 水平扩容/拆分：按照数据行的切分