# WebService

## 简介

**跨编程语言和跨操作系统平台的远程调用技术**

## 交互过程原理

**WebService遵循WSDL（web服务定义语言）/SOAP（简单请求协议）规范的协议通过XML封装数据，然后由Http协议来传输数据。**

- WSDL

  WSDL是用来描述WebService的，它用XML的格式描述了WebService有哪些方法、参数类型、访问路径等等。我们要使用一个WebService肯定首先要获取它的WSDL

- SOAP

  SOAP（简单对象访问协议）是一种基于http的传输协议，用来访问远程服务，在HTTP协议的基础上，把编写成XML的REQUEST参数, 放在HTTP BODY上提交个WEB SERVICE服务器(SERVLET，ASP什么的) 处理完成后，结果也写成XML作为RESPONSE送回用户端， 为了使用户端和WEB SERVICE可以相互对应，可以使用WSDL作为这种通信方式的描述文件，利用WSDL工具可以自动生成WS和用户端的框架文件，SOAP具备把复杂对象序列化捆绑到XML里去的能力。

## 服务搭建

- pom.xml

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
    <relativePath/> 
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web-services</artifactId>
    </dependency>
    <dependency>
        <groupId>wsdl4j</groupId>
        <artifactId>wsdl4j</artifactId>
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
```



- countries.xsd

```java
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:tns="http://spring.io/guides/gs-producing-web-service"
           targetNamespace="http://spring.io/guides/gs-producing-web-service" elementFormDefault="qualified">

    <xs:element name="getCountryRequest">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="name" type="xs:string"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

    <xs:element name="getCountryResponse">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="country" type="tns:country"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

    <xs:complexType name="country">
        <xs:sequence>
            <xs:element name="name" type="xs:string"/>
            <xs:element name="population" type="xs:int"/>
            <xs:element name="capital" type="xs:string"/>
            <xs:element name="currency" type="tns:currency"/>
        </xs:sequence>
    </xs:complexType>

    <xs:simpleType name="currency">
        <xs:restriction base="xs:string">
            <xs:enumeration value="GBP"/>
            <xs:enumeration value="EUR"/>
            <xs:enumeration value="PLN"/>
        </xs:restriction>
    </xs:simpleType>
</xs:schema>
```



## 客户端搭建

```java

```



## 与SOA的关系

SOA不是Web Service，Web Service是目前最适合实现SOA的技术

[Producing a SOAP web service]: https://spring.io/guides/gs/producing-web-service/
[Consuming a SOAP web service]: https://spring.io/guides/gs/consuming-web-service/

