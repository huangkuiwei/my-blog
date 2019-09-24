---
title: setTimeout模拟setInterval
date: 2019-04-23 11:20:30
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
`setTimeout`和`setInterval`都是js中的定时器，使用的基本语法相同。它们都有两个参数，一个是将要执行的代码字符串，还有一个是以毫秒为单位的时间间隔。
```javascript
setInterval(()=>{}, 1000);
setTimeout(()=>{}, 1000);
```
**区别：**setInterval在执行完一次代码之后，经过指定的时间间隔，执行代码，而setTimeout只执行一次那段代码。

**注意：**假设setTimeout定时器指定时间为1秒，而函数的执行时间是2秒，则setTimeout的总运行总时长为3秒。而setInterval不会被调用的函数所束缚，它只是简单地每隔一定时间就重复执行一次指定的函数。所以在函数的逻辑比较复杂，所处理的时间较长时，setInterval有可能会产生连续干扰的问题。若要避免这一问题，建议通过setTimeout来模拟一个setInterval。

**实现：**
```javascript
// 可避免setInterval因执行时间导致的间隔执行时间不一致
setTimeout (function () {
  // do something
  setTimeout (arguments.callee, 500)
}, 500)
```
这里主要用到了`arguments.callee`，这个属性返回当前正在执行的函数。
这里外层的 setTimeout 不要用箭头函数，因为箭头函数没有 arguments 属性。