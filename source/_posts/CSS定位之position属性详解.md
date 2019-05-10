---
title: CSS定位之position属性详解
date: 2018-08-05 22:52:42
author: huangkuiwei
img: http://pic.appvv.com/e410f4c07f84a45d5e78f4fc7a709b54/175.png
tags: 
    - HTML
    - CSS
---
## 一. 前言
position属性规定元素的定位类型。这个属性定义建立元素布局所用的定位机制。任何元素都可以定位，不过绝对或固定元素会生成一个块级框，而不论该元素本身是什么类型。相对定位元素会相对于它在正常流中的默认位置偏移。　　
position属性有4个常用的属性值，分别是absolute、fixed、relative、static。
>1. absolute：生成绝对定位的元素，相对于static定位以外的第一个父元素进行定位。元素的位置和大小通过left、top、right以及bottom属性进行确定。
>2. fixed：生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置和大小通过left、top、right以及bottom属性进行确定。
>3. relative：生成相对定位的元素，相对于其正常位置进行定位。
>4. static：默认值。没有定位，元素出现在正常的流中（**忽略 top, bottom, left, right 或者 z-index 声明**）。

## 二. 属性详解
### 1. relative
>relative：是相对于其正常文本流中的位置进行偏移。

```css
* {
  margin: 0;
  padding: 0;
}
.app {
  position: relative;
  background: sienna;
  width: 200px;
  height: 50px;
  top: 10px;
  left: 20px;
}
```
```html
<div style="height: 100px;background: aqua"></div>
<div class="app">hello world</div>
<div style="height: 100px;background: aqua"></div>
```
![效果图](/medias/postimages/04.png "效果图")
总结：relative是相对正常文档流的位置进行偏移，原先占据的位置依然存在，也就是说它不会影响后面元素的位置。left表示相对原先位置右边进行偏移，top表示相对原先位置下边进行偏移。**当left和right同时存在，仅left有效，当top和bottom同时存在仅top有效**。relative的偏移是基于对象的margin左上侧的。
### 2. absolute
>absolute：元素绝对定位，相对于static定位以外(relative/absolute/fixed)的第一个父元素，如果没有找到则相对于html元素进行定位，元素脱离文档流。

```css
* {
  margin: 0;
  padding: 0;
}
.app {
  position: absolute;
  background: sienna;
  left: 20px;
  right: 20px;
  top: 20px;
}
```
```html
<div style="height: 100px;background: aqua"></div>
<div class="app">hello world</div>
<div style="height: 100px;background: burlywood"></div>
```
![效果图](/medias/postimages/05.png "效果图")
### 3. fixed
>fixed：生成固定定位的元素，相对于浏览器窗口定位，即浏览器窗口滚动也不会影响元素位置，元素的位置与文档流无关，因此不占据空间，可能会和其他元素发生重叠。

```css
* {
  margin: 0;
  padding: 0;
}
.app {
  position: fixed;
  background: sienna;
  left: 20px;
  right: 20px;
  bottom: 0;
}
```
```html
<div style="height: 100px;background: aqua"></div>
<div class="app">hello world</div>
<div style="height: 100px;background: burlywood"></div>
```
![效果图](/medias/postimages/06.png "效果图")
窗口滚动不会影响.app元素位置。