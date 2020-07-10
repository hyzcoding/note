## 简介



## 导入

```xml
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
        </dependency>
```



## 配置使用

- websocket配置

```java
@Configuration
@ComponentScan
@EnableAutoConfiguration
public class WebSocketConfig implements ServletContextInitializer {
    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        return new ServerEndpointExporter();
    }

    /**
     * 配置tomcat启动项
     * @param servletContext servlet上下文
     */
    @Override
    public void onStartup(ServletContext servletContext) {
        servletContext.addListener(WebAppRootListener.class);
        servletContext.setInitParameter("org.apache.tomcat.websocket.textBufferSize","5242800");
    }

}

```

- websocket类

```java
@ServerEndpoint(value = "/webserver/{user_id}")
@Component
public class SocketService {
                       
    /**
     * 连接开启调用的方法
     *  @param userId 客户端id
     */
    @OnOpen
    public void onOpen(@PathParam("user_id" String userId){
        
    }
    /**
     * 收到客户端消息后调用的方法
     *
     * @param session 会话
     * @param message 客户端发送过来的消息
     */
    @OnMessage
    public void onMessage(String message, Session session) {

    }               
    /**
     * 连接关闭调用的方法
     */
    @OnClose
    public void onClose() {

    }
    /**
     * 异常调用的方法
     */            
    @OnError
    public void onError(Session session, Throwable error) {
          error.printStackTrace();
    }
}
```

