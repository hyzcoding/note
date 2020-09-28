# JAVA基础
## 简介

```
JRE ---------------Java Runtime Environment--运行环境（使用者运行）
Server JRE -----------  服务端使用的运行环境
SDK ---------------- Software Development Kit -- 软件开发工具包
String[] args -- 数组形式 
String … args --可变参数形式
```

## 特性

* 1. 简单的

* 2. 面向对象的

* 3. 分布式

* 4. 健壮的

* 5. 安全的

* 6. 体系结构中立的

* 7. 可移植的

* 8. 解释型的

* 9. 高性能的

* 10. 多线程的

* 11. 动态的

## 对象

- 面向对象的三大特性
  1. 封装
  2. 继承
  3. 多态
  4. 抽象

## 继承与多态

- 重载与重写的区别

  > 重载就是同样的一个方法能够根据输入数据的不同，做出不同的处理
  >
  > 重写就是当子类继承自父类的相同方法，输入数据一样，但要做出有别于父类的响应时，你就要覆盖父类方法

## 命令行

```bash

java -jar app.jar -Dspring.profiles.active=dev --server.module

# 测试环境

java -jar app.jar -Dspring.profiles.active=qa
java -jar app.jar -Dspring.profiles.active=qa --server.port=8060

# 生产环境

java -jar  -Dspring.profiles.active=prod,module /home/hyz/home/home-0.0.1-SNAPSHOT.jar


java -jar  -Dspring.profiles.active=prod,module -Dlogging.level.com.pqtel.pqcloud.core.dao=DEBUG /home/hyz/home/home-0.0.1-SNAPSHOT.jar
			
java -jar  -Dspring.profiles.active=prod,module -Dserver.port=80   /home/hyz/home/home-0.0.1-SNAPSHOT.jar



```

