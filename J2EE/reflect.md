# 反射与代理
## 反射
#### 定义

JAVA反射机制：是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。



#### 相关类

| 类名          | 用途                                             |
| ------------- | ------------------------------------------------ |
| Class类       | 代表类的实体，在运行的Java应用程序中表示类和接口 |
| Field类       | 代表类的成员变量（成员变量也称为类的属性）       |
| Method类      | 代表类的方法                                     |
| Constructor类 | 代表类的构造方法                                 |

* class

```java
Class clazz = Class.forName("com.example.Exeample.class");
clazz.getName(); // 获得类的完整路径名字
clazz.getPackage(); // 获得类的包
clazz.getSimpleName(); // 获得类的名字
clazz.getDeclaredClasses(); // 获取继承的类和接口
Example example = clazz.newInstance(); // 创建类的实例
```



## 静态代理



## 动态代理

```java
  Object object = Proxy.newProxyInstance(
                //和目标对象的类加载器保持一致
                clazz.getClassLoader(),
                //目标对象实现的接口，因为需要根据接口动态生成对象
                new Class[]{clazz},
                //InvocationHandler:事件处理器，即对目标对象方法的执行
                (Object proxy, Method method, Object[] args) -> {
   
                    return null;
                });
```

