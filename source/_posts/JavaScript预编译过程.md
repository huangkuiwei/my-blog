---
title: JavaScript预编译过程
date: 2018-07-18 16:04:02
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
>在一个js代码执行过程中，一般认为会存在三个步骤，也就是所谓的js三部曲，分别是：
1. 语法分析
2. 预编译
3. 解释执行

本文会以预编译为重点对js的代码执行进行分析。
## 一. 语法分析
js代码的执行是读一行代码执行一行，但在执行之前系统会先对js进行全面扫描检查是否存在低级的语法错误，并不会立即执行语句。
## 二. 预编译
### 1. 函数声明整体提升
我们写一个函数，我们先执行这个函数，然后再写函数体查看它是否能执行：
```javascript
test();   // 'hello world'
function test() {
  console.log('hello world')
}
```
执行结果并没有让我们失望，控制台上打印出了'hello world'，只是为什么呢？
**这是因为js的预编译将函数体放在了js的最前头，也就是将函数声明整体优先提升，变成下面的代码顺序。**
```javascript
function test() {
  console.log('hello world')
}
test()    // 'hello world'
```
### 2. 变量声明提升
我们用var声明一个变量，但是我们把输出这个变量的命令写在声明的前面。
```javascript
console.log(a);   // undefined
var a = 100
```
这里显示的是undefined，而不是1。
**这是因为js预编译将变量的声明提升到了js最前面而不是将它的变量的值提升，也就是将var a提升，而a=1还是在它原来的位置，所以也就是说只是声明了a并没有赋值，从而变成下面的一段代码。**
```javascript
var a;
console.log(a);
a = 100
```
### 3. 解释执行
对代码进行解释执行，在执行代码之前js会先进行一个预编译。
>1. js会创建一个AO对象（活动对象，一般时指函数的上下文对象）
>2. 找形参和变量声明，将变量和形参名作为AO对象的属性名，并给它们付一个初始值undefined
>3. 将实参值和形参统一
>4. 在函数体里面找函数声明，值赋予函数体

实例
```javascript
function fn(a){
  console.log(a);
  var a=123;
  console.log(a);
  function a(){}
  console.log(a);
  var b=function (){};
  console.log(b);
  function d(){}
}
fn(1)
```
![效果图](/medias/postimages/03.png "效果图")
一步步来推算：
1.系统自动创建对象AO={}。
2.找形参和变量声明，将变量和形参名作为AO对象的属性名，并给它们付一个初始值undefined。
>形参：a
>变量声明：a和b    //形参里有a了但是变量声明也有a这时只写一个a就行
>找到之后将这a和b当作AO对象的属性名写进去并且赋值为undefined。

```javascript
AO = {
  a: undefined,
  b: undefined
}
```
3.将实参值和形参统一，也就是将实参的值赋给形参。
```javascript
AO = {
  a:1,
  b:undefined
}
```
4.在函数体里面找函数声明，值赋予函数体。
```javascript
AO={
  a:function a(){},
  b:function (){},
  d:function (){}
}
```
预编译提前帮我们整理了js的执行顺序，根据函数声明整体提升，变量声明提升这两句就可以得到。
```javascript
function fn(a){
  var a;
  var b;
  function a(){}
  function d(){}
  console.log(a);
  a=123;
  console.log(a);
  console.log(a);
  b=function (){};
  console.log(b)
}
fn(1);
```
这样看就更加显而易见。