---
title: JavaScript类型转换总结
date: 2018-08-01 22:05:41
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - JavaScript
---
## 一. 数据类型转换
### 1. 强制转换
强制转换主要指使用Number()、String()和Boolean()三个函数，手动将各种类型的值，分别转换成数字、字符串或者布尔值。
#### 1.1 Number()
使用Number函数，可以将任意类型的值转化成数值。
下面分成两种情况讨论，一种是参数是原始类型的值，另一种是参数是对象。
① 原始类型的值转换：
```javascript
// 数值：转换后还是原来的值
Number(324);    // 324

// 字符串：如果可以被解析为数值，则转换为相应的数值
Number('324');    // 324

// 字符串：如果不可以被解析为数值，返回 NaN
Number('324abc');   // NaN

// 空字符串转为0
Number('');   // 0

// 布尔值：true 转成 1，false 转成 0
Number(true);   // 1
Number(false);  // 0

// undefined：转成 NaN
Number(undefined);    // NaN

// null：转成0
Number(null)    // 0
```
>Number函数将字符串转为数值，要比parseInt函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为NaN。

```javascript
parseInt('42 cats');    // 42
Number('42 cats')       // NaN
```
上面代码中，parseInt逐个解析字符，而Number函数整体转换字符串的类型。
>另外，parseInt和Number函数都会自动过滤一个字符串前导和后缀的空格。

```javascript
parseInt('\t\v\r12.34\n');    // 12
Number('\t\v\r12.34\n')       // 12.34
```
② 对象类型的值转换：
>简单的规则是，Number方法的参数是对象时，将返回NaN，除非是包含单个数值的数组。

```javascript
Number({a: 1});       // NaN
Number([1, 2, 3]);    // NaN
Number([5])           // 5
```
之所以会这样，是因为Number背后的转换规则比较复杂。
1. 第一步，调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，不再进行后续步骤。
2. 第二步，如果valueOf方法返回的还是对象，则改为调用对象自身的toString方法。如果toString方法返回原始类型的值，则对该值使用Number函数，不再进行后续步骤。
3. 第三步，如果toString方法返回的是对象，就报错。

Number函数将obj对象转为数值。背后发生了一连串的操作，首先调用obj.valueOf方法, 结果返回对象本身；于是，继续调用obj.toString方法，这时返回字符串[object Object]，对这个字符串使用Number函数，得到NaN。
默认情况下，对象的valueOf方法返回对象本身，所以一般总是会调用toString方法，而toString方法返回对象的类型字符串（比如[object Object]）。所以，会有下面的结果。
```javascript
Number({})      // NaN
```
#### 1.2 String()
① 原始类型的值转换：
1. 数值：转为相应的字符串。
2. 字符串：转换后还是原来的值。
3. 布尔值：true转为字符串"true"，false转为字符串"false"。
4. undefined：转为字符串"undefined"。
5. null：转为字符串"null"。

```javascript
String(123);          // "123"
String('abc');        // "abc"
String(true);         // "true"
String(undefined);    // "undefined"
String(null)          // "null"
```
String方法背后的转换规则，与Number方法基本相同，只是互换了valueOf方法和toString方法的执行顺序。
1. 先调用对象自身的toString方法。如果返回原始类型的值，则对该值使用String函数，不再进行以下步骤。
2. 如果toString方法返回的是对象，再调用原对象的valueOf方法。如果valueOf方法返回原始类型的值，则对该值使用String函数，不再进行以下步骤。
3. 如果valueOf方法返回的是对象，就报错。

#### 1.3 Boolean()
Boolean()函数可以将任意类型的值转为布尔值。
它的转换规则相对简单：除了以下五个值的转换结果为false，其他的值全部为true。
1. undefined
2. null
3. 0（包含-0和+0）
4. NaN
5. ''（空字符串）

```javascript
Boolean(undefined);   // false
Boolean(null);        // false
Boolean(0);           // false
Boolean(NaN);         // false
Boolean('')           // false
```
>注意，所有对象（包括空对象）的转换结果都是true，包括false对应的布尔对象new Boolean(false)也是true。

```javascript
Boolean({});                   // true
Boolean([]);                   // true
Boolean(new Boolean(false))    // true
```
### 2. 自动转换
下面介绍自动转换，它是以强制转换为基础的。
遇到以下三种情况时，JavaScript 会自动转换数据类型，即转换是自动完成的，用户不可见。
第一种情况，不同类型的数据互相运算。
```javascript
123 + 'abc'   // "123abc"
```
第二种情况，对非布尔值类型的数据求布尔值。
```javascript
if ('abc') {
  console.log('hello')
}  // "hello"
```
第三种情况，对非数值类型的值使用一元运算符（即+和-）。
```javascript
+ {foo: 'bar'};   // NaN
- [1, 2, 3]       // NaN
```
>自动转换的规则是这样的：预期什么类型的值，就调用该类型的转换函数。比如，某个位置预期为字符串，就调用String函数进行转换。如果该位置即可以是字符串，也可能是数值，那么默认转为数值。

由于自动转换具有不确定性，而且不易除错，建议在预期为布尔值、数值、字符串的地方，全部使用Boolean、Number和String函数进行显式转换。
#### 2.1 自动转换为布尔值
JavaScript 遇到预期为布尔值的地方（比如if语句的条件部分），就会将非布尔值的参数自动转换为布尔值。系统内部会自动调用Boolean函数。
因此除了以下五个值，其他都是自动转为true。
1. undefined
2. null
3. +0或-0
4. NaN
5. ''（空字符串）

下面两种写法，有时也用于将一个表达式转为布尔值。它们内部调用的也是Boolean函数。
```javascript
// 写法一
expression ? true : false

// 写法二
!! expression
```
#### 2.2 自动转换为字符串
JavaScript 遇到预期为字符串的地方，就会将非字符串的值自动转为字符串。具体规则是，先将复合类型的值转为原始类型的值，再将原始类型的值转为字符串。
字符串的自动转换，主要发生在字符串的加法运算时。当一个值为字符串，另一个值为非字符串，则后者转为字符串。
```javascript
'5' + 1                 // '51'
'5' + true              // "5true"
'5' + false             // "5false"
'5' + {}                // "5[object Object]"
'5' + []                // "5"
'5' + function (){}     // "5function (){}"
'5' + undefined         // "5undefined"
'5' + null              // "5null"
```
#### 2.3 自动转换为数值 
JavaScript 遇到预期为数值的地方，就会将参数值自动转换为数值。系统内部会自动调用Number函数。
除了加法运算符（+）有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值。
```javascript
'5' - '2'          // 3
'5' * '2'          // 10
true - 1           // 0
false - 1          // -1
'1' - 1            // 0
'5' * []           // 0
false / '5'        // 0
'abc' - 1          // NaN
null + 1           // 1
undefined + 1      // NaN
```
>注意：null转为数值时为0，而undefined转为数值时为NaN。

一元运算符也会把运算子转成数值。
```javascript
+'abc'    // NaN
-'abc'    // NaN
+true     // 1
-false    // 0
```