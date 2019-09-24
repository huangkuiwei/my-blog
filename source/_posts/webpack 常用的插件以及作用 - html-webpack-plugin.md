---
title: webpack 常用的插件以及作用 - html-webpack-plugin
date: 2019-05-08 23:12:34
author: huangkuiwei
img: http://pic2.zhimg.com/v2-ea57e2d2c487d0af20c93c4be5f25b5f_1200x500.jpg
tags: 
    - JavaScript
    - Node.js
    - Webpack
---
`html-webpack-plugin`插件的主要作用有两个：
> 1. 为html文件中引入的外部资源如script、link动态添加每次compile后的hash，防止引用缓存的外部文件问题
> 2. 可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置N个html-webpack-plugin可以生成N个页面入口

**用法示例**
```javascript
// webpack.config.js 配置
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 插件选项
plugins: [
  new HtmlWebpackPlugin({ // 打包输出HTML
    title: 'Hello World app',
    minify: { // 压缩HTML文件
      removeComments: true, // 移除HTML中的注释
      collapseWhitespace: true, // 删除空白符与换行符
      minifyCSS: true// 压缩内联css
    },
    filename: 'index.html',
    template: 'index.html'
  }),
]
```
**相关属性**
##### 1. title：生成html文件的标题
##### 2. filename：输出的html的文件名称
##### 3. template：html模板所在的文件路径
> 1. html模板所在的文件路径
> 2. 根据自己的指定的模板文件来生成特定的 html 文件。这里的模板类型可以是任意你喜欢的模板，可以是 html, jade, ejs, hbs, 等等，但是要注意的是，使用自定义的模板文件时，需要提前安装对应的 loader， 否则webpack不能正确解析。
> 3. 如果你设置的 title 和 filename于模板中发生了冲突，那么以你的title 和 filename 的配置值为准。

##### 4. inject：注入选项。有四个选项值 true, body, head, false.
> 1. true：默认值，script标签位于html文件的 body 底部
> 2. body：script标签位于html文件的 body 底部（同 true）
> 3. head：script 标签位于 head 标签内
> 4. false：不插入生成的 js 文件，只是单纯的生成一个 html 文件

##### 5. favicon：给生成的 html 文件生成一个 favicon。属性值为 favicon 文件所在的路径名
##### 6. minify：minify 的作用是对 html 文件进行压缩，minify 的属性值是一个压缩选项或者 false 。默认值为false, 不对生成的 html 文件进行压缩。
```javascript
plugins:[
  new HtmlWebpackPlugin({
  //部分省略，具体看minify的配置
  minify: {
     //是否对大小写敏感，默认false
    caseSensitive: true,
    
    //是否简写boolean格式的属性如：disabled="disabled" 简写为disabled  默认false
    collapseBooleanAttributes: true,
    
    //是否去除空格，默认false
    collapseWhitespace: true,
    
    //是否压缩html里的css（使用clean-css进行的压缩） 默认值false；
    minifyCSS: true,
    
    //是否压缩html里的js（使用uglify-js进行的压缩）
    minifyJS: true,
    
    //Prevents the escaping of the values of attributes
    preventAttributesEscaping: true,
    
    //是否移除属性的引号 默认false
    removeAttributeQuotes: true,
    
    //是否移除注释 默认false
    removeComments: true,
    
    //从脚本和样式删除的注释 默认false
    removeCommentsFromCDATA: true,
    
    //是否删除空属性，默认false
    removeEmptyAttributes: true,
    
    //  若开启此项，生成的html中没有 body 和 head，html也未闭合
    removeOptionalTags: false, 
    
    //删除多余的属性
    removeRedundantAttributes: true, 
    
    //删除script的类型属性，在h5下面script的type默认值：text/javascript 默认值false
    removeScriptTypeAttributes: true,
    
    //删除style的类型属性， type="text/css" 同上
    removeStyleLinkTypeAttributes: true,
    
    //使用短的文档类型，默认false
    useShortDoctype: true,
    }
  })
]
```
##### 7.hash：hash选项的作用是 给生成的 js 文件一个独特的 hash 值，该 hash 值是该次 webpack 编译的 hash 值。默认值为 false 。
```javascript
plugins: [
  new HtmlWebpackPlugin({
    hash: true
  })
]
```
编译打包后
```html
<script type=text/javascript src=bundle.js?22b9692e22e7be37b57e></script>
```
执行 webpack 命令后，你会看到你的生成的 html 文件的 script 标签内引用的 js 文件，是不是有点变化了。
bundle.js 文件后跟的一串 hash 值就是此次 webpack 编译对应的 hash 值。
##### 8.cache：默认是true的，表示内容变化的时候生成一个新的文件。
##### 9.showErrors：这个我们自运行项目的时候经常会用到，showErrors 的作用是，如果 webpack 编译出现错误，webpack会将错误信息包裹在一个 pre 标签内，属性的默认值为 true ，也就是显示错误信息。开启这个，方便定位错误
##### 10.chunks：chunks主要用于多入口文件，当你有多个入口文件，那就回编译后生成多个打包后的文件，那么chunks 就能选择你要使用那些js文件
```javascript
entry: {
  index: path.resolve(__dirname, './src/index.js'),
  devor: path.resolve(__dirname, './src/devor.js'),
  main: path.resolve(__dirname, './src/main.js')
}

plugins: [
  new httpWebpackPlugin({
    chunks: ['index','main']
  })
]
```
那么编译后：
```html
<script type=text/javascript src="index.js"></script>
<script type=text/javascript src="main.js"></script>
```
而如果没有指定 chunks 选项，默认会全部引用。
##### 11.excludeChunks：排除掉一些js,
```javascript
entry: {
  index: path.resolve(__dirname, './src/index.js'),
  devor: path.resolve(__dirname, './src/devor.js'),
  main: path.resolve(__dirname, './src/main.js')
}

plugins: [
  new httpWebpackPlugin({
   excludeChunks: ['devor.js']//和的等等效
  })
]
```
那么编译后：
```html
<script type=text/javascript src="index.js"></script>
<script type=text/javascript src="main.js"></script>
```














