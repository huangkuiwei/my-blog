---
title: Vue实例之$mount和el的异同
date: 2018-06-02 18:05:22
author: huangkuiwei
img: http://www.manongjc.com/images/51jb/154648858215Q4G648Pl8582.png
tags: 
    - Vue
    - JavaScript
---
## 一. 用法异同
1.两者在使用效果上没有任何区别，都是为了将实例化后的vue挂载到指定的dom元素中。
2.如果在实例化vue的时候指定el，则该vue将会渲染在此el对应的dom中，反之，若没有指定el，则vue实例会处于一种“未挂载”的状态，此时可以通过$mount来手动执行挂载。
>注意：如果$mount没有提供参数，模板将被渲染为文档之外的的元素，并且你必须使用原生DOM API把它插入文档中，否则没什么意义。
## 二. 实例
```html
<!-- html -->
<div id="app"></div>
```
```javascript
// javascript
let MyComponent = Vue.extend({
  template: '<div>Hello World!</div>'
});

// 创建并挂载到 #app (会替换 #app)
new MyComponent().$mount('#app');

// 同上
new MyComponent({ el: '#app' });

// 或者，在文档之外渲染并且随后挂载
let component = new MyComponent().$mount();
document.getElementById('app').appendChild(component.$el)
```