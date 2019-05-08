---
title: JavaScript中var与let、const的区别
date: 2018-06-22 21:05:25
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
## 一. 前言
&emsp;&emsp;在JavaScript中，有三个声明变量的中关键词，ES5中var以及ES6中新增的let、const，他们都可以用来声明变量，不过在用法上又有些区别，我今天就整理了一下，主要是介绍他们三者常见的用法和区别。
## 二. 区别
### 1. var声明的全局变量会挂载在window上，而let和const声明的变量不会
```javascript
// var
var a = 100;
console.log(a, window.a)     // 100 100
```
```javascript
// let 
let a = 100;
console.log(a, window.a)    // 100 undefined
```
```javascript
// const
const a = 100;
console.log(a, window.a)    // 100 undefined
```
### 2. var声明变量存在变量提升，let和const不存在变量提升
```javascript
// var
console.log(a);   // undefined
var a = 100
```
```javascript
// let
console.log(a);   // 报错
let a = 100
```
```javascript
// const
console.log(a);   // 报错
const a = 100
```
### 3. let和const声明形成块作用域
>块作用域一般是指{}内的区域，在这个块作用域内用let和const定义的变量，在外部是无法访问到的。**值得注意的一点是，if/for循环中()内使用let或者const声明的变量属于后面的{}，而不是{}外部。**

```javascript
if(true){
  var a = 1;
  let b = 2
}
console.log(a);   // 1
console.log(b);   // 报错
```
```javascript
if(true){
  var a = 1;
  const c = 3
}
console.log(a);   // 1
console.log(c)    // 报错
```
### 4. 同一作用域下let和const不能声明同名变量，而var可以
```javascript
// var
var a = 1;
var a = 2;
console.log(a)    // 2
```
```javascript
// let
let a = 1;
let a = 2   // 报错 Identifier 'a' has already been declared
```
```javascript
// const
const a = 1;
const a = 2   // 报错 Identifier 'a' has already been declared
```
###  5.暂存性死区
>在相同的函数或块作用域内重新声明同一个变量会引发SyntaxError；在声明变量或常量之前使用它，会引发ReferenceError，这就是暂存性死区。

```javascript
var a = 100;
if(true){
  // 在当前块作用域中存在a使用let/const声明的情况下，给a赋值10时，只会在当前作用域找变量a
  // 而这时，还未到声明时候，所以控制台Error:a is not defined
  a = 10;
  let a = 1
}
```
```javascript
function test(){
   var foo = 33;
   if (true) {
      let foo = (foo + 55);   // ReferenceError
   }
}
test()
```
在上面例子中，if语句中使用了let声明了foo，因此在(foo+55)中引用的是if块级作用域中的foo，而不是test函数中的foo。但是由于if中的foo还没有声明完，因此它仍处于暂存死区。
### 6. const
注意事项：
① 一旦声明必须赋值。
② 声明后不能再修改。
③ 如果声明的是复合类型数据可以修改其属性。
```javascript
const a;    // Missing initializer in const declaration
```
```javascript
const a = 100;
a = 200   // Assignment to constant variable
```
```javascript
const arr = [];
arr.push('hello');
console.log(arr)    // ['hello']
```