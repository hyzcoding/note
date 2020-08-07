# Spring Data JPA

## 简介

## 配置

- pom.xml

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  ```

  

- application.yml

  ```yaml
  spring:
    datasource:
      url: jdbc:mysql://localhost:3306/mytest
      type: com.alibaba.druid.pool.DruidDataSource
      username: root
      password: root
     driver-class-name: com.mysql.cj.jdbc.Driver # 驱动
    jpa:
      hibernate:
        ddl-auto: update  # 自动更新
      show-sql: true   # 日志中显示sql语句
  ```

- entity

  ```java
  @Entity
  @Getter
  @Setter
  public class Person {
  
      @Id
      @GeneratedValue
      private Long id;
  
      @Column(name = "name", nullable = true, length = 20)
      private String name;
  
      @Column(name = "agee", nullable = true, length = 4)
      private int age;
  }
  ```

- dao

  ```java
  public interface PersonRepository extends JpaRepository<Person, Long> {
  }
  ```

  ```java
  
  @Repository
  public interface PersonRepository extends JpaRepository<Person, Integer>{
      
      public Person findById(Long id);
      
      public Person save(Person person);
      
      @Query(value = "SELECT * FROM person p WHERE name=:name")
      public Person findName(@Param("name") String name);
   
  
  ```

  

