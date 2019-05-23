---
title: 关键字new到底做了什么？
date: 2018-10-02 12:10:32
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
**通过关键字 new 创建新对象会经历下面四个步骤**
>1. 创建一个新对象：let o = {}
>2. 将构造函数的作用域赋给新对象（因此this指向了这个新对象）：Person.call(o, arg1, agg2, ...) （Person原来的this指向的是window）
>3. 执行构造函数中的代码(为这个新对象添加属性)
>4. 返回新对象

常规代码：
```javascript
function People(name, age) {
  this.name = name;
  this.age = age
}

People.prototype.say = function(){
  console.log('我的名字是' + this.name + '，今年' + this.age + '岁了')
};

let p = new People('huang', 24);
p.say();     // 我的名字是huang，今年24岁了

// p 的 __proto__ 指向 p 的构造函数 People 的 prototype 属性
console.log(p.__proto__ === People.prototype);             // true
// People 的原型的 constructor 构造函数指向它本身（People本身就是构造函数）
console.log(People.prototype.constructor === People);      // true
console.log(p.__proto__.constructor === People)            // true
```
自己封装一个和 new 有相同效果的函数：
```javascript
function People(name, age) {
  this.name = name;
  this.age = age
}

People.prototype.say = function () {
  console.log('hello world')
};
let p1 = new People('huang', 23);

function New(Fn) {

  let obj = {};                   // 定义一个空对象
  let args = [...arguments].slice(1);      // 拿到实参数组，交给 apply

  obj.__proto__ = Fn.prototype;

  Fn.apply(obj, args);         // 改变 Fn 中 this 的指向并执行 Fn 函数
  return obj
}

let p2 = New(People, 'huang', 23)

```
![效果图](/medias/postimages/07.png "效果图")