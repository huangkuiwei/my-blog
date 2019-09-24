---
title: 给行内元素设置padding、margin有用吗？
date: 2018-06-29 22:22:45
author: huangkuiwei
img: http://oshup.com/wp-content/uploads/2018/04/Learn-HTML5-and-CSS3-languages-for-Dummies.jpg
tags: 
    - HTML
    - CSS
---
&emsp;&emsp;我们都知道行内元素是不能设置width和height的，想要试着width和height就需要把行内元素的display属性设置成block、inline-block等属性，那么要是直接给行内元素设置padding和margin会产生效果吗？
### 一. 行内元素有盒子模型吗？
行内元素和其它元素一样有盒子模型。
### 二. 先说结论
1. 行内元素的padding-top、padding-bottom、margin-top、margin-bottom属性设置是无效的。
2. 行内元素的padding-left、padding-right、margin-left、margin-right属性设置是有效的。
3. 行内元素的padding-top、padding-bottom从显示的效果上是增加的，但其实设置的是无效的。并不会对他周围的元素产生任何影响。
 
### 三.实例
```html
<!-- html -->
<div>
  <p>我是p元素</p>
  <span>我是span元素</span><span>我是span元素</span>
</div>
``` 
```css
/* css */
* {
  margin: 0;
  padding: 0;
}
p {
  border: 1px solid #cccccc;
}
span {
  background: #f00;
  padding: 5px 10px;
  margin: 5px 10px;
}
``` 
![效果图](/medias/postimages/01.png "效果图")
从效果图可以看到，padding-left、padding-right、margin-left、margin-right是有效的，margin-top、margin-bottom是无效的，padding-top、padding-bottom有点奇怪，效果上好像是有了，因为好像行内元素padding部分的背景生效了，不过这只是表面上的效果，其实并没有产生作用，因为它并没有真正产生外边距效果，所以事实上是无效的。
