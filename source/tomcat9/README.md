# tomcat
## 简介

- 目录结构及功能

  ```bash
  tomcat
  ├── bin 	# 执行文件
  ├── cert 	# 自定义证书存储目录
  ├── conf 	# 配置文件
  ├── lib		# 依赖jar包
  ├── logs	# 日志
  ├── temp	# 临时存储
  ├── webapps	# 执行war包及web文件目录
  └── work	# 执行时生成class文件
  ```

  

## 使用

- 部署war包

  - 修改配置文件

    - conf/server.xml

      ```xml
       <Host name="localhost"  appBase="webapps"
                  unpackWARs="true" autoDeploy="true">
           <!-- 新加一行 path url路径 docBase 相对webapps路径 或\开头的绝对路径 -->
      	<Context path="/unnamed" docBase="unnamed" reloadable="true"/>
      </Host>
      ```

      

- 配置ssl证书

  - 修改配置文件