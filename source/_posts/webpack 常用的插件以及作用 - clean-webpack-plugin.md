---
title: webpack 常用的插件以及作用 - clean-webpack-plugin
date: 2019-06-19 22:43:21
author: huangkuiwei
img: http://pic2.zhimg.com/v2-ea57e2d2c487d0af20c93c4be5f25b5f_1200x500.jpg
tags: 
    - JavaScript
    - Node.js
    - Webpack
---
`clean-webpack-plugin`是我们在配置webpack自动化构建工具的时候常用的创建，它的作用就是每次重新打包的时候都会先删除掉`dist`目录下的文件再生成新的文件。
#### 1. 安装
```html
npm install clean-webpack-plugin -D
```
这个插件只有我们在开发的时候才会用到，到了线上不会用到。

#### 2. 用法
```javascript
const CleanWebpackPlugin = require('clean-webpack-plugin');   // 引进插件

module.exports = {
  // ...代码
  plugin: [
    new CleanWebpackPlugin('dist')
  ]
}
```
上面的代码在我之前用的项目里面没有问题，clean-webpack-plugin 用的是1.0.X版本，今天我升级到了最新的版本：3.0，发现上面的代码报错了(`“CleanWebpackPlugin is not a constructor”`)，我查了官方的收藏发现我这旧版本的写法已经不兼容新版本了，新版本的写法如下：
```javascript
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
  // ...
  plugins: [
    new CleanWebpackPlugin({
      // ...相关配置
    })
  ]
}
```
