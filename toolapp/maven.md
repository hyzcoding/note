## 简介

用于构建和管理项目

## 预制环境

  - jdk

## 安装

1. 解压

   ```bash
   > tar -zxvf apache-maven-x.x.x-xx.tar.gz
   > sudo mv ./apache-maven-x.x.x-xx /usr/lib
   ```

   

2. 配置环境变量

   ```bash
   > vi /etc/profile
   ## 行末添加
   export MAVEN_HOME=/usr/lib/apache-maven-x.x.x-xx
   export PATH=${PATH}:${MAVEN_HOME}/bin
   ```

   

3. 测试

   ```bash
   > mvn -v
   ```


 ## 配置

` ${MAVEN_HOME}/conf/setting.xml`

```XML
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 
   http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <!-- 下载至本地地址路径 -->
    <localRepository>/usr/lib/localRepository</localRepository>
    <!-- 远程镜像地址 -->
    <mirrors>
        <mirror>
          <id>alimaven</id>
          <name>aliyun maven</name>
          <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
          <mirrorOf>central</mirrorOf>        
        </mirror>
	</mirrors>
</settings>
```



 ## 使用

#### 命令

```bash
> mvn clean  		 ## 项目清理的处理
> mvn compile 		 ## 执行编译	
> mvn install  		 ## 安装打包的项目到本地仓库
> mvn test 			 ## 测试
> mvn package 		 ## 打包
> mvn clean deploy   ## 拷贝最终的工程包到远程仓库中
```



#### POM

* 文件结构

  ```xml
  <project xmlns = "http://maven.apache.org/POM/4.0.0"
      xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
      http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <!--父项目的坐标 -->
      <parent>
          <!--被继承的父项目的构件标识符 -->
         <artifactId>project</artifactId>
          <!--被继承的父项目的全球唯一标识符 -->
            <groupId>com.companyname.project-group</groupId>
          <!--被继承的父项目的版本 -->
        <version>1.0</version>
          <!-- 父项目的pom.xml文件的相对路径 -->
          <relativePath >../pom.xml</relativePath>
      </parent>
      <!-- maven模型版本 -->
      <modelVersion>4.0.0</modelVersion>
      <!-- 公司或者组织的唯一标志 -->
      <groupId>com.companyname.project-group</groupId>
   
      <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
      <artifactId>project</artifactId>
   
      <!-- 版本号 -->
      <version>1.0</version>
        <packaging>jar</packaging>
        <description>A maven project to study maven.</description>
          <properties>
          <java.version>1.8</java.version>
      </properties>
       <dependencies>
          <!-- https://mvnrepository.com/artifact/io.netty/netty-all -->
          <dependency>
              <groupId>io.netty</groupId>
              <artifactId>netty-all</artifactId>
              <version>4.1.32.Final</version>
          </dependency>
               <!-- 在这里添加你的依赖 -->
      <dependency>
          <groupId>ldapjdk</groupId>  <!-- 库名称，也可以自定义 -->
          <artifactId>ldapjdk</artifactId>    <!--库名称，也可以自定义-->
          <version>1.0</version> <!--版本号-->
          <scope>system</scope> <!--作用域-->
          <systemPath>${basedir}\src\lib\ldapjdk.jar</systemPath> <!--项目根目录下的lib文件夹下-->
      </dependency> 
      </dependencies>
      
      <build>
          <plugins>
              <plugin>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-maven-plugin</artifactId>
              </plugin>
          </plugins>
      </build>                                                                          
  </project>
  ```

  