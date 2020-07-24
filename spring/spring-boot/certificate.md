# SSL证书

- application.yml

  ```yml
  server:
    port: 443
    ssl:
    	enable: true
      key-store: classpath:ssl/xxx.pfx
      key-store-type: PKCS12
      key-store-password: 123456
  ```

  

- configuration.java

  ```java
    	/**
    	 *
     	 * 设置端口监听跳转
    	 *
    	 */
  	@Bean
      public Connector connector(){
          Connector connector=new Connector("org.apache.coyote.http11.Http11NioProtocol");
          connector.setScheme("http");
          connector.setPort(80);
          connector.setSecure(false);
          connector.setRedirectPort(443);
          return connector;
      }
      /**
       * 配置多个端口号时使用
       * @return servlet工厂类
       */
      @Bean
      public ServletWebServerFactory servletContainer() {
          TomcatServletWebServerFactory tomcat = new TomcatServletWebServerFactory();
          tomcat.addAdditionalTomcatConnectors(createStandardConnector()); // 添加http
          return tomcat;
      }
  ```

  