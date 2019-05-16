---
title: $attrs和$listeners
date: 2018-09-07 21:34:12
author: huangkuiwei
img: http://www.manongjc.com/images/51jb/154648858215Q4G648Pl8582.png
tags: 
    - JavaScript
    - Vue
---
## 一. 前言
$attrs和$listeners是我们平时在封装自己的Vue组件的时候经常用到的两个属性，他们的用处很大，我们今天就说一说这两个属性的用法。
## 二. $attrs
定义：包含了父作用域中不作为 prop 被识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件——在创建高级别的组件时非常有用。
### 1.非 Prop 的特性
在讲 $attrs 之前，要说一下非 props 的特性。
>一个非 prop 特性是指传向一个组件，但是该组件并没有相应 prop 定义的特性。因为显式定义的 prop 适用于向一个子组件传入信息，然而组件库的作者并不总能预见组件会被用于怎样的场景。这也是为什么组件可以接受任意的特性，而这些特性会被添加到这个组件的根元素上。

也就是说在默认情况下，父组件定义的一些子组件没有接收的特性会有子组件的根组件继承。
对于绝大多数特性来说，从外部提供给组件的值会替换掉组件内部设置好的值。所以如果传入 type="text" 就会替换掉 type="date" 并把它破坏！庆幸的是，class 和 style 特性会稍微智能一些，即两边的值会被合并起来。
```html
<base-input type="date" class="base-input" placeholder="请输入..." style="color: #ffff00;"></base-input>
```
```html
<!-- 子组件模板 -->
<input type="text" class="my-input">
```
在上面这个例子中，父组件中有 type、class、style 和 placeholder 四个未被子组件接收的特性，这时，子组件的根元素将会继承父组件的这四个特性，其中 class 和 style 比较智能，会将父组件的 class 和 style 的属性值和子组件的根元素本身的 class 和 style 进行合并，不过另外两个会简单的继承，其中 type 的属性值会被替换成父组件的 date，会破环原有的属性值。
### 2. inheritAttrs
从上面的例子中我们知道父组件中的非 props 特性会被子组件的根组件继承，有时会产生负作用，此时我们的 inheritAttrs 就派上用场了。
>如果你不希望组件的根元素继承特性，你可以在组件的选项中设置 inheritAttrs: false。
>注意：注意 inheritAttrs: false 选项不会影响 style 和 class 的绑定。

```javascript
Vue.component('base-input', {
  inheritAttrs: false
})
```
这尤其适合配合实例的 $attrs 属性使用，该属性包含了传递给一个组件的特性名和特性值。
```html
<base-input placeholder="请输入..."></base-input>
```
```javascript
Vue.component('base-input', {
  inheritAttrs: false,
  template: `<label for="name"><input type="text" id="name" v-bind="$attrs"></label>`
})
```
这个时候 placeholder 属性就没有继承在子组件的根元素 label 上了，而是在 input 元素上，从而达到了我们的目的。
## 三. $listeners
### 1.native修饰符
你可能有很多次想要在一个组件的根元素上直接监听一个原生事件。这时，你可以使用 v-on 的 .native 修饰符：
```html
<base-input @focus.native="doSomething"></base-input>
```
在有的时候这是很有用的，不过在你尝试监听一个类似 input 的非常特定的元素时，这并不是个好主意。比如上述 base-input 组件可能做了如下重构，所以根元素实际上是一个 label 元素：
```html
<label>
  {{ label }}
  <input v-bind:value="value" v-on:input="$emit('input', $event.target.value)" >
</label>
```
这时，父级的 .native 监听器将静默失败。它不会产生任何报错，但是 doSomething 处理函数不会如你预期地被调用。
为了解决这个问题，Vue 提供了一个 $listeners 属性，它是一个对象，里面包含了作用在这个组件上的所有监听器：
```javascript
{
  focus: function (event) { /* ... */ }
  input: function (value) { /* ... */ },
}
```
有了这个 $listeners 属性，你就可以配合 v-on="$listeners" 将所有的事件监听器指向这个组件的某个特定的子元素。对于类似 input 的你希望它也可以配合 v-model 工作的组件来说，为这些监听器创建一个类似下述 inputListeners 的计算属性通常是非常有用的。
>父组件的事件添加 .native 修饰符后， $listeners 对象中就不会包含该事件，这个时候该事件被子组件的根元素捕获，无法再指向别的特定子元素。所以如果想让父组件的事件绑定到特定的子元素上，那么就不要加 .native 修饰符。
