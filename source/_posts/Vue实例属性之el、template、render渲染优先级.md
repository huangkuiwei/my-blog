---
title: Vue实例属性之el、template、render渲染优先级
date: 2018-06-01 09:20:04
author: huangkuiwei
img: http://www.linuxeden.com/wp-content/uploads/2017/07/vihz5fczfwkyqo0z1200.jpg
tags: 
    - Vue
    - JavaScript
---
## 一. 优先级
1.当Vue选项对象中有render渲染函数时，Vue构造函数将直接使用渲染函数渲染DOM树。
2.当选项对象中没有render渲染函数时，Vue构造函数首先通过将template模板编译生成渲染函数，然后再渲染DOM树。
3.当Vue选项对象中既没有render渲染函数，也没有template模板时，会通过el属性获取挂载元素的outerHTML来作为模板，并编译生成渲染函数。
>换言之，在进行DOM树的渲染时，render渲染函数的优先级最高，template次之且需编译成渲染函数。如果挂载点el属性对应的元素存在，而且前两者均不存在时，其outerHTML才会用于编译与渲染。
## 二. 实例
```html
<!-- html -->
<div class="app-1">{{ info }}</div>
<div class="app-2">{{ info }}</div>
<div class="app-3">{{ info }}</div>
```
```javascript
// javascript
new Vue({
  el: '.app-1',
  data: {
    info: '这是通过el属性获取挂载元素的outerHTML方式渲染。'
  },
  template: '<div>这是template属性模板渲染。</div>',
  render(h){
    return h('div', {}, '这是render属性方式渲染。')
  }
});

new Vue({
  el: '.app-2',
  data: {
    info: '这是通过el属性获取挂载元素的outerHTML方式渲染。'
  },
  template: '<div>这是template属性模板渲染。</div>'
});

new Vue({
  el: '.app-3',
  data: {
    info: '这是通过el属性获取挂载元素的outerHTML方式渲染。'
  }
})
```
三种方式渲染结果：
1.这是render属性方式渲染。
2.这是template属性模板渲染。
3.这是通过el属性获取挂载元素的outerHTML方式渲染。