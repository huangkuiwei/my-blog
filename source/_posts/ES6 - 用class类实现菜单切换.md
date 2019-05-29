---
title: ES6 - 用class类实现菜单切换
date: 2018-09-28 21:14:55
author: huangkuiwei
img: http://5b0988e595225.cdn.sohucs.com/images/20170910/4b1328ea36b14afa8a30a69f32eb9a44.jpeg
tags: 
    - JavaScript
    - ES6
---
下面是用 ES6 新特性 class 类写的一个简单易懂的切换菜单功能。
```html
<!-- html -->
<div id="menu">
  <button>选项一</button>
  <button>选项二</button>
  <button>选项三</button>
  <div class="content">选项一内容</div>
  <div class="content">选项二内容</div>
  <div class="content">选项三内容</div>
</div>
```

```css
/* css */
* {
  margin: 0;
  padding: 0;
}
button {
  padding: 5px;
}
.content {
  height: 200px;
  width: 200px;
  border: 1px solid #ccc;
}
```

```javascript
// JavaScript
class Menu {
  // 在实例化新对象的时候，constructor 构造函数会自动运行。
  constructor(obj) {
    this.menu = document.getElementById(obj);
    this.btn = this.menu.querySelectorAll('button');
    this.div = this.menu.querySelectorAll('div');
    this.colorList = ['red', 'green', 'blue'];
    this.index = 0;
    this.init();      // 初始化
    for (let i = 0; i < this.btn.length; i++) {     // 添加事件
      this.btn[i].onclick = () => {
        this.index = i;
        this.hide();
        this.show()
      }
    }
  }

  init() {
    for (let i = 0; i < this.div.length; i++) {
      this.div[i].style.display = 'none';
    }
    this.div[this.index].style.display = 'block';
    this.div[this.index].style.background = this.colorList[this.index];
    this.btn[this.index].style.background = this.colorList[this.index]
  }

  hide() {
    for (let i = 0; i < this.div.length; i++) {
      this.div[i].style.display = 'none';
      this.btn[i].style.background = null
    }
  }

  show() {
    this.div[this.index].style.display = 'block';
    this.div[this.index].style.background = this.colorList[this.index];
    this.btn[this.index].style.background = this.colorList[this.index]
  }
}

new Menu('menu')
```
![效果图](/medias/postimages/08.gif "效果图")