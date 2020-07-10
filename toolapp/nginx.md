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



## 使用

```bash
> nginx/sbin/nginx -s reload            # 重新载入配置文件
> nginx/sbin/nginx -s reopen            # 重启 Nginx
> nginx/sbin/nginx -s stop              # 停止 Nginx
```

