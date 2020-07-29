## 安装



- docker

  ```bash
  > sudo docker pull rabbitmq：management
  ```

  

## 配置



## 使用

- docker运行

  ```bash
  docker run -dit --name Myrabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 rabbitmq:management
  ```

  