---
title: JavaScript中的奇淫技巧（持续更新）
date: 2018-09-22 21:14:55
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
在这篇文章中，我会记录一些在我平时工作中遇到的一些有趣的JavaScript写法和技巧，这些写法有的很实用，有的很装逼，反正看起来很酷就是了。
### 1.数组快速去重
现在有一个数组：[4, 7, 2, 6, 2, 4, 7, 4, 8, 3, 5, 2, 6]
```javascript
let arr = [4, 7, 2, 6, 2, 4, 7, 4, 8, 3, 5, 2, 6];
let set = new Set(arr);
let newArr = [...set];
console.log(newArr)   // [4, 7, 2, 6, 8, 3, 5]
```
>分析：实例化新 Set 对象时，可以接收一个可迭代对象，由于 Set 对象中的值具有唯一性，所有无法重复添加值，重复添加的值会被过滤掉，最后利用解构赋值就可以将 Set 对象中的值赋给新数组，这个新数组就是过滤掉重复值的数组了。

### 2. 交换两个变量的值
```javascript
// 以前的写法，利用第三个变量作为中转站
let a = 1;
let b = 2;
let c = a;
a = b;
b = c;
console.log(a, b)     // 2 1
```
```javascript
// 利用解构赋值
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a, b)     // 2 1
```
>分析：利用解构赋值，[a, b] = [b, a] 相当于 [a, b] = [2, 1]，此时 a 的值就变为了2，同理 b 的值就变成了1。

### 3. 快速获取一个数组中的最大值或者最小值
```javascript
let arr = [1, 4, 5, 2, 6, 3, 9, 3];
// 第一种方法，利用剩余参数
console.log(Math.max(...arr));            // 9
// 第二种方法，这里第一个参数是什么都无所谓，不需要用到 this
console.log(Math.max.apply(null, arr))    // 9
```
>解释见注释，获取最小值同理，Math.min()