# 大数据处理架构Hadoop

## Hadoop简介与版本演变

# 简介

> apache开源软件
>
> 包装nio等并发编程
>
> 支持多种编程语言

> HDFS+MapReduce
>
> HDFS 分布式存储
>
> MapReduce 分布式处理

> 数据排序 -- 海量数据处理

## 特性

- 高可靠性
- 高效性
- 高可扩展性
- 高容错性
- 成本低

## 应用现状

- 日志处理、行为分析

![image-20201002104824464](..\..\_media\bigdata\image-20201002104824464.png)

## 不同版本

- apache 

    ![image-20201002105147175](..\..\_media\bigdata\image-20201002105147175.png)

    - 2.0改进

    > yarn 从mapreduce分解，管理资源调度，mapreduce 之进行计算
    >
    > storm spark 等都搭建在yarn框架之上

    > 多个名称节点、热备份namenode

- Cloudera (CDH)
- MapR

![image-20201002105924551](..\..\_media\bigdata\image-20201002105924551.png)

![image-20201002110024822](..\..\_media\bigdata\image-20201002110024822.png)

![image-20201002110024822](..\..\_media\bigdata\image-20201002110024822.png)

# 项目结构

Tez 有向无环图

![image-20201002110417266](..\..\_media\bigdata\image-20201002110417266.png)

Spark 基于内存、MapReduce 基于磁盘、Hive将SQL转成MapReduce

Pig 流处理，轻量级脚本语言

Oozie 作业流调度

HBase 列族数据库

## Hadoop 安装

略

# Hadoop 集群部署

略