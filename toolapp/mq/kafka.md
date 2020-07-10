## 简介

### 应用场景

​	日志收集系统和消息系统

### 消息传递模式

1. ##### 点对点
生产者发送一条消息到queue，只有一个消费者能收到。
![消息队列点对点](../../.asset/kafka/p2p.png)
2. ##### 发布-订阅
发布者发送到topic的消息，只有订阅了topic的订阅者才会收到消息。
![消息队列订阅发布](../../.asset/kafka/p2s.png)



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
  
  > bin/kafka-server-start.sh config/server-1.properties &
  > bin/kafka-server-start.sh config/server-2.properties &
  ```

  

## JAVA API

