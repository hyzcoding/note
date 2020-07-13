# package.json

## 简介

项目的清单

## 结构

一个json

```json
{
  "name": "xxx",
  "version": "xxx",
  "description": "xxx",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
  },
  "dependencies": {
    "axios": "^0.19.2",
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.2.0",
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "babel-eslint"
    },
    "rules": {}
  },
  "browserslist": [
    "> 1%",
    "last 2 versions"
  ]
}

```

## 属性解释

- name

  软件包的名称

  ```json
  "name":"xxx"
  ```

- author

  软件包作者名称 

  ```json
  "author":"hyzcoding"
  
  "author":{
      "name":"hyzcoding",
      "email":"hyz951226@gamil.com",
      "url":"http://hyzcoding.cn"
  }
  ```

- contributors

  贡献者 **同`author`属性结构**

- bugs

  github的issues页面地址，用于讨论bug

  ```json
  "bugs":"https://github.com/hyzcoding/note/issues"
  ```
  
- homepage
  软件包主页
  
  ```json
  "homepage":"https://hyzcoding.github.com/note"
  ```
  
- version

  软件包的当前版本

  ```json
  "version":"1.1.0"
  ```

- license

  软件包的许可证

  ```json
  "license": "MIT"
  ```

- keywords

  软件包关键字

  ```json
  "keywords":["note","java","javascript","spring"]
  ```

- description

  软件包描述

  ```json
  "description":"这是软件包描述"
  ```

- repository

  软件包仓库

  ```json
  "repository": "github:nodejscn/node-api-cn"
  ```

- main

  软件包的入口点

  ```json
  "main": "src/main.js"
  ```

- private

  设置为`true`时，不发布至`npm`

  ```json
  "private":true
  ```

- scripts

  运行的node脚本

  ```json
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "test": "npm run unit",
    "lint": "eslint --ext .js,.vue src test/unit",
    "build": "node build/build.js"
  }
  ```

- dependencies

  依赖

- devDependencies

  开发环境依赖
  
- engines
  要运行的 Node.js 或其他命令的版本
  
  ```json
  "engines": {
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0",
    "yarn": "^0.13.0"
  }
  ```
  
- browserslist

  要支持哪些浏览器（及其版本）。 Babel、Autoprefixer 和其他工具会用到它，以将所需的 polyfill 和 fallback 添加到目标浏览器。

  ```json
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ]
  ```
