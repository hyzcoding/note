# 关系型数据库

## 基本语法
```sql
insert into user (id,name,age) values (1,'aa',18);
update user set name='aaa' where id=1;
delete from user where id = 1;
select * from user left join emp on user.id=emp.id where user.age>10 sort by user.age desc;
```

