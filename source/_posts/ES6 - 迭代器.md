---
title: ES6 - 迭代器
date: 2019-05-19 22:31:53
author: huangkuiwei
img: http://5b0988e595225.cdn.sohucs.com/images/20170910/4b1328ea36b14afa8a30a69f32eb9a44.jpeg
tags: 
    - JavaScript
    - ES6
---
迭代器在`MDN`中的定义：
> 在 JavaScript 中，迭代器是一个对象，它定义一个序列，并在终止时可能返回一个返回值。 更具体地说，迭代器是通过使用 next() 方法实现 Iterator protocol 的任何一个对象，该方法返回具有两个属性的对象： value，这是序列中的 next 值；和 done ，如果已经迭代到序列中的最后一个值，则它为 true 。如果 value 和 done 一起存在，则它是迭代器的返回值。

一旦创建，迭代器对象可以通过重复调用next（）显式地迭代。 迭代一个迭代器被称为消耗了这个迭代器，因为它通常只能执行一次。 在产生终止值之后，对next（）的额外调用应该继续返回{done：true}。

**用ES5模拟一个迭代器**
```javascript
function makeIterator (arr) {
    let nextIndex = 0;
    //返回一个迭代器方法
    return {
        next: () => {
            if(nextIndex < arr.length){
                return {value: arr[nextIndex++], done: false}
            }else{
                return {done: true}
            }
        }
    }
}

const it = makeIterator([1,2,3]);

console.log('1:', it.next());     // {value: 1, done: false}
console.log('2:', it.next());     // {value: 2, done: false}
console.log('3:', it.next());     // {value: 3, done: false}
console.log('end:', it.next());   // {done: true}
```
ES5中遍历集合通常都是 for循环，数组还有 forEach 方法，对象就是 for-in，ES6 中又添加了 Map 和 Set，而迭代器可以统一处理所有集合数据的方法。迭代器是一个接口，只要你这个数据结构暴露了一个iterator的接口，那就可以完成迭代。ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费。

数据结构只要部署了 Iterator 接口，我们就成这种数据结构为“可遍历”（Iterable）。ES6 规定，默认的 Iterator 接口部署在数据结构的 Symbol.iterator 属性，或者说，一个数据结构只要具有 Symbol.iterator 数据，就可以认为是“可遍历的”（iterable）。

**可以供 for...of 消费的原生数据结构：**
> 1. Array
> 2. Map
> 3. Set
> 4. String
> 5. TypedArray（一种通用的固定长度缓冲区类型，允许读取缓冲区中的二进制数据）
> 6. 函数中的 arguments 对象
> 7. NodeList 对象

可以看上面的原生数据结构中并没有对象（Object），为什么呢？
那是因为对象属性的遍历先后顺序是不确定的，需要开发者手动指定。本质上，遍历器是一种线性处理，对于任何非线性的数据结构，部署遍历器接口就等于部署一种线性变换。
**做如下处理，可以使对象供 for...of 消费：**

```javascript
let obj = {name: 'huang', age: 24};
 // 直接使用 for...of...循环，报错
 // for(let item of obj){
 //   console.log(item)     // 报错
 // }
obj[Symbol.iterator] = function () {
let num = Object.values(obj);
let index = 0;
return {
  next() {
    if (index < num.length) {
      return {
        value: num[index++],
        done: false
      }
    } else {
      return {
        value: undefined,
        done: true
      }
    }
  }
}
};
// 每次遍历都会调用返回的遍历器对象的 next() 方法，item 的值就是返回的 value 值。
// 一旦遇到 done 为 true的时候就停止遍历。
for (let item of obj) {
  console.log(item)     // huang 24
}
```












