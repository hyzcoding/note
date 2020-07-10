## 查询

* ### 内存占用

  `top` ---- 查看用户实时进程、IO信息 

  ​			`q`键退出

* ### 进程 `ps` 命令

  ```bash
  > ps -ef|grep ${name}
  ```

* ### 端口占用
  `net-tools` 

  ```bash
  > netstat -anp|grep ${port}
  ```

* ### 文件

  ```bash
  > find . -name '*.txt' # 查找当前目录下名称匹配的所有文件
  > ll ${pwd} # 查看目录详细信息
  > ls ## 查看基本信息
  ```

## 操作

* ### 抓包

  ``` bash
  > ipconfig ##查看网口
  > tcpdump -i eth0 port 8080 host 192.168.0.1 -w /home/hyz/tcp.pcap
  ```

  

* ### 远程

  ```bash
  > systemctl start sshd ## 启动
  > ssh -l username host ## 登录
  > ssh root@host ## 登录
  > scp root@host /filepath /filepath2 ## 远程复制
  ```


## 权限

* ### chown

  更改用户组权限
  
  ``` bash
  > chown root: foo ## 在用户名后跟冒号[:]运行该命令将同时设置用户和组所有权。
  ```

* ### chmod

  更改文件读写权限

  ``` bash
  > chmod +X filename 
  > chmod 777 -R filepath
  ```

  