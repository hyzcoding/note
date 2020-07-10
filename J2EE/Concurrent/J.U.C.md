## 简介

`java 5 ` 提供的并发工具类`java.util.concurrent`

## 功能

用于保持高并发(读多写少)下的同步接口，并发容器类，异步IO，简单的任务框架

## 使用

| 类名                  | 实现接口 | 描述                                     |
| --------------------- | -------- | ---------------------------------------- |
| CopyOnWriteArrayList  | List     | 线程安全的ArrayList                      |
| CopyOnWriteArraySet   | Set      | 线程安全的HashSet                        |
| ConcurrentSkipListSet | Set      | 线程安全的TreeSet                        |
| ConcurrentHashMap     | Map      | 线程安全的HashMap                        |
| ConcurrentSkipListMap | Map      | 线程安全的TreeMap                        |
| ArrayBlockingQueue    | Queue    | 线程安全的有界的阻塞队列                 |
| LinkedBlockingQueue   | Queue    | 单向链表实现的(指定大小)阻塞队列         |
| LinkedBlockingDeque   | Queue    | 双向链表实现的(指定大小)双向并发阻塞队列 |
| ConcurrentLinkedQueue | Queue    | 单向链表实现的无界队列                   |
| ConcurrentLinkedDeque | Queue    | 双向链表实现的无界队列                   |

## 源码解析

