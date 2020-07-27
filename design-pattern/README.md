# 设计模式

## 简介

- 概念

  设计模式的本质是面向对象设计原则的实际运用，是对类的封装性、继承性和多态性以及类的关联关系和组合关系的充分理解。

- 要素

  - 模式名称 -- 助记名
  - 问题 -- 应用环境
  - 解决方案 -- 设计的组成成分、它们之间的相互关系及各自的职责和协作方式
  - 效果 -- 模式的优缺点

- 分类和功能

  | 范围\目的 | 创建型                | 结构型 | 行为型 |
  | --------- | --------------------- | ------ | ------ |
  | 类   | [工厂方法](#工厂方法factory-method) | [适配器](#适配器adapter) |   [解释器](#解释器interpreter)   [模板方法](#模板方法template-method)  |
  | 对象 | [抽象工厂](#抽象工厂abstract-factory) 	 [生成器](#生成器builder)	<br/>	[原型](#原型prototype)	 [单例](#单例singleton) | [桥接](#桥接bridge) [组合](#组合composite) <br/> [装饰](#装饰decorator) [外观](#外观facade) <br/> [享元](#享元flyweight)  [代理](#代理proxy) | [责任链](#责任链chain-of-reponsibility)	[命令](#命令command) [迭代器](#迭代器iterator) <br/>  [中介者](#中介者mediator) 	[备忘录](#备忘录memento) 	[观察者](#观察者observer)	<br/> 	[状态](#状态state)		[策略](#策略strategy)		[访问者](#访问者visitor) |

  

## 创建型模式

- #### 抽象工厂(Abstract Factory)

  ```java
  
  ```

  

- ####  生成器(Builder)

- ####  工厂方法(Factory Method)

- ####  原型(Prototype)

  ```java
  /**
   *
   * 克隆拷贝对象
   * (1)浅拷贝：我们只拷贝对象中的基本数据类型（8种），对于数组、容器、引用对象等都不会拷贝
   * (2)深拷贝：不仅能拷贝基本数据类型，还能拷贝那些数组、容器、引用对象等
   *
   */
  public class A implements Cloneable{
     private B b;
      
     @Override
     protected A clone(){
         A a = null;
         try{
             B bb = b.clone();
             a = (A) super.clone();
             a.setB(bb);
         }catch (CloneNotSupportedException e){
              e.printStackTrace();
          }
     }
  }
  ```

  

- ####  单例(Singleton)

  ```java
  /**
   *
   * 节省内存资源、保证数据内容的一致性
   *
   */
  // 1. 懒汉式 线程安全，调用效率不高 可以延时加载
      public class A{
          
          private static volatile A a= null;
          
          private A();
         // 双重检查锁在多线程并发时，可能会因为JVM指令重排出问题  volatile 解决此问题
          public static A getInstance(){
             if(a == null){
                 synchronized(A.class){
                      if (a == null){
  						a = new A();	
              		}
                 }
             }
              return a;
          }
      }
  // 2. 饿汉式 线程安全，调用效率高 不能延时加载
  	public class B{
          private static final B b = new B();
          
          private B(){}
          
          public static B getInstance(){
              return B;
          }
      }
  ```

  

## 结构型模式

- ####  适配器(Adapter)
- ####  桥接(Bridge)
- ####  组合(Composite)
- ####  装饰(Decorator)
- ####  外观(Facade)
- ####  享元(Flyweight)
- ####  代理(Proxy)

## 行为模式

- ####  责任链(Chain of Reponsibility)
- ####  命令(Command)
- ####  解释器(Interpreter)
- ####  迭代器(Iterator)
- ####  中介者(Mediator)
- ####  备忘录(Memento)
- #### 观察者(Observer)
- #### 状态(State)
- #### 策略(Strategy)
- #### 模板方法(Template Method)
- #### 访问者(Visitor)

[23种设计模式全面解析]: http://c.biancheng.net/design_pattern/

[Design patterns implemented in Java]: https://github.com/iluwatar/java-design-patterns

