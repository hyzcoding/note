## 简介



## 预制环境

```bash
> yum install gcc-c++ 				# gcc
> yum install -y pcre pcre-devel	# PERE
> yum install -y zlib zlib-devel	# zlib
> yum -y install pcre  pcre-devel zlib  zlib-devel openssl openssl-devel # openssl
```



## 安装

```bash
> tar zxvf nginx-x.x.x.tar.gz
> cd nginx-x.x.x
> ./configure
> make
> make install
```



```bash
> nginx/sbin/nginx -v
```



## 配置

```bash
> vi nginx/conf/nginx.conf
```

- upstream

  - upstream参数

    | 参数           | 详细信息                                 |
    | -------------- | ---------------------------------------- |
    | `service`      | 反向服务器ip地址及端口或域名地址         |
    | `weight`       | 权重（默认是1，权重越高请求越多）        |
    | `maxfails`     | 重试次数                                 |
    | `fail_timeout` | 超时时间                                 |
    | `backup`       | 备用服务(当其他不存活时，全部转发至备用) |
    | `max_conns`    | 允许最大连接数                           |
    | `down`         | 临时停用主机                             |
    | `slow_start`   | 节点恢复时，不立刻加入                   |

  - 配置示例

  ```nginx
  upstream domainname {
      server 192.168.0.2:9090 max_conns=800 weight=2;
      server 192.168.0.3:9090 max_fails=3 failtimeout=30s weight=5; 
     # max_fails=3 fail_timeout=30s代表在30秒内请求某一应用失败3次，认为该应用宕机，后等待30秒，这期间内不会再把新请求发送到宕机应用，而是直接发到正常的那一台，时间到后再有请求进来继续尝试连接宕机应用且仅尝试1次，如果还是失败，则继续等待30秒...以此循环，直到恢复。
      server 192.168.0.4:9090 weight=2 backup;
  }
  
  server {
      listen 80;
      server_name domainname;#域名
      location /{
           proxy_pass http://domainname;  #域名
      }
              
  }
  ```

  

- 负载均衡算法/策略

  - 算法

  | 算法          | 详细信息                                             | 使用场景                        |
  | ------------- | ---------------------------------------------------- | ------------------------------- |
  | `round_robin` | 轮询，默认负载均衡算法，配合`weight`基于权重进行轮询 | 默认                            |
  | `ip_hash`     | 根据客户端ip进行负载均衡                             | 保持session 一致性              |
  | `url_hash`    | 根据客户端请求url进行负载均衡                        | 静态资源缓存,节约存储，加快速度 |
  | `hash key`    | 对某一个key进行hash                                  | 通过lua设置一致性hashkey        |
  | `least_conn`  | 负载到最少连接的主机                                 |                                 |
  | `least_time`  | 负载到响应最快的主机                                 |                                 |

  - 示例

  ```nginx
  upstream domainname {
      ip_hash;
      # round_robin;
      # hash $consistent_key consistent;
      # hash $url;
      server 192.168.0.3:9090 weight=1; # ip 地址 和 权重(默认是1，权重越高请求越多)
      server 192.168.0.4:9090 weight=2;
  }
  server {
      listen 80;
      server_name domainname;#域名
      location /{
           set $consistent_key $arg_cat;
          if($consistent_key =""){
              set $consistent_key $request_uri;
          }
           proxy_pass http://domainname;  #域名
      }
              
  }
  ```

- 心跳检查（需要集成`nginx_upstream_check_module`模块）

  - 参数

  | 参数                      | 详细信息                         |
  | ------------------------- | -------------------------------- |
  | `interval`                | 检测间隔时间                     |
  | `fall`                    | 检测失败指定次数后，标识为不存活 |
  | `rise`                    | 检测成功指定次数后，标识为存活   |
  | `timeout`                 | 检测请求超时时间                 |
  | `type`                    | 检测类型 , tcp/http              |
  | `check_http_send`         | 检测时发送的http请求内容         |
  | `check_http_expect_alive` | 返回匹配状态码，认为存活         |

  - 示例

  ```nginx
  upstream domainname {
      server 192.168.0.3:9090 weight=1;
      server 192.168.0.4:9090 weight=2;
   	#  check interval=3000 rise=1 fall=3 timeout=2000 type=tcp;
      check interval=3000 rise=1 fall=3 timeout=2000 type=http;
      check_http_send "HEAD /status HTTP/1.1 \r\n\r\n";
      check_http_expect_alive http_2xx http_3xx;
  }
  
  ```

  

- location配置

  - 语法

    ```nginx
    location   [ = | ~ | ~* | ^~ | @]   /uri/     { configuration }
    ```

  - 示例

    ```nginx
    location ~ ^/backend/(.*)$ {
        # 失败重试 配置指定返回状态码进行下一台主机重试
        proxy_next_upstream error timeout http_500 http_502 http_504;
        proxy_next_upstream_timeout 5s;
        proxy_next_upstream_tries 2;
        
        # 请求上级都用GET请求
        proxy_method GET;
        
        #不传输请求体
        proxy_pass_request_body off;
        
        #不传输请求头
        proxy_pass_request_headers off;
        
        # 设置响应不传输客户端
        proxy_hide_header Vary;
        # 支持keep-alive
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        # 传递请求头
        proxy_set_header Host $host;
        proxy_pass http://localhost:8001;
    }
    ```

    

    

- 日志



**整体结构**

```nginx
http{
    include mine.types;
    default_type application/octect-stream;
    
    sendfile on;
    
    keepalive_timeout 65;
    server {
        listen 443 ssl;
        server_name hyzcoding.com;
        ssl_certificate      cert.pem; # 路径为当前配置文件路径
        ssl_certificate_key  cert.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        client_max_body_size 500M;
        location / {
             proxy_pass http://localhost:8081;
        }
    }
}
```



## 使用

```bash
> nginx/sbin/nginx -s reload            # 重新载入配置文件
> nginx/sbin/nginx -s reopen            # 重启 Nginx
> nginx/sbin/nginx -s stop              # 快速停止 Nginx
> nginx/sbin/nginx -s quit 				# 退出 Nginx
```

## 扩展

- cgi与fastcgi

- 优点

  - 跨平台、配置简单。
  - 非阻塞、高并发连接： 处理 2-3 万并发连接数
  - 内存消耗小
  - 成本低廉，且开源。
  - 稳定性高，宕机的概率非常小。

- 与apache不同点

- 反向代理服务器优点

- ngx_http_upstream_module

- 错误状态码

- 模块安装

- 四层负载均衡

  ```nginx
  stream {
      upstream mysql_backend {
          
      }
      server {
          
      }
  }
  ```

  