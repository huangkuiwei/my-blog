---
title: JavaScript中的奇淫技巧（持续更新）
date: 2018-09-22 21:14:55
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
在这篇文章中，我会记录一些在我平时工作中遇到的一些有趣的JavaScript写法和技巧，这些写法有的很实用，有的很装逼，反正看起来很酷就是了。
### 1. 数组快速去重
现在有一个数组：[4, 7, 2, 6, 2, 4, 7, 4, 8, 3, 5, 2, 6]
```javascript
let arr = [4, 7, 2, 6, 2, 4, 7, 4, 8, 3, 5, 2, 6];
let set = new Set(arr);
let newArr = [...set];
console.log(newArr)   // [4, 7, 2, 6, 8, 3, 5]
```
>分析：实例化新 Set 对象时，可以接收一个可迭代对象，由于 Set 对象中的值具有唯一性，所有无法重复添加值，重复添加的值会被过滤掉，最后利用解构赋值就可以将 Set 对象中的值赋给新数组，这个新数组就是过滤掉重复值的数组了。
>此技巧适用于包含基本类型的数组：undefined，null，boolean，string和number。 （如果你有一个包含对象，函数或其他数组的数组，你需要一个不同的方法！）

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

### 4. 确保用到原型上的方法是准确的
```javascript
let arr = [1, 4, 5, 2, 6, 3, 9, 3];
arr.slice = function() {
  console.log('这个数组的slice方法被修改了')
};
// 要截取数组 arr 前三位。发现 slice 方法不管用了。
console.log(arr.slice(0, 3));         // 这个数组的slice方法被覆盖了
// 直接使用原型上的方法
Array.prototype.slice.call(arr, 0, 3)   // [1, 4, 5]
```
>分析：对象原型上的方法可以会被对象本身定义的方法覆盖，如果你不确定方法被覆盖，而此时要用到原型上的方法，就可以采取这种方式，方法后面使用 call 或者 apply 方法，使函数的 this 指向你要使用的对象，后面再接参数。

### 5. 快速将变量转换位数字或者字符串
```javascript
// 转换为数字类型
let a = '123';
let arr1 = ['22'];
let arr2 = ['12', '32'];
let obj = {};
let b = true;
console.log(+a, +arr1, +arr2, +obj, +b);          // 123 22 NaN NaN 1
console.log(typeof +a, typeof +arr1, typeof +arr2, typeof +obj)   // number number number number
```
>通过一元运算符 + 或者 - 将其它类型转换为数字类型。会自动调用函数 Number()。

```javascript
// 转换为字符串类型类型
let a = 123;
let b = '123';
let c = [1,2,3];
let d = {name: 'huang'};
let e = true;
console.log(a + '', b + '', c + '', d + '')     // 123 123 1,2,3 [object Object] true
```
>通过在变量后面加一个空字符串将其它类型转换为字符串类型。会自动调用函数 String()。