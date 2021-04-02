# Websocket

## 简介



## 导入

```xml
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
        </dependency>
```



## 原生自定义的WebSocket

- websocket配置

```java
@Configuration
@ComponentScan
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
    public void onOpen(@PathParam("user_id") String userId, Session session){
        
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

- 前端实现

  ```javascript
  var socket;
      socket = new WebSocket("ws://ip:port/endpoint/{id}"); 
      //打开事件  
      socket.onopen = function () {
          console.log("已连接服务器");
          // 发送消息
          socket.send("这是来自客户端的消息" + location.href + new Date());  
      };
      //获得消息事件  
      socket.onmessage = function (msg) {
          console.log("msg", msg.data)
          var data = msg.data
      };
      //关闭事件  
      socket.onclose = function () {
          console.log("Socket已关闭");
      };
      //发生了错误事件  
      socket.onerror = function () {
          alert("Socket发生了错误");
          //此时可以尝试刷新页面
      }
  ```


## spring的websocket

- config

  ```java
  @Configuration
  public class WebSocketConfig implements WebSocketConfigurer {
  @Override
  public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
      registry.addHandler(myHandler(), "/")
          .setAllowedOrigins("*");
  }
  
  @Bean
   public WebSocketHandler myHandler() {
       return new MessageHandler();
   }
  }
  ```

  

- handler

  ```java
  @Component
  public class MessageHandler extends TextWebSocketHandler {
     private List<WebSocketSession> clients = new ArrayList<>();
  
     @Override
     public void afterConnectionEstablished(WebSocketSession session) {
         clients.add(session);
         System.out.println("uri :" + session.getUri());
         System.out.println("连接建立: " + session.getId());
         System.out.println("current seesion: " + clients.size());
     }
  
     @Override
     public void afterConnectionClosed(WebSocketSession session, CloseStatus status) {
         clients.remove(session);
         System.out.println("断开连接: " + session.getId());
     }
  
     @Override
     protected void handleTextMessage(WebSocketSession session, TextMessage message) {
         String payload = message.getPayload();
         Map<String, String> map = JSONObject.parseObject(payload, HashMap.class);
         System.out.println("接受到的数据" + map);
         clients.forEach(s -> {
             try {
                 System.out.println("发送消息给: " + session.getId());
                 s.sendMessage(new TextMessage("服务器返回收到的信息," + payload));
             } catch (Exception e) {
                 e.printStackTrace();
             }
         });
     }
  }
  ```

  

## 基于STOMP协议的WebSocket

### STOMP

STOMP即Simple (or Streaming) Text Orientated Messaging Protocol，简单(流)文本定向消息协议。

### 后台实现

- websocketConfig

  ```java
  @Configuration
  @EnableWebSocketMessageBroker
  public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
  
      @Override
      public void configureMessageBroker(MessageBrokerRegistry config) {
          // 订阅消息 topic 前缀
          config.enableSimpleBroker("/topic");
          // 消息发送controller 前缀
          config.setApplicationDestinationPrefixes("/app");
      }
  	/**
  	* sockjs endpoint
  	*/
      @Override
      public void registerStompEndpoints(StompEndpointRegistry registry) {
          registry.addEndpoint("/gs-guide-websocket").withSockJS();
      }
  
  }
  ```

- controller

  ```java
  @Controller
  public class GreetingController {
      @MessageMapping("/hello")
      @SendTo("/topic/greetings")
      public Greeting greeting(HelloMessage message) throws Exception {
          return new Greeting("Hello, " + message.getName() + "!");
      }
  }
  ```

  

### 前端代码

```javascript
var stompClient;
var socket = new SockJS('/gs-guide-websocket');
    stompClient = Stomp.over(socket);
    stompClient.connect({}, function (frame) {
        console.log('Connected: ' + frame);
        stompClient.subscribe('/topic/greetings', function (greeting) {
            console.log(JSON.parse(greeting.body).content);
        });
    });
    stompClient.send("/app/hello", {}, JSON.stringify({'name': 'aaa'}))

    stompClient.disconnect();

```



## 分布式部署方案





参考文档：

[spring boot 集成 websocket 的四种方式](https://www.cnblogs.com/kiwifly/p/11729304.html)