# Optional
## 作用

为了解决 null 安全问题的一个 API

## 使用

```java
// 如果存在则获取名称，如果为null 返回unknown
public static String getName(Person p) {
    return Optional.ofNullable(p)
                    .map(p->p.getName())
                    .orElse("Unknown");
}

//of()：为非null的值创建一个Optional
Optional<String> optional = Optional.of(p);
```

