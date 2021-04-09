# zookeeper
## 简介

服务注册发现，分布式服务框架，分布式锁

## 使用场景



## 预制环境

* jdk



## 安装

### 单机模式

 ```bash
> cp conf/zoo_sample.cfg conf/zoo.cfg
> bin/zkServer.sh start
> bin/zkCli.sh
 ```


### 集群模式

```bash
echo '1' > data/myid

## zoo.cfg
tickTime=2000
initLimit=5
syncLimit=2
dataDir=/data/soft/zookeeper-cluster/zookeeper-3.4.12-12181/data
dataLogDir=/data/soft/zookeeper-cluster/zookeeper-3.4.12-12181/logs
clientPort=12181

server.1=127.0.0.1:12888:13888
server.2=127.0.0.1:14888:15888
server.3=127.0.0.1:16888:17888

## 分别启动
> bin/zkServer.sh
```



## 配置



## 使用

- 服务注册发现
- 分布式锁

## 原理解析

- 内部结构
- CP一致性