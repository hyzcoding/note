# Feign

## 简介

## 配置

- pom.xml

  ```xml
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  ```

- application.yml

  ```yaml
  hystrix:
    command:
      default:
        execution:
          isolation:
            thread:
              timeoutInMilliseconds: 60000
  
  #熔断器开启
  feign:
    hystrix:
      enabled: true
    okhttp:
      enabled: true
    httpclient:
      enabled: false
  ```

  

## 使用

 

```java
@FeignClient(name="jungle-article",fallback = ArticleHystrix.class)
@Service
public interface ArticleService {
    /**
     * 上传文件
     * @param file 文件
     * @return 结果
     */
    @PostMapping(name = "/upload",consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    Result<String> upload(@RequestPart("file") MultipartFile file);

    /**
     * 添加
     *
     * @param article 文章
     * @return 结果
     */
    @PostMapping("/add")
    Result<String> add(@RequestBody Article article);
    
}
```

```java
@Component
public class ArticleHystrix implements ArticleService {
    @Override
    public Result<String> upload(MultipartFile file) {
        return Result.fail(HttpStatus.SERVICE_UNAVAILABLE.value(),"服务器异常");
    }

    @Override
    public Result<String> add(Article article) {
        return Result.fail(HttpStatus.SERVICE_UNAVAILABLE.value(),"服务器异常");
    }
}
```