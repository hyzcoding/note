## 简介

spring 快速搭建脚手架，基于SpringFramework。

#### 特性

1.  创建独立的spring应用
2.  内嵌servlet容器(Tomcat、Jetty、Undertow)
3.  提供固化的"starter"依赖，简化构建配置
4.  自动装配Spring或第三方类库
5.  提供运维特性
6.  无代码生成，不需xml配置

#### 导入

- 基本导入

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.3.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>${springboot.version}</version>
    </dependency>
       <!-- spring-boot tomcat 容器依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
<build>
    <plugins>
        <!-- spring-boot maven 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

- 切换servlet容器

```xml
<packaging>war</packaging>
<dependencies>
   <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
       <exclusions>
       	 <exclusion>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-starter-tomcat</artifactId>
         </exclusion>
       </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

#### 配置

- application.yml

```yml
server: 
 port: 8080
```



#### 示例

- 启动

```java
/**
* 只有在不同于启动类包时需要配置scanBasePackages
* @SpringBootApplication 是 Spring Boot 的核心注解，它是一个组合注解，该注解组合了： @Configuration、@EnableAutoConfiguration、@ComponentScan； 若不是用 @SpringBootApplication 注解也可以使用这三个注解代替。
*/
@SpringBootApplication(scanBasePackages="cn.hyzcoding.example")
public class ExampleApplication{
    public static void main(String[] args){
        SpringApplication.run(ExampleApplication.class,args);
    }
}
```

- 组件

```java
@Component("nameComponent") // 组件声明装配
public class NameComponent{
    @Bean // 注入对象 默认为方法名称
    public NameRespository nameRespository(){
        return new NameRespository();
    }
    @Autowired // 导入已经装配注册的对象
    @Qualifier("a") // 细粒度筛选注入的子类
    private Person person;
}
@Service("a")
public class APerson implements Person{}
@Service("b")
public class BPerson implements Person{}
```

- 配置

  

```java
@Configuration // 配置类
// 引入 property文件
@PropertySource(value = {"classpath:config/others.properties"}, encoding = "UTF-8")
// 引入 xml文件中的bean
@ImportReSource(value = {"classpath:config/others.xml"}, encoding = "UTF-8")
public class NameConfiguration{
    
}
```

> other.xml
>
> ```xml
> <beans>
> 	<bean id="nameService" class="com.service.NameService"></bean>
> </beans>
> ```



## 异步

```java
public class NameService{
    /**
     * 其他类对象调用异步才有效
     */
    @Async
    public void printlnName(String name){
        System.out.println(name);
    }
     /**
     * 有返回值的异步调用
     */
    @Async
    public Future<String> getName(String name){
         return new AsyncResult<String>(name);
        // future.isDone() 是否完成
        // future.get() 获取值
    }
}
```





## 无持久层启动

```java
// 排除相关自动装配的类
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
public class ExampleApplication {

    public static void main(String[] args) {
        SpringApplication.run(ExampleApplication.class, args);
    }
}
```



