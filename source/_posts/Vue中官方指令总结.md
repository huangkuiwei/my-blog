---
title: Vue中官方指令总结
date: 2018-09-15 21:21:46
author: huangkuiwei
img: http://www.manongjc.com/images/51jb/154648858215Q4G648Pl8582.png
tags: 
    - Vue
    - JavaScript
---
### 1. v-html
>1. `v-html`可以将元素的内容渲染成该元素绑定的属性值，直接作为HTML，因此使用了该指令的元素之间的内容会被忽略，而且该指令会忽略解析属性值中的数据绑定。
>2. `v-html`后面可以跟任意 JavaScript 表达式。不过最后渲染时都会将表达式的返回值转换为字符串，展现在视图上，所以你可以写`<div v-html="myHtml+{}"></div>`也不会报错，他会将对象转换为字符串格式。
>3. 可以通过 `>>>` 符号来选择到属性值的根元素，进而也可以选择修改更深的元素。

```html
<template>
  <div class="app">
    <div v-html="myHtml+{}"></div>
    <div v-html="myHtml"></div>
    <div v-html="myOtherHtml()"></div>
  </div>
</template>

<script >
  export default {
    data(){
      return {
        myHtml: `<div style="color: #f00"><span>hello world</span></div>`
      }
    },
    methods: {
      myOtherHtml() {
        return `<p>这是一个函数返回值</p>`
      }
    }
  }
</script>

<style scoped>
  .app >>> div {
    font-size: 20px;
    > span {
      color: darkblue;
    }
  }
</style>
```
![效果图](/medias/postimages/10.png "效果图")
>注意：你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值。

### 2. v-bind
Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 `v-bind` 指令：
```html
<div v-bind:id="app">Hello World</div>
```
对于布尔特性 (它们只要存在就意味着值为 true)，v-bind 工作起来略有不同，在这个例子中：
```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```
如果 isButtonDisabled 的值是 null、undefined 或 false，则 disabled 特性甚至不会被包含在渲染出来的 button 元素中。
>同样，`v-bind`指令后面同样可以跟任意 JavaScript 表达式

### 3. v-if
>1. `v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 `truthy` 值的时候被渲染
>2. 也可以用 `v-else` 添加一个“else 块”
>3. `v-else-if`，顾名思义，充当 v-if 的“else-if 块”，可以连续使用
>4. 可以在 `<template>` 元素上使用 v-if 条件渲染分组
>5. 用 `key` 管理可复用的元素，如果没有添加 `key` 属性，那么切换的时候会高效复用相同的元素
>6. `key` 表示：这两个元素是完全独立的，不要复用它们
>7. `v-if` 后面可以跟任意表达式，根据表达式返回的值是否是 `truthy`，来确定是否确定是否要渲染。（可以理解为表达式最后的值都会通过函数 `Boolean()` 来转换为布尔值，为 `true` 就渲染，反之就不渲染）

### 4. v-show
另一个用于根据条件展示元素的选项是 v-show 指令。用法大致一样，不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
> 1. 注意，v-show 不支持 `<template>` 元素，也不支持 `v-else`。

v-if vs v-show 的对比：
>1. `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
>2. `v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
>3. 相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
>4. 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

### 5. v-for
我们可以用 ` v-for` 指令基于一个数组来渲染一个列表。v-for 指令需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据数组，而 `item` 则是被迭代的数组元素的别名。
```html
<template>
  <div>
    <div v-for="item in list">
      my name is {{item.name}},age is {{item.age}}
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        list: [
          {name: 'huang', age: 24},
          {name: 'zhang', age: 33},
          {name: 'li', age: 12}
        ]
      }
    }
  }
</script>
```
![效果图](/medias/postimages/11.png "效果图")
在 `v-for` 块中，我们可以访问所有父作用域的属性。v-for 还支持一个可选的第二个参数，即当前项的索引。
```html
<template>
  <div>
    <div v-for="(item, index) in list">
      my name is {{item.name}},age is {{item.age}}
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        list: [
          {name: 'huang', age: 24},
          {name: 'zhang', age: 33},
          {name: 'li', age: 12}
        ]
      }
    }
  }
</script>
```
你也可以用 `of` 替代 `in` 作为分隔符，因为它更接近 JavaScript 迭代器的语法。
`v-for` 语法同样可以应用在对象中：
```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```
>1. 注意：`v-for`的表达式特殊，必须是`item in items`的格式，不能随便写了。
>2. 建议尽可能在使用 `v-for` 时提供 key attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
>3. 不推荐在同一元素上使用 `v-if 和 v-for`。
>4. 当 `v-if 和 v-for` 处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。
>5. 在自定义组件上，你可以像在任何普通元素上一样使用 v-for 。
>6. 当在组件上使用 v-for 时，key 现在是必须的。

### 6. v-on
可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。
需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法：
```html
<template>
 <button v-on:click="warn('Form cannot be submitted yet.', $event)">
   Submit
 </button>
</template>

<script >
  export default {
    methods: {
    warn: function (message, event) {
      // 现在我们可以访问原生事件对象
      if (event) event.preventDefault();
      alert(message)
    }
  }
</script>
```