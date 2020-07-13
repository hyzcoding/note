# vue.config.js

## 简介

`vue.config.js` 是一个可选的配置文件，如果项目的 (和 `package.json` 同级的) 根目录中存在这个文件，那么它会被 `@vue/cli-service` 自动加载。你也可以使用 `package.json` 中的 `vue` 字段，但是注意这种写法需要你严格遵照 JSON 的格式来写。

#### 整体`json`

```js
// vue.config.js
module.exports = {
  // 选项...
}
```

#### 配置属性

- ##### publicPath

  部署应用包时的基本 URL ，默认`publicPath:'/'`

- ##### outputDir

  `build`生成的生产环境构建文件的目录，默认`outputDir:'dist'`

- ##### assetsDir

  放置生成的静态资源 (js、css、img、fonts) 的 (相对于 `outputDir` 的) 目录，默认`assetsDir:''`

- ##### indexPath

  指定生成的 `index.html` 的输出路径 (相对于 `outputDir`)。也可以是一个绝对路径，默认`indexPath:'index.html'`

- ##### filenameHashing

  文件名hash，默认`filenameHashing:true`

- ##### pages

  多页面下构建项目

  ```json
  module.exports = {
    pages: {
      index: {
        // page 的入口
        entry: 'src/index/main.js',
        // 模板来源
        template: 'public/index.html',
        // 在 dist/index.html 的输出
        filename: 'index.html',
        // 当使用 title 选项时，
        // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
        title: 'Index Page',
        // 在这个页面中包含的块，默认情况下会包含
        // 提取出来的通用 chunk 和 vendor chunk。
        chunks: ['chunk-vendors', 'chunk-common', 'index']
      },
      // 当使用只有入口的字符串格式时，
      // 模板会被推导为 `public/subpage.html`
      // 并且如果找不到的话，就回退到 `public/index.html`。
      // 输出文件名会被推导为 `subpage.html`。
      subpage: 'src/subpage/main.js'
    }
  }
  ```

  **封装扩展**

  ```js
  let glob = require('glob')
  
  //配置pages多页面获取当前文件夹下的html和js
  function getEntry(globPath) {
      let entries = {}, tmp, htmls = {};
  
      // 读取src/pages/**/底下所有的html文件
      glob.sync(globPath+'html').forEach(function(entry) {
          tmp = entry.split('/').splice(-3);
          htmls[tmp[1]] = entry
      })
  
      // 读取src/pages/**/底下所有的js文件
      glob.sync(globPath+'js').forEach(function(entry) {
          tmp = entry.split('/').splice(-3);
          entries[tmp[1]] = {
              entry,
              template: htmls[tmp[1]] ? htmls[tmp[1]] : 'index.html', //  当前目录没有有html则以共用的public/index.html作为模板
              filename:tmp[1] + '.html'   //  以文件夹名称.html作为访问地址
          };
      });
      console.log(entries)
      return entries;
  }
  let htmls = getEntry('./src/pages/**/*.');
  module.exports = {
      pages:htmls,
      publicPath: './',           //  解决打包之后静态文件路径404的问题
      outputDir: 'output',        //  打包后的文件夹名称，默认dist
      devServer: {
          open: true,             //  npm run serve 自动打开浏览器
          index: '/page1.html'    //  默认启动页面
      }
  }
  ```

- ##### lintOnSave

  设置为 `true` 或 `'warning'` 时，`eslint-loader` 会将 lint 错误输出为编译警告。默认情况下，警告仅仅会被输出到命令行，且不会使得编译失败。

- ##### webpack 相关

  [参考官网说明](https://cli.vuejs.org/zh/guide/webpack.html)

  - **简单配置**

    ```json
    configureWebpack: {
        plugins: [
          new MyAwesomeWebpackPlugin()
        ]
      }
    ```

    ```json
     configureWebpack: config => {
        if (process.env.NODE_ENV === 'production') {
          // 为生产环境修改配置...
        } else {
          // 为开发环境修改配置...
        }
      }
    ```

    

  - **链式操作**

    列出所有规则和插件的名字：

    ```bash
    vue inspect --rules
    vue inspect --plugins
    ```

    

- ##### devServer

  ```json
  module.exports = {
    devServer: {
      proxy: {
        '/api': {
          target: '<url>',
          ws: true,
          changeOrigin: true
        },
        '/foo': {
          target: '<other_url>'
        }
      }
    }
  }
  ```

  