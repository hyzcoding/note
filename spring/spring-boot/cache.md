## 简介

spring 缓存机制

## 导入

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```



## 配置

- application.java

```java
@SpringBootApplication
@EnableCaching  //开启缓存
public class ExampleApplication{

    public static void main(String[] args) {
        SpringApplication.run(ExampleApplication.class, args);
    }

}
```

- redis配置

```java
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory factory) {

        StringRedisTemplate template = new StringRedisTemplate(factory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }

    @Bean
    public RedisCacheManager redisCacheManager(RedisTemplate redisTemplate) {
        RedisCacheWriter redisCacheWriter = RedisCacheWriter.nonLockingRedisCacheWriter(redisTemplate.getConnectionFactory());
        RedisCacheConfiguration redisCacheConfiguration = RedisCacheConfiguration.defaultCacheConfig()
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(redisTemplate.getValueSerializer()));
        return new RedisCacheManager(redisCacheWriter, redisCacheConfiguration);
    }


}

```



## 使用

```java
@Cacheable(value = "person" ,key = "#p0.id", condition = "#p0.age > 18") // 设置缓存
public Person selectOne(Person person) {
    return personDao.selectOne(uid);
}
@CachePut(value = "person" ,key = "#p0.id") // 更新缓存
public Person update(Person person) {
    return personDao.selectById(id);
}

@Cacheable(value = "person" ,key = "#id") // 设置缓存
public Person selectById(String id) {
    return personDao.selectById(id);
}

@CacheEvict(value="person",key="#id") // 清除缓存
public void deleteById(String id) {
    personDao.deleteById(id);
}

@CacheEvict(value="person",allEntries=true) // 清除缓存
public void delectAll() {
    personDao.deleteAll();
}
```



