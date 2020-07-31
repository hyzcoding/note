# lombok

## 简介

## 使用

- pom.xml

  ```xml
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
  </dependency>
  ```

  

- java

  ```java
  @Getter
  @Setter
  @ToString
  @Slf4j
  @NoArgsConstructor
  @AllArgsConstructor
  /**
   * @Data: 自动为所有字段添加@ToString, @EqualsAndHashCode, @Getter方法，为非final字段添加@Setter,和@RequiredArgsConstructor
   */
  public class Sample {
      
      @NonNull private String aa;
  }
  ```

  ```java
  @Cleanup InputStream in = new FileInputStream(args[0]);
  @Cleanup OutputStream out = new FileOutputStream(args[1]);
  ```

  `@Builder`：用在类、构造器、方法上，为你提供复杂的 builder APIs，让你可以像如下方式一样调用Person.builder().name("xxx").city("xxx").build()；

