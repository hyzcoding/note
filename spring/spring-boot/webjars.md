# webjars

## 简介

前端静态引用资源统一打包管理依赖

## 引用

```xml
<!-- 用于注释管理版本号 --> 
<dependency>
  <groupId>org.webjars</groupId>
  <artifactId>webjars-locator-core</artifactId>
</dependency>
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>xxxx</artifactId>
    <version>xxxxx</version>
</dependency>
```

## 配置

```java
    @Bean
    public WebMvcConfigurer mvcConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addResourceHandlers(ResourceHandlerRegistry registry) {
                registry.addResourceHandler("/webjars/**","/webjars/**/**")
                        .addResourceLocations("/webjars/")
                        .resourceChain(false)
                        .addResolver(new WebJarsResourceResolver())
                        .addResolver(new PathResourceResolver());
            }
        };
    }
```

```properties
spring.resources.static-locations=file:///E://resources/static
```



## 使用

```html
    <link href="/webjars/bootstrap/css/bootstrap.min.css" rel="stylesheet">
    <script src="/webjars/jquery/jquery.min.js"></script>
```

