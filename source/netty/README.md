# NETTY
## 简介

## 基础知识

- tcp与udp区别

## 使用

- 服务端

```java
// 线程组, Reactor线程池
// 服务器自身绑定端口监听套接字，接受连接
NioEventLoopGroup boos = new NioEventLoopGroup();
// 接收客户端的 channel，网络读写
NioEventLoopGroup worker = new NioEventLoopGroup();

ChannelFuture channelFuture;
try {
     // 服务启动引导对象，降低服务端开发复杂度
ServerBootstrap serverBootstrap = new ServerBootstrap();
            channelFuture = serverBootstrap.group(boos, worker).channel(NioServerSocketChannel.class)
                    .localAddress(new InetSocketAddress(DEFAULT_PORT))
                // 用于处理网络I/O事件
                    .childHandler(new ChannelInitializer<NioSocketChannel>() {
                        @Override
                        protected void initChannel(NioSocketChannel ch) {
                            // 每一个连接 一个channel
                            ChannelPipeline pipeline = ch.pipeline();
                            // 设置单例 所有客户端同一个 handler
                            // ChannelPipeline 用于保存处理过程需要用到的ChannelHandler和ChannelHandlerContext
                            pipeline.addLast(new HttpServerCodec());// http 编解码
                            pipeline.addLast("httpAggregator", new HttpObjectAggregator(512 * 1024)); // http 消息聚合器,将多个消息转化为一个单一的请求或响应，     512*1024为接收的最大contentlength
                            pipeline.addLast(new HttpHandler());// 请求处理器
//                            pipeline.addLast("nio", serverHandler);
                        }
                        // 阻塞，等待绑定完成
                    }).bind().sync();
    // 进行阻塞，等待服务端链路关闭之后main函数退出
     channelFuture.channel().closeFuture().sync();

 } catch (Exception e) {
     e.printStackTrace();
 }finally{
     boos.shutdownGracefully();
     worker.shutdownGracefully();
}
```

```java
public class HttpHandler extends ChannelInboundHandlerAdapter {
    
     @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ByteBuf buf = (ByteBuf) msg;
        byte[] req = new byte[buf.readableBytes()];
        buf.readBytes(req);
        String body = new String(req, "UTF-8");
         ctx.write(body);
    }
        /**
     * 最后一次读取
     *
     * @param ctx ChannelHandlerContext
     */
    @Override
    public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
        ctx.writeAndFlush(Unpooled.EMPTY_BUFFER).addListener(ChannelFutureListener.CLOSE);
    }

    /**
     * 异常调用
     *
     * @param ctx   ChannelHandlerContext
     * @param cause Throwable
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}
```

- 客户端

```java
public class NettyClient {
    public static void main(String[] args) throws Exception {
        EventLoopGroup group = new NioEventLoopGroup();

        Bootstrap bootstrap = new Bootstrap();
        ClientHandler clientHandler = new ClientHandler();
        ChannelFuture channelFuture = bootstrap.group(group).channel(NioSocketChannel.class)
                .remoteAddress(new InetSocketAddress("127.0.0.1", 8000))
                .handler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel socketChannel) {
                        socketChannel.pipeline().addLast(clientHandler);
                    }
                }).connect().sync();
        channelFuture.channel().closeFuture().sync();
        group.shutdownGracefully().sync();
    }
}
```

```java
@ChannelHandler.Sharable
public class ClientHandler extends SimpleChannelInboundHandler<ByteBuf> {
    /**
     * 收到消息时调用
     * @param channelHandlerContext
     * @param byteBuf
     * @throws Exception
     */
    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf) throws Exception {
        System.out.println(
                "Client received: " + byteBuf.toString(CharsetUtil.UTF_8)
        );
    }

    /**
     * 建立连接时调用
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        ctx.writeAndFlush(Unpooled.copiedBuffer("Netty Netty", CharsetUtil.UTF_8));
    }

    /**
     * 抛出异常时调用
     * @param ctx
     * @param cause
     * @throws Exception
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}
```



## 启动详解



## 扩展

### 核心组件

1. **Channel**

   

2. **EventLoop**

### 特性

- 内存零拷贝