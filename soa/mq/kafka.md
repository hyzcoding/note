# Kafka

## 简介

> **分布式、分区的、多副本的、多订阅者**，基于**zookeeper**协调的分布式日志系统（也可以当做MQ系统），常见可以用于web/nginx日志、访问日志，消息服务等等。

### 应用场景

	- 日志收集系统
	- 消息系统
	- 用户活动跟踪
	- 运营指标
	- 流式处理
	- 事件源

## 特性

- 高吞吐量、低延迟
- 可扩展性
- 持久性、可靠性
- 容错性
- 高并发

## 组件结构

![结构图解](../../_media/kafka/constructor.png)

kafka中的zookeeper，producer、consumer、broker三者通过zookeeper管理协调请求和转发，这其间的连接都是需要zookeeper来进行分发的。

![结构图解](../../_media/kafka/zookeeper.png)

### 消息传递模式

1. ##### 点对点
生产者发送一条消息到queue，只有一个消费者能收到。
![消息队列点对点](../../_media/kafka/p2p.png)
2. ##### 发布-订阅
发布者发送到topic的消息，只有订阅了topic的订阅者才会收到消息。
![消息队列订阅发布](../../_media/kafka/p2s.png)



## 安装

```bash
 tar -zxvf kafka.tar.gz
```



## 目录

* `/bin`  操作kafka的可执行脚本
* `/config` 配置文件所在目录
* `lib` 依赖库
* 

## 配置

* 单个无需配置

* 集群

  1. 复制配置文件

  ```bash
  > cp config/server.properties config/server-1.properties
  > cp config/server.properties config/server-2.properties
  ```

   2. 修改集群配置

  ```bash
  config/server-1.properties:
      broker.id=1
      listeners=PLAINTEXT://:9093
      log.dir=/tmp/kafka-logs-1
  
  config/server-2.properties:
    broker.id=2
    listeners=PLAINTEXT://:9094
      log.dir=/tmp/kafka-logs-2
  ```

  

## 使用

* 启动

  ```bash
  > bin/zookeeper-server-start.sh config/zookeeper.properties
  
  > bin/kafka-server-start.sh -daemon config/server-1.properties 
  > bin/kafka-server-start.sh -daemon config/server-2.properties 
  ```

  

## JAVA API

[Kafka的简介与架构]: https://www.cnblogs.com/frankdeng/p/9310684.html

[Kafka面试题]: https://zhuanlan.zhihu.com/p/94412266

