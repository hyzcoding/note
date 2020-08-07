# NioEventLoop

## 简介

## [Reactor线程模型](J2EE/Concurrent/Reactor线程模型)

## Netty线程模型

> 1. 根据启动参数选择不同的Reactor线程模型。
>
> 2. 一个NioEventLoopGroup对应多个NioEventLoop,一个NioEventLoop有一个Thread和Selector。
> 3. boss线程组的acceptor线程处理accept事件,生成NioServerChannel 注册到 work线程组的某个 selecor上，由work线程处理连接上的读写事件。
>
> 4. selector负责轮询可读，可写事件，当有可读可写事件时调用channelpipeline的handler进行处理。
>
> 5. netty采用了串行化设计,channel的消息从读取、编码以及后续Handler的执行，始终都由一个IO线程EventLoop负责，减少线程上下文切换带来的开销，避免并发风险。

对比reactor模式

- NioEventLoop 相当于 reactor中的 Initiation Dispatcher （初始分发器）
- ChannelHandler 相当于reactor中的 Event Handler （事件处理者）
- Selector 相当于reactor中的 Synchronous Event Demultiplexer （同步事件分离器）
- NioSocketChannel 相当于 ractor中的 handle （句柄或描述符）
- netty的线程模型类似于reactor的主从多reactor模式，只是<span style='color:red'>从reactor其实有多个</span>，一个NioEventLoop相当于一个reactor，负责一部分channel。而reactor的线程池对应于netty的用户自定义线程池。

[Netty线程模型总结]: https://blog.csdn.net/chenyun19890626/article/details/100991204

