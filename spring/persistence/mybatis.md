# [mybatis](https://mybatis.org/mybatis-3/zh/index.html)

## 简介

> 1. 封装了jdbc的半ORM（对象关系映射）框架。
> 2. 使用 XML 或注解来配置和映射原生信息， POJO映射成数据库中的记录。
> 3. 通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。

## 导入

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```

## spring配置

```yml
mybatis:
  mapper-locations: classpath:mapper/*.xml
```

## 使用

```java
@Mapper
public interface PersonDao {
    int insertOne(Person person);
    @Results(id = "personResult", value = {
            @Result(property = "id", column = "id", id = true),
            @Result(property = "personName", column = "person_name")
    })
    @Select("<script>SELECT * FROM table_person </script>")
    Person selectOne(Person person);
}
```



## xml映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//ibatis.apache.org//DTD Mapper 3.0//EN"
        "http://ibatis.apache.org/dtd/ibatis-3-mapper.dtd">

<mapper namespace="com.hyzcoding.dao.PersonDao">
</mapper>
```



- #### insert
```xml
<insert id="insertOne"  parameterType="cn.hyzcoding.Person" resultType="Integer">
	insert into table_person (person_name,person_age) values (#{personName},#{personAge})
</insert>
```

- #### select
```xml
<select id="selectOne"  parameterType="cn.hyzcoding.Person"  resultMap="person">
	select * from table_person
      <where>
        <if test="id != null">
             id = #{id}
        </if>
        <if test="personName != null">
            AND person_name like #{personName}
        </if>
        <if test="personAge != null">
            AND person_age like #{personAge}
        </if>
      </where>
</select>
```
- #### update
```xml
<update id="updateOne"  parameterType="cn.hyzcoding.Person">
    <!-- order='AFTER' 更新后数据 -->
    <selectKey keyProperty='id' resultType='java.lang.String' order='BEFORE'>
            select person_name from table_person where id=#{id}
  </selectKey>
	update table_person
    <set>
      <if test="id != null">id=#{id},</if>
      <if test="personName != null">person_name=#{personName},</if>
      <if test="personAge != null">person_age=#{personAge},</if>
    </set>
  where id=#{id}
</update>
```
- #### delete
```xml
<delete id="deleteOne"  parameterType="cn.hyzcoding.Person">
	delete from table_person
      <where>
        <if test="id != null">
             id = #{id}
        </if>
        <if test="personName != null">
            AND person_name like #{personName}
        </if>
        <if test="personAge != null">
            AND person_age like #{personAge}
        </if>
      </where>
</delete>
```
- #### 结果映射

```xml
<resultMap id="person" type="Person">
  <id property="id" column="person_id" />
  <result property="personName" column="person_name"/>
  <result property="personAge" column="person_age"/>
</resultMap>
```



## 注解

[映射注解示例](https://mybatis.org/mybatis-3/zh/java-api.html)



## SqlSession





## 多数据源

- application.yml

```yml
# master 数据源配置
master:
  datasource:
    url: jdbc:sqlserver://localhost:3306/example?characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai&autoReconnect=true
    username: root
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: root
# second 数据源配置
second:
  datasource:
    url: jdbc:mysql://localhost:4306/example?characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai&autoReconnect=true
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
```

- java 配置

```java
@Configuration
@MapperScan(basePackages = MasterDataSourceConfig.PACKAGE, sqlSessionFactoryRef = "masterSqlSessionFactory")
public class MasterDataSourceConfig {
 
    static final String PACKAGE = "com.example.dao.master";
    static final String MAPPER_LOCATION = "classpath:mapper/master/*.xml";
 
    @Value("${master.datasource.url}")
    private String url;
 
    @Value("${master.datasource.username}")
    private String user;
 
    @Value("${master.datasource.password}")
    private String password;
 
    @Value("${master.datasource.driver-class-name}")
    private String driverClass;
 
    @Bean(name = "masterDataSource")
    @Primary
    public DataSource masterDataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setDriverClassName(driverClass);
        dataSource.setJdbcUrl(url);
        dataSource.setUsername(user);
        dataSource.setPassword(password);
        return dataSource;
    }
 
    @Bean(name = "masterTransactionManager")
    @Primary
    public DataSourceTransactionManager masterTransactionManager() {
        return new DataSourceTransactionManager(masterDataSource());
    }
 
    @Bean(name = "masterSqlSessionFactory")
    @Primary
    public SqlSessionFactory masterSqlSessionFactory(@Qualifier("masterDataSource") DataSource masterDataSource)
            throws Exception {
        final SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
        sessionFactory.setDataSource(masterDataSource);
        sessionFactory.setMapperLocations(new PathMatchingResourcePatternResolver()
                .getResources(MasterDataSourceConfig.MAPPER_LOCATION));
        return sessionFactory.getObject();
    }
 
}
```

## 扩展



## 源码解析

