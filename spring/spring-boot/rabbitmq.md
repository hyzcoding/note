# SpringBoot 使用 rabbitmq

## 配置

- pom.xml

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-amqp</artifactId>
    </dependency>

    ```

- application.yml

  ```yml
  server:
    port: 8092
  spring:
    application:
      name: jungle-document
    rabbitmq:
      host: localhost
      port: 5672
      username: test
      password: 123456
  ```

### 发布者(Producer)



### 消费者(Consumer)





