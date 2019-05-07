---
title: 字符串截取函数slice()、substr()、substring()的用法和区别
date: 2018-05-011 18:44:30
author: huangkuiwei
img: http://img.25pp.com/uploadfile/soft/images/2015/0301/20150301021016689.jpg
tags: 
    - javascript
---
## 一.前言
> 在javascript中有三个很常用的字符串截取函数：slice()、substr()、substring()，这三个函数经常混在一起用，老是会记糊涂，所以我今天特别整理了一下这三个函数的用法。
## 二.用法
### 1.slice()
①.slice方法用于从原字符串取出子字符串并返回，不改变原字符串。它的第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含该位置）。
```
'JavaScript'.slice(0, 4)        // "Java"
```
②.如果省略第二个参数，则表示子字符串一直到原字符串结束。
```
'JavaScript'.slice(4)       // "Script"
```
③.如果参数是负值，表示从结尾开始倒数计算的位置，即该负值加上字符串长度。
```
'JavaScript'.slice(-6)          // "Script"
'JavaScript'.slice(0, -6)       // "Java"
'JavaScript'.slice(-2, -1)      // "p"
```
④.如果第一个参数大于第二个参数，slice方法返回一个空字符串。
```
'JavaScript'.slice(2, 1)        // ""
```
### 2.substr()
①.substr方法用于从原字符串取出子字符串并返回，不改变原字符串，跟slice和substring方法的作用相同。substr方法的第一个参数是子字符串的开始位置（从0开始计算），第二个参数是子字符串的长度。
```
'JavaScript'.substr(4, 6)       // "Script"
```
②.如果省略第二个参数，则表示子字符串一直到原字符串的结束。
```$xslt
'JavaScript'.substr(4)      // "Script"
```
③.如果第一个参数是负数，表示倒数计算的字符位置。如果第二个参数是负数，将被自动转为0，因此会返回空字符串。
```$xslt
'JavaScript'.substr(-6)         // "Script"
'JavaScript'.substr(4, -1)      // ""
```
上面代码中，第二个例子的参数-1自动转为0，表示子字符串长度为0，所以返回空字符串。
### 3.substring()
①.substring方法用于从原字符串取出子字符串并返回，不改变原字符串，跟slice方法很相像。它的第一个参数表示子字符串的开始位置，第二个位置表示结束位置（返回结果不含该位置）。
```$xslt
'JavaScript'.substring(0, 4)        // "Java"
```
②.如果省略第二个参数，则表示子字符串一直到原字符串的结束。
```$xslt
'JavaScript'.substring(4)       // "Script"
```
③.如果第一个参数大于第二个参数，substring方法会自动更换两个参数的位置。
```$xslt
'JavaScript'.substring(10, 4)       // "Script"
// 等同于
'JavaScript'.substring(4, 10)       // "Script"
```
上面代码中，调换substring方法的两个参数，都得到同样的结果。
④.如果参数是负数，substring方法会自动将负数转为0。
```$xslt
'JavaScript'.substring(-3)          // "JavaScript"
'JavaScript'.substring(4, -3)       // "Java"
```
上面代码中，第二个例子的参数-3会自动变成0，等同于'JavaScript'.substring(4, 0)。由于第二个参数小于第一个参数，会自动互换位置，所以返回Java。
由于这些规则违反直觉，因此不建议使用substring方法，应该优先使用slice。