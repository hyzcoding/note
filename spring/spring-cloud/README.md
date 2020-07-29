# Spring Cloud

- pom.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <packaging>pom</packaging>
      <modules>
          <module>jungle-common</module>
          <module>jungle-gateway</module>
          <module>jungle-config</module>
          <module>jungle-eureka</module>
          <module>jungle-admin</module>
          <module>jungle-provider</module>
          <module>jungle-provider-api</module>
          <module>jungle-zipkin</module>
      </modules>
          <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.3.1.RELEASE</version>
          <relativePath/> <!-- lookup parent from repository -->
      </parent>
          <dependencyManagement>
          <dependencies>
              <dependency>
                  <groupId>org.springframework.cloud</groupId>
                  <artifactId>spring-cloud-dependencies</artifactId>
                  <version>${spring-cloud.version}</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
          </dependencies>
      </dependencyManagement>
  ```

  



[史上最简单的 SpringCloud 教程]: https://github.com/forezp/SpringCloudLearning
[用SpringCloud进行微服务架构演进]: https://www.cnblogs.com/zhangs1986/p/10546973.html

[OAuth2 使用Zuul细粒度权限控制笔记]: https://blog.csdn.net/qq_38723394/article/details/106981219

