---
title: v-model使用总结
date: 2018-08-20 10:15:41
author: huangkuiwei
img: http://www.manongjc.com/images/51jb/154648858215Q4G648Pl8582.png
tags: 
    - Vue
    - JavaScript
---
## 一. 前言
你可以用 v-model 指令在表单 input、textarea 及 select 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。
>v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。

v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

1. text 和 textarea 元素使用 value 属性和 input 事件；
2. checkbox 和 radio 使用 checked 属性和 change 事件；
3. select 字段将 value 作为 prop 并将 change 作为事件。

## 二. 文本
```html
<input type="text" v-model="message">
<p>{{message}}</p>
```
当 input 输入框中的内容改变时，p 元素的内容也会随之更改。
## 三. 多行文本
```html
<textarea v-model="message"></textarea>
<p>{{message}}</p>
```
同上。
>在文本区域插值并不会生效，应用 v-model 来代替。

## 四. 复选框
1.单个复选框，绑定到布尔值：
```html
<input type="checkbox" v-model="checkbox">{{checkbox}}
```
```javascript
new Vue({
  el: '#app',
  data: {
    checkbox: false
  }
})
```
当选择框被选择时，checkbox 的值为 true，反之为 false。

2.多个复选框，绑定到同一个数组：
```html
<input type="checkbox" value="篮球" v-model="checkbox">篮球
<input type="checkbox" value="足球" v-model="checkbox">足球
<input type="checkbox" value="乒乓球" v-model="checkbox">乒乓球
<p>{{checkbox}}</p>
```
```javascript
new Vue({
  el: '#app',
  data: {
    checkbox: ['足球']
  }
})
```
## 五. 单选按钮
1.
```html
<input type="radio">
```