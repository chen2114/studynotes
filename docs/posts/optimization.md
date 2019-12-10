---
title: vue项目优化
tags: [项目优化]
categories: [项目优化]
---
## 代码包优化
### 屏蔽sourceMap
在 vuejs 项目的 config 目录下有三个文件`dev.env.js`(开发环境配置文件)、`prod.env.js`(上线配置文件)、`index.js`(通用配置文件)。vue-cli 脚手架在上线配置文件会自动设置允许 sourceMap 打包，所以在上线前可以屏蔽 sourceMap。
``` js
// index.js
module.exports = {
  build: {
    productionSourceMap: false, // 将productionSourceMap修改成false
    // https://webpack.js.org/configuration/devtool/#production
    devtool: '#source-map'
  }
}
```
### 对项目代码中的JS/CSS/SVG(*.ico)文件进行gzip压缩
在 vue-cli 脚手架的配置信息中，有对代码进行压缩的配置项，但是首先需要对 compress-webpack-plugin 支持，所以需要通过 npm install --save-dev compression-webpack-plugin 安装。gzip 会对 js、css 文件进行压缩处理；对项目文件进行压缩之后，需要浏览器客户端支持 gzip 以及后端支持 gzip。
``` js
// index.js
module.exports = {
  build: {
    productionGzip: true, // 将productionGzip 设置为true，开启 gzip 压缩
    productionGzipExtensions: ['js', 'css','svg'] // 设置需要进行压缩的什么格式的文件。对于png，jpg，jpeg没有压缩效果
  }
}
```
### 对路由组件进行懒加载
``` js
// 定义一个能够被 Webpack 自动代码分割的异步组件，把组件按组分块
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```
在.babelrc中
``` js
"plugins": ["@babel/plugin-syntax-dynamic-import"]
```
### 使用 CDN 外部加载资源
首先在index.html中引入echarts的外部CDN
``` html
// index.html
<script src="https://cdn.bootcss.com/echarts/4.1.0/echarts.min.js"></script>
```
然后在webpack.base.config.js中,做如下改动
``` js
// webpack.base.config.js
module.exports = {
  externals: {
    // import ElementUI from 'element-ui'
    // 'element-ui': 'ElementUI',
    "echarts": "echarts" // 默认是配置引用的库（这里是echarts）暴露出的全局变量
  },
}
```
删除 main.js 文件中 import 导入的 echarts 文件及 Vue.use(echarts)
## 查看打包分析报告
执行$ npm run build --report后生成分析报告