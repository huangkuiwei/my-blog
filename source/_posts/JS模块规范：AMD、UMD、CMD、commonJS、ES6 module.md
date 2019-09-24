---
title: JS模块规范：AMD、UMD、CMD、commonJS、ES6 module
date: 2019-04-18 21:26:34
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - ES6 JavaScript
---

## 1. commonJS
**特点：**
> 1. 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
> 2. 模块加载会阻塞接下来代码的执行，需要等到模块加载完成才能继续执行——同步加载。

**环境：**服务器环境
**应用：**nodejs的模块规范是参照commonJS实现的。
**语法：**
> 1. 导入：require('路径')
> 2. 导出：module.exports和exports

**注意：**module.exports和exports的的区别是exports只是对module.exports的一个引用，相当于Node为每个模块提供一个exports变量，指向module.exports。这等同在每个模块头部，有一行var exports = module.exports;这样的命令。
```javascript
// a.js
// 相当于这里还有一行：var exports = module.exports;代码
exports.a = 'Hello world';  // 相当于：module.exports.a = 'Hello world';

// b.js
let moduleA = require('./a.js');
console.log(moduleA.a);     // 打印出hello world
```

## 2. AMD
**特点：**
> 1. 异步加载
> 2. 管理模块之间的依赖性，便于代码的编写和维护。

**环境：**浏览器环境
**应用：**requireJS是参照AMD规范实现的
**语法：**
> 1. 导入：require(['模块名称'], function ('模块变量引用'){// 代码});
> 2. 导出：define(function (){return '值');

```javascript
// a.js
define(function (){
　　return {
　　　a:'hello world'
　　}
});
// b.js
require(['./a.js'], function (moduleA){
    console.log(moduleA.a); // 打印出：hello world
});
```

## 3. CMD
**特点：**CMD是在AMD基础上改进的一种规范，和AMD不同在于对依赖模块的执行时机处理不同，CMD是就近依赖，而AMD是前置依赖。
**环境：**浏览器环境
**应用：**seajs是参照UMD规范实现的，requireJS的最新的几个版本也是部分参照了UMD规范的实现
**语法：**
> 1. 导入：define(function(require, exports, module) {});
> 2. 导出：define(function (){return '值');

```javascript
// a.js
define(function (require, exports, module){
　　exports.a = 'hello world';
});
// b.js
define(function (require, exports, module){
    let moduleA = require('./a.js');
    console.log(moduleA.a); // 打印出：hello world
});
```

## 4. UMD
**特点：**兼容AMD和commonJS规范的同时，还兼容全局引用的方式
**环境：**浏览器或服务器环境
**应用：**无
**语法：**无导入导出规范，只有如下的一个常规写法：
```javascript
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        //AMD
        define(['jquery'], factory);
    } else if (typeof exports === 'object') {
        //Node, CommonJS之类的
        module.exports = factory(require('jquery'));
    } else {
        //浏览器全局变量(root 即 window)
        root.returnExports = factory(root.jQuery);
    }
}(this, function ($) {
    //方法
    function myFunc(){};
    //暴露公共方法
    return myFunc;
}));
```

## 5.ES6 module
**特点：**
> 1. 按需加载（编译时加载）
> 2. import和export命令只能在模块的顶层，不能在代码块之中（如：if语句中）,import()语句可以在代码块中实现异步动态按需动态加载

**环境：**浏览器或服务器环境（以后可能支持）
**应用：**ES6的最新语法支持规范
**语法：**
> 1. 导入：import {模块名A，模块名B...} from '模块路径'
> 2. 导出：export和export default
> 3. import('模块路径').then()方法

**注意：**export只支持对象形式导出，不支持值的导出，export default命令用于指定模块的默认输出，只支持值导出，但是只能指定一个，本质上它就是输出一个叫做default的变量或方法。
```javascript
/*错误的写法*/
// 写法一
export 1;

// 写法二
var m = 1;
export m;

// 写法三
if (x === 2) {
  import MyModual from './myModual';
}

/*正确的三种写法*/
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};

// 写法四
var n = 1;
export default n;

// 写法五
if (true) {
    import('./myModule.js')
    .then(({export1, export2}) => {
      // ...·
    });
}

// 写法六
Promise.all([
  import('./module1.js'),
  import('./module2.js'),
  import('./module3.js'),
])
.then(([module1, module2, module3]) => {
   ···
});
```

