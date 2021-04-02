# Spring

## 参数

```bash
java -jar app.jar -Dspring.profiles.active=dev --server.module

# 测试环境

java -jar app.jar -Dspring.profiles.active=dev
java -jar app.jar -Dspring.profiles.active=dev --server.port=8060

# 生产环境

java -jar  -Dspring.profiles.active=prod,module  home-0.0.1-SNAPSHOT.jar


java -jar  -Dspring.profiles.active=prod,module -Dlogging.level.com.hyzcoding.example.dao=DEBUG home-0.0.1-SNAPSHOT.jar
			
java -jar  -Dspring.profiles.active=prod,module -Dserver.port=80 home-0.0.1-SNAPSHOT.jar

```



## SpringMVC



```
DispatcherServlet
```