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

