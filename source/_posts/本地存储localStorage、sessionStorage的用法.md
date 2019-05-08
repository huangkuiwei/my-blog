---
title: 本地存储localStorage、sessionStorage的用法
date: 2018-07-12 21:09:11
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
## 一. 介绍
在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k)，localStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。
## 二. 优势与局限
localStorage的优势
1. localStorage拓展了cookie的4K限制
2. localStorage会可以将第一次请求的数据直接存储到本地，这个相当于一个5M大小的针对于前端页面的数据库，相比于cookie可以节约带宽，但是这个却是只有在高版本的浏览器中才支持的

localStorage的局限
1. 浏览器的大小不统一，并且在IE8以上的IE版本才支持localStorage这个属性
2. 目前所有的浏览器中都会把localStorage的值类型限定为string类型，这个在对我们日常比较常见的JSON对象类型需要一些转换
3. localStorage在浏览器的隐私模式下面是不可读取的
4. localStorage本质上是对字符串的读取，如果存储内容多的话会消耗内存空间，会导致页面变卡
5. localStorage不能被爬虫抓取到
>localStorage与sessionStorage的唯一一点区别就是localStorage属于永久性存储，而sessionStorage属于当会话结束的时候，sessionStorage中的键值对会被清空

所以我们以localStorage来分析
## 三. 使用
### 1. localStorage的写入
```javascript
localStorage['name'] = 'huang';
// 或者
localStorage.age = 22;
// 或者（推荐）
localStorage.setItem('address', '长沙市')
```
![localStorage](/medias/postimages/02.png "localStorage")
>注意：localStorage/sessionStorage储存的值都是字符串格式，typeof的类型都是'string',所有有时需要JSON.stringify()和JSON.parse()进行处理。

### 2. localStorage的读取
```javascript
console.log(localStorage['name']);    // 'huang'
// 或者
console.log(localStorage.age);        // '22'
// 或者（推荐）
localStorage.getItem('address')       // '长沙市'
```
### 2. localStorage的s删除
localStorage的删除主要有两个方法：
>1. localStorage.clean()        // 把所有的键值对删除
>2. localStorage.removeItem()   // 只删除某一个键值对
```javascript
localStorage.removeItem('address');
console.log(localStorage.getItem('address'));   // undefined
localStorage.clean();
console.log(localStorage.getItem('age'))        // undefined
```
## 四. localStorage其他注意事项
 一般我们会将对象存入localStorage中，但是在localStorage会自动将localStorage转换成为字符串形式，这个时候我们可以使用JSON.stringify()这个方法，来将对象转换成为JSON字符串。
```javascript
let info = {
  name: 'huang',
  age: 22,
  address: '长沙市'
};
let data = JSON.stringify(info);
localStorage.setItem('data', data);
console.log(localStorage.data)    // '{"name":"huang","age":22,"address":"长沙市"}'   
```
读取之后要将JSON字符串转换成为JSON对象，使用JSON.parse()方法
```javascript
let jsonObj = JSON.parse(localStorage.getItem('data'));
console.log(jsonObj)        // {"name":"huang","age":22,"address":"长沙市"}
```