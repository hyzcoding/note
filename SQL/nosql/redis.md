# redis
## 简介

> 开源的、**基于内存**的**数据结构存储器**，可以用作**数据库**、**缓存**和**消息中间件**。

## 特性

> **单线程**，采用**事件循环(Event-Loop)模式**处理客户端请求。
>
> - **不必考虑线程安全问题。**
> - **减少线程切换损耗的时间。**

## 安装

```bash
> tar xzf redis-xxx.tar.gz
> cd redis-xxx
> make
> cd ./src
```





## 配置

```bash
> vi ../redis.conf
daemonize yes 				## 配置后台运行
protected-mode yes 			## 开启保护模式 只有本机可以访问
port 6379 					## 配置为0 , redis将不在socket 上监听任何客户端连接
requirepass foobared		##  设置redis连接密码
databases 16 				## 设置默认数据库数量
```



## 使用

无配置启动

```bash
> cd src
> ./redis-server
```
配置文件启动
```bash
> cd src
> ./redis-server ../redis.conf
```

## 命令

```bash
> ./redis-cli
localhost:6379> keys * 					## 模糊查找键
localhost:6379> get key 				## 获取对应key的值
localhost:6379> exists key 				## key 是否存在
localhost:6379> expire key seconds 		## 更新过期时间
localhost:6379> set key value			## 设置值
```



## 集群搭建

- 哨兵模式