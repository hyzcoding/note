# Node.js

## 简介

一个开源与跨平台的` JavaScript` 运行时环境。

[中文网地址](http://nodejs.cn/)

## 安装

1. [官网下载](https://nodejs.org/zh-cn/download/)
2. 解压安装
3. 检查版本 `> node --version`

## npm

#### 简介

`npm` 是 Node.js 标准的软件包管理器。

[**Yarn**](https://yarnpkg.com/en/) *是 npm 的一个替代选择。*

#### 使用

- 安装依赖

  ```bash
  > npm install -g <package> # 全局安装
  
  > npm install # 安装项目 package.json中的所有依赖
  > npm install <package> # 安装指定依赖至项目
  > npm install <package>@<version> # 安装指定版本
  ## --save 添加至dependencies
  ## --save-dev 添加至devDependencies
  > npm update # 更新
  > npm update <package> # 更新指定包
  ```

- 运行任务

    ```bash
> npm run <task> # 运行指定任务
    ```
    
    ```javascript
        "scripts": {
            "serve": "vue-cli-service serve",
            "build": "vue-cli-service build",
            "lint": "vue-cli-service lint"
        },
    ```
    
- 查看已安装

    ```bash
    > npm list # 查看已安装
    > npm list -g # 查看全局已安装
    > npm view <package> version # 查看包最新版本
    ```

- 卸载依赖

    ```bash
    > npm uninstall <package>
    > npm uninstall -g <package>
    ## -S 或 --save 移除package.json中的dependencies引用
    ## -D 或 --save-dev 移除package.json中的devDependencies引用
    ```

#### 淘宝镜像

```bash
## 安装
> npm install -g cnpm --registry=https://registry.npm.taobao,org
> cnpm install <package>
```



#### npx

 // todo

#### 本地安装位置

`node_modules`文件夹中