# yarn

## 简介

[中文官网](https://yarn.bootcss.com/)

## 使用

**初始化一个新项目**

```
yarn init
```

**添加依赖包**

```
yarn add <package>
yarn add <package>@<version>
yarn add <package>@<tag>
```

**将依赖项添加到不同依赖项类别中**

分别添加到 `devDependencies`、`peerDependencies` 和 `optionalDependencies` 类别中：

```
yarn add <package> --dev
yarn add <package> --peer
yarn add <package> --optional
```

**升级依赖包**

```
yarn upgrade <package>
yarn upgrade <package>@<version>
yarn upgrade <package>@<tag>
```

**移除依赖包**

```
yarn remove <package>
```

**安装项目的全部依赖**

```
yarn
```

或者

```
yarn install
```

|                          npm | yarn                 |
| ---------------------------: | :------------------- |
|                  npm install | yarn                 |
|     npm install react --save | yarn add react       |
|   npm uninstall react --save | yarn remove react    |
| npm install react --save-dev | yarn add react --dev |
|            npm update --save | yarn upgrade         |

