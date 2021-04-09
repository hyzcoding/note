# Dubbo

## 简介

> 阿里巴巴开源的RPC框架，解决服务治理、服务注册与发现、远程RPC调用

## 整合SpringBoot

- 依赖项

  pom.xml

  ```xml
  <properties>
      <spring-boot.version>2.3.5.RELEASE</spring-boot.version>
      <dubbo.version>2.7.7</dubbo.version>
  </properties>
  ...
  <dependency>
      <groupId>org.apache.dubbo</groupId>
      <artifactId>dubbo-dependencies-bom</artifactId>
      <version>${dubbo.version}</version>
      <type>pom</type>
      <scope>import</scope>
  </dependency>
  ```

  

- 配置项

  application.properties

  ```properties
  dubbo.scan.base-packages=com.hyzcpding.demo.service
  // rpc协议(序列化方式，默认使用基于hessian的序列化)
  dubbo.protocol.name=dubbo
  // 使用hessian 使用fastjson该属性指定为fastjson
  dubbo.protocol.serialization=hessian
  dubbo.protocol.port=12346
  dubbo.application.version=1.0.0
  dubbo.registry.address=nacos://localhost:8848
  ```

  

- 启动项

  

- provider

  ```java
  @DubboService(interfaceClass = DemoService.class)
  public class DemoServiceImpl implements DemoService{
      ...
  }
  ```

  

- consumer