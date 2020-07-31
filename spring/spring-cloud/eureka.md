# eureka

## 简介

springcloud 服务注册与发现框架

## 配置

- 注册与发现服务

  - pom.xml

    ```xml
    <!-- SpringCloud版本 Hoxton , SpringBoot版本 2.2.4 -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    </dependency>
    ```

    

  - application.yml

    ```yml
    spring:
    #  profiles: docker
      application:
        name: jungle-eureka
    
      # 安全认证的配置
      security:
        basic:
          enabled: false
        user:
          name: admin  # 用户名
          password: 123456   # 用户密码
    server:
      port: 8761
    eureka:
      instance:
        #单机hostname: localhost
        hostname: localhost        #eureka7002.com  eureka服务端的实例名称
      client:
        register-with-eureka: false     #false表示不向注册中心注册自己。
        fetch-registry: false           #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
        serviceUrl:
          #单机设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
          #defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
    
          #Eureka高复用时设置其他的Eureka之间通信
          #defaultZone: http://eureka7003.com:7003/eureka/,http://eureka7004.com:7004/eureka/
          defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
        #server:
        #enable-self-preservation: false   #Eureka服务端关闭心跳连接测试
    ```

    

  - java

    ```java
    @EnableEurekaServer
    @SpringBootApplication
    public class JungleEurekaServerApplication {
        public static void main(String[] args) {
            SpringApplication.run(JungleEurekaServerApplication.class, args);
        }
    }
    ```

    

- 服务注册客户端

  - pom.xml

    ```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    ```

  - application.yml

    ```yml
    spring:
      application:
        name: jungle-user
    server:
      port: 9002
    eureka:
      client:
        serviceUrl:
          defaultZone: http://localhost:8761/eureka/
    ```

  - java

    ```java
    @EnableEurekaClient
    @SpringBootApplication
    public class UserServiceApplication {
        public static void main(String[] args) {
            SpringApplication.run(UserServiceApplication.class, args);
        }
    }
    ```

    

## 扩展

- 自定义监控界面

  ```yml
  # 打开监控界面
  spring:
    freemarker:
      prefer-file-system-access: false
  ```

  