## 简介



## 配置使用

#### 注解

```java
@Scheduled(cron="0/1 * *  * * ? ")		// 每1秒执行一次
@Scheduled(fixedDelay = 1000) 		    // 上一个任务结束到下一个任务开始的时间间隔为固定的1秒
@Scheduled(fixedRate = 1000)			// 每间隔1秒钟就会执行任务
@Scheduled(fixedDelay = 1000, initialDelay = 2000) // 第一次执行的任务将会延迟2秒钟后才会启动
public void testSchedule(){} // 方法不能带参数


@EnableScheduling
public class ExampleApplication{}
```



#### 扩展

- 注入`bean`

```java
    @Bean
    public ThreadPoolTaskScheduler threadPoolTaskScheduler() {
        ThreadPoolTaskScheduler threadPoolTaskScheduler = new ThreadPoolTaskScheduler();
        threadPoolTaskScheduler.setThreadNamePrefix("poolScheduler");
        threadPoolTaskScheduler.setPoolSize(50);
        threadPoolTaskScheduler.initialize();
        return threadPoolTaskScheduler;
    }
```

- cron表达式

  1. 结构

     从左到右（用空格隔开）：秒 分 小时 月份中的日期 月份 星期中的日期 年份

  2. 字段值

     | 字段                     | 允许值                                 | 允许的特殊字符             |
     | ------------------------ | -------------------------------------- | -------------------------- |
     | 秒（*Seconds*）          | 0~59的整数                             | , - * /   四个字符         |
     | 分（*Minutes*）          | 0~59的整数                             | , - * /   四个字符         |
     | 小时（*Hours*）          | 0~23的整数                             | , - * /   四个字符         |
     | 日期（*DayofMonth*）     | 1~31的整数（但是你需要考虑你月的天数） | ,- * ? / L W C   八个字符  |
     | 月份（*Month*）          | 1~12的整数或者 JAN-DEC                 | , - * /   四个字符         |
     | 星期（*DayofWeek*）      | 1~7的整数或者 SUN-SAT （1=SUN）        | , - * ? / L C #   八个字符 |
     | 年(可选，留空)（*Year*） | 1970~2099                              | , - * /   四个字符         |

- 使用

```java
threadPoolTaskScheduler.schedule(()=>System.out.println( System.currentTimeMillis()),new CronTrigger("0 */1 * * * *")) //每1分钟执行一次
```

