# 分库分表

## 简介

> 是一种水平数据拆分，会按照如ID、用户、时间等维度进行拆分，拆分算法/策略可以是取模、哈希、区间或者数据路由表等。



> 部署多个redis实例，通过Twemproxy并使用一致性哈希算法进行分片，先通过HaProxy进行Twemproxy的负载均衡，然后通过内网域名进行访问。

## 中间件

### [ShardingSphere](http://shardingsphere.apache.org/)

- **简介**

  关系型数据库中间件，数据分片、读写分离、多数据副本、数据加密、影子库压测等功能，以及 `MySQL、PostgreSQL、SQLServer、Oracle `等 SQL 与协议的支持。

  `ShardingSphere` 已于2020年4月16日成为 Apache 软件基金会的顶级项目。

  `Sharding-JDBC、Sharding-Proxy`和`Sharding-Sidecar`这3款相互独立的产品组成。

- **Sharding-JDBC**

  - 支持
    - ORM框架  - `JPA, Hibernate, Mybatis, Spring JDBC Template`

- **Sharding-Proxy**

- **Sharding-Sidecar**

