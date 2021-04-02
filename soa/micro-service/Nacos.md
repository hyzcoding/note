# Nacos

## 简介

## 功能

## 安装

## 与框架结合

- gRPC

  - pom

    ```xml
        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>nacos-config-spring-boot-starter</artifactId>
            <version>0.2.7</version>
            <exclusions>
                <exclusion>
                    <groupId>com.alibaba.nacos</groupId>
                    <artifactId>nacos-client</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>com.alibaba.nacos</groupId>
            <artifactId>nacos-client</artifactId>
            <version>1.2.0</version>
            <exclusions>
                <exclusion>
                    <groupId>com.google.guava</groupId>
                    <artifactId>guava</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    ```

    

  - utils文件

   ```java
      public class NacosUtils {
      
          public static Properties buildNacosProperties(URI uri, Properties properties) {
              StringBuilder serverAddrBuilder =
                      new StringBuilder(uri.getHost()) // Host
                              .append(":")
                              .append(uri.getPort()); // Port
      
              String serverAddr = serverAddrBuilder.toString();
              properties.put(SERVER_ADDR, serverAddr);
              return properties;
          }
      }
   ```

    - Application

      ```java
      @SpringBootApplication
      public class DemoApplication {
          private int port;
          private Server server;
          @Autowired
          RegisterService registerService;
          
          private MutableHandlerRegistry handlerRegistry ;
      
          private ServerServiceDefinition serverServiceDefinition;
          
          public static void main(String[] args) {
              SpringApplication.run(RegisterApplication2.class, args);
          }
          @PostConstruct
          public void init() throws IOException, NacosException {
              this.port = 10003;
              URI uri = URI.create("http://localhost:8848");
              Properties properties = NacosUtils.buildNacosProperties(uri,new Properties());
              NamingService namingService = NacosFactory.createNamingService(properties);
              handlerRegistry= new MutableHandlerRegistry();
              BindableService bindableService = this.registerService;
              serverServiceDefinition = bindableService.bindService();
              namingService.registerInstance(serverServiceDefinition.getServiceDescriptor().getName(), createInstance());
              handlerRegistry.addService(serverServiceDefinition);
              this.server = NettyServerBuilder.forPort(this.port).fallbackHandlerRegistry(handlerRegistry)
                      .sslContext(GrpcSslContexts.configure(serverSslContext).build())
                      .build();
              start();
          }
      
          private Instance createInstance() {
              Instance instance = new Instance();
              instance.setIp("localhost");
              instance.setPort(port);
              return instance;
          }
              private void start() throws IOException {
              server.start();
              log.info("Server started, listening on " + port);
              Runtime.getRuntime().addShutdownHook(new Thread() {
                  @Override
                  public void run() {
                      // Use stderr here since the logger may has been reset by its JVM shutdown hook.
                      System.err.println("*** shutting down gRPC server since JVM is shutting down");
                      this.interrupt();
                      System.err.println("*** server shut down");
                  }
              });
          }
      }
      ```

      

- Dubbo

- Spring Cloud