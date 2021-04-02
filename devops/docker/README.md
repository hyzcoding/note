# docker

## 简介

- 概念

  > 开源的应用容器引擎。属于 Linux 容器的一种封装，提供简单易用的容器使用接口。
  >
  > Docker是一个容器化平台，它以容器的形式将您的应用程序及其所有依赖项打包在一起，以确保您的应用程序在任何环境中无缝运行。

- 应用场景

  

- 优缺点

  > 1. 快速，一致地交付您的应用程序（提供一次性的环境）
  > 2. 响应式部署和扩展
  > 3. 在同一硬件上运行更多工作负载

## 架构

## 安装

## 使用

```bash
> sudo systemctl start docker # 启动
> sudo docker run hello-world # 运行
> sudo docker stop hello-world # 停止
> sudo docker pause : # 暂停容器中所有的进程。
> sudo docker unpause : # 恢复容器中所有的进程。
```



## 命令

- 常用

  ```bash
  docker run --name 名称 -v 容器目录:宿主机目录 -p容器端口:宿主机端口 -d 容器
  docker ps -a --no-trunc ## 查看完整运行命令
  docker pull   # 拉取或者更新指定镜像
  docker push   # 将镜像推送至远程仓库
  docker rm     # 删除容器
  docker rmi    # 删除镜像
  docker images # 列出所有镜像
  docker ps     # 列出所有容器
  docker ps –a  # 同时列出状态
  docker cp	  # 命令用于容器与主机之间的数据拷贝
  # 主机到容器：
  docker cp /www 96f7f14e99ab:/www/
  # 容器到主机：
docker cp 96f7f14e99ab:/www /tmp/
  docker start id
  docker restart id
  docker exec -it id /bin/bash
  docker logs [OPTIONS] CONTAINER #  --details 
  ```
  
  

## 扩展

- 运行状态

- 与虚拟机的不同

- Dockerfile

  

[Docker 入门教程]: http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html

## 问题

> 	cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running
> 		问题 试试 sudo usermod -aG docker $USER 然后 sudo service docker start 说不定有惊喜哦