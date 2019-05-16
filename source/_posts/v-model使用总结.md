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

1. text 和 textarea 元素使用 value 属性和 input 事件。
2. checkbox 和 radio 使用 checked 属性和 change 事件。
3. select 字段将 value 作为 prop 并将 change 作为事件。

v-model语法糖：
```html
<input type="text" v-model="message">
// 等价于
<input type="text" :vlaue="message" @input="message=$event.target.value">
```
应用在组件上：
```html
<base-input v-model="message"></base-input>
```
```javascript
// 注册组件
Vue.component('base-input', {
  props: {
    value: String
  },
  template: `<input type="text" :value="value" @input="$emit('input', $event.target.value)"/>`
});

new Vue({
  el: '#app',
  data: {
    message: 'hello world'
  }
})
```
>为了让它正常工作，这个组件内的 input 必须：
>1. 将其 value 特性绑定到一个名叫 value 的 prop 上
>2. 在其 input 事件被触发时，将新的值通过自定义的 input 事件抛出

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
1.单选时通常绑定到字符串：
```html
<input type="radio" v-model="radio" value="篮球">篮球
<input type="radio" v-model="radio" value="足球">足球
<input type="radio" v-model="radio" value="乒乓球">乒乓球
<input type="radio" v-model="radio" value="橄榄球">橄榄球
<p>{{radio}}</p>
```
```javascript
new Vue({
  el: '#app',
  data: {
    radio: '篮球'
  }
})
```
## 六. 选择框
1.单选时 (通常绑定到一个字符串)：
```html
<select v-model="select">
  <option disabled value="">请选择</option>
  <option value="0">选项一</option>
  <option value="1">选项二</option>
  <option value="2">选项三</option>
</select>
<p>{{select}}</p>
```
```javascript
new Vue({
  el: '#app',
  data: {
    select: ''
  }
})
```
>1. 如果 v-model 表达式的初始值未能匹配任何选项，select 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。
>2. 如果 option 选项中没有 value 属性，那么 v-model 的值将会为 option 元素之间的文本值，比如上例中的'选项一'、'选项二'等。

2.多选时 (绑定到一个数组)：
```html
<select v-model="select" multiple>
  <option value="0">选项一</option>
  <option value="1">选项二</option>
  <option value="2">选项三</option>
</select>
<p>{{select}}</p>
```
```javascript
new Vue({
  el: '#app',
  data: {
    select: []
  }
})
```
## 七. 值绑定
我们可能想把值绑定到 Vue 实例的一个动态属性上，这时可以用 v-bind 实现，**并且这个属性的值可以不是字符串（可以是Object类型）**。
```html
<select v-model="select">
  <option disabled value="">请选择</option>
  <option v-for="item in list" :value="item.address">{{item.name}}</option>
</select>
<p>{{select}}</p>
```
```javascript
new Vue({
  el: '#app',
  data: {
    list: [
      {name: 'huang', age: 22, address: ['长沙市', '湘潭市']},
      {name: 'zhang', age: 33, address: ['湘潭市', '株洲市']},
      {name: 'li', age: 44, address: ['株洲市', '长沙市']},
      {name: 'tang', age: 55, address: ['娄底市', '湘潭市']}
    ],
    select: ['长沙市', '湘潭市']
  }
})
```
上面这个例子中，默认就选择了第一项 'huang'，此时 v-model 的值绑定到了一个数组，这也是可行的。
## 八. 修饰符
### .lazy
>在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步。
### .number
>如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符。这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 parseFloat() 解析，则会返回原始的值。
### .trim
>如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符。