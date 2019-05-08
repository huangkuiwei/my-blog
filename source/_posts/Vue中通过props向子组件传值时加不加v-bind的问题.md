---
title: Vue中通过props向子组件传值时加不加v-bind的问题
date: 2018-07-10 21:55:46
author: huangkuiwei
img: http://www.manongjc.com/images/51jb/154648858215Q4G648Pl8582.png
tags: 
    - Vue
    - JavaScript
---
## 一. 前言
我在刚开始学vue的时候，有关Vue中父组件通过prop传值给子组件时，是否加v-bind的问题，没弄清楚时感觉很乱，弄清楚之后很简单。由于结果记起来很容易，所以先给出结果：
>只有传递字符串常量时，不采用v-bind形式，其余情况均采用v-bind形式传递。

## 二. 实例
### 1. String
```html
<my-component title="Hello World"></my-component>
```
如果传入的值是字符串变量：
```html
<!-- stringValue为Vue示例中已经绑定的字符串变量 -->
<my-component :title="stringValue"></my-component>
```
### 2. Number
```html
<my-component :num="100"></my-component>
```
如果传入的值是数值变量：
```html
<!-- numValue为Vue示例中已经绑定的数值变量 -->
<my-component :num="numValue"></my-component>
```
### 3. Boolean
```html
<my-component :bool="false"></my-component>
```
如果传入的值是布尔值变量：
```html
<!-- BooleanValue为Vue示例中已经绑定的布尔值变量 -->
<my-component :bool="BooleanValue"></my-component>
```
### 4. Object/Array
```html
<my-component :obj="{name: 'huang'}"></my-component>
<!-- 或者 -->
<my-component :arr="['huang', 'liu']"></my-component>
```
如果传入的值是对象/数组变量：
```html
<!-- objValue为Vue示例中已经绑定的对象变量 -->
<my-component :obj="objValue"></my-component>
<!-- 或者 -->
<!-- arrValue为Vue示例中已经绑定的数组变量 -->
<my-component :obj="arrValue"></my-component>
```
