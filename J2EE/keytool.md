# Keytool

## 简介

> Keytool 是一个Java数据证书的管理工具 ,Keytool将密钥（key）和证书（certificates）存在一个称为keystore的文件中在keystore里，包含两种数据:密钥实体（Key entity）-密钥（secret key）或者是私钥和配对公钥（采用非对称加密）可信任的证书实体（trusted certificate entries）-只包含公钥



>  Keystore，用来存放服务端证书，可以看成一个放key的库，key就是公钥，私钥，数字签名等组成的一个信息。
>
>  truststore是放服务端信任的客户端证书的一个store，里存放的是只包含公钥的数字证书，代表了可以信任的客户端证书，   而keystore是包含私钥的。



> 作为文件形式存在的证书一般有这几种格式：
>
> 　　(1)、带有私钥的证书
>
> ​    由Public Key Cryptography Standards #12，PKCS#12标准定义，包含了公钥和私钥的二进制格式的证书形式，以pfx作 为证书文件后缀名。
>
> 　　(2)、二进制编码的证书
>
> 　　  证书中没有私钥，只包含公钥，DER 编码二进制格式的证书文件，以cer作为证书文件后缀名。
>
> 　　(3)、Base64编码的证书
>
> ​     证书中没有私钥，只包含公钥，BASE64 编码格式的证书文件，也是以cer作为证书文件后缀名。

## 证书



## 使用keytool工具创建证书

- 创建证书yunbo.keystore文件
- 导出证书给客户端安装



```BASH
>keytool -genkey -v -alias server -keyalg RSA -storetype PKCS12  -keystore server.keystore -storepass 123456
>keytool -genkey -v -alias superadmin -keyalg RSA -storetype PKCS12  -keystore superadmin.keystore -storepass 123456
>keytool -genkey -v -alias ca -keyalg RSA -storetype PKCS12  -keystore ca.keystore -storepass 123456
```

```bash
keytool -certreq -alias server -keystore server.keystore -storepass 123456 -file server.csr

keytool -certreq -alias superadmin -keystore superadmin.keystore -storepass 123456 -file superadmin.csr
```



```bash
keytool -gencert -alias ca -keystore ca.keystore -storepass 123456 -infile server.csr -outfile server.cer
keytool -gencert -alias ca -keystore ca.keystore -storepass 123456 -infile superadmin.csr -outfile superadmin.cer
```

```bash
keytool -exportcert -alias ca -keystore ca.keystore -storepass 123456 -file ca.cer
keytool -import -v -file ca.cer -keystore trust.keystore -storepass 123456
```

```bash
keytool -importcert -alias ca -keystore server.keystore -storepass 123456 -file ca.cer
keytool -importcert -alias ca -keystore superadmin.keystore -storepass 123456 -file ca.cer
```

```bash
keytool -importcert -alias server -keystore server.keystore -storepass 123456 -file server.cer
keytool -importcert -alias superadmin -keystore superadmin.keystore -storepass 123456 -file superadmin.cer
```

```bash
keytool -list -keystore server.keystore -storepass 123456 -v
keytool -list -keystore superadmin.keystore -storepass 123456 -v

```



证书安装：

```bash
keytool -import -trustcacerts -alias baidu.com -keystore "%JAVA_HOME%\jre\lib\security\cacerts" -file d:\star.baidu.com.cer -storepass changeit
```

证书查看：

```bash
keytool -list -v -alias baidu.com -keystore "%JAVA_HOME%\jre\lib\security\cacerts" -storepass changeit -keypass changeit
```



证书删除：

```bash
keytool -delete -v -alias baidu.com -keystore "%JAVA_HOME%\jre\lib\security\cacerts" -storepass changeit -keypass changeit
```



[Java 证书已经证书管理]: https://www.cnblogs.com/molao-doing/articles/9687445.html
[spring boot，https，双向ssl认证]: https://www.cnblogs.com/htuao/p/10091458.html

[配置SpringBoot实现TLS双向认证]: https://www.jianshu.com/p/e1aaa5e9de17
[keytool命令制作CA根证书，签发二级证书]: https://www.cnblogs.com/dijia478/p/12103977.html

