## 简介

[mybatis](https://mybatis.org/mybatis-3/zh/index.html)

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

## xml映射文件

- #### insert
```xml
<insert id="insertOne"  parameterType="cn.hyzcoding.Person" resultType="Integer">
	insert into table_person (person_name,person_age) values (#{personName},#{personAge})
</insert>
```

- #### select
```xml
<select id="insertOne"  parameterType="cn.hyzcoding.Person"  resultMap="person">
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
<update id="insertOne"  parameterType="cn.hyzcoding.Person">
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
<delete id="insertOne"  parameterType="cn.hyzcoding.Person">
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

## 源码解析

