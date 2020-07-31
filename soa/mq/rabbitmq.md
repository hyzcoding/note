## 安装



- docker

  ```bash
  > sudo docker pull rabbitmq：management
  ```

  

## 配置



## 使用

- docker运行

  ```bash
  docker run -dit --name Myrabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin -p 15672:15672 -p 5672:5672 rabbitmq:management
  ```

  

## SpringBoot

- pom.xml

  ```xml
         <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-amqp</artifactId>
          </dependency>
  ```

  

- application.yml

  ```yml
  spring:
    application:
      name: jungle-document
    rabbitmq:
      host: localhost
      port: 5672
      username: test
      password: 123456
      queue: article
  ```

  

- producer

  - config

  ```java
      @Bean
      public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
          RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
          rabbitTemplate.setMessageConverter(new Jackson2JsonMessageConverter());
          return rabbitTemplate;
      }
  ```

  - sender

  ```java
  @Component
  public class ArticleSender {
      @Autowired
      private AmqpTemplate rabbitAmqpTemplate;
  
      //queue 队列名称
      @Value("${spring.rabbitmq.queue}")
      private String queue;
  
      /*
       * 发送消息的方法
       */
      public void send(Article article){
          /**
           * convertAndSend - 转换并发送消息的template方法。
           * 是将传入的普通java对象，转换为rabbitmq中需要的message类型对象，并发送消息到rabbitmq中。
           * 参数一：交换器名称。 类型是String
           * 参数二：路由键。 类型是String
           * 参数三：消息，是要发送的消息内容对象。类型是Object
           */
          this.rabbitAmqpTemplate.convertAndSend(this.queue, article);
      }
  }
  ```

  

- consumer

  - config

  ```java
      @Bean
      public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
          RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
          rabbitTemplate.setMessageConverter(new Jackson2JsonMessageConverter());
          return rabbitTemplate;
      }
      @Bean
      public SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory(ConnectionFactory connectionFactory) {
          SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
          factory.setConnectionFactory(connectionFactory);
          factory.setMessageConverter(new Jackson2JsonMessageConverter));
          return factory;
      }
  ```

  - receiver

  ```java
  @Component
  @RabbitListener(queues = "article", containerFactory="rabbitListenerContainerFactory")
  @Slf4j
  public class ArticleReceiver {
      @Autowired
      ArticleService articleService;
  
      @RabbitHandler
      public void process(Article article){
          articleService.insertArticle(article);
      }
  }
  ```

## 扩展

