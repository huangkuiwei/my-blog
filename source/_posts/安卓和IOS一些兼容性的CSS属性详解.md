---
title: 安卓和IOS一些兼容性的CSS属性详解
date: 2018-12-21 21:45:15
author: huangkuiwei
img: http://oshup.com/wp-content/uploads/2018/04/Learn-HTML5-and-CSS3-languages-for-Dummies.jpg
tags: 
    - HTML
    - CSS
---
博主在做移动端网页开发的时候，经常被安卓和IOS的一些兼容性属性折磨，想想就头痛...，所以我觉得有必要总结一下。
### 1. -webkit-text-size-adjust
>1. 这个属性并不是标准。 你必须为每个你想要应用的浏览器加上属性前缀。该属性具有继承性。
>2. -webkit-text-size-adjust 的本职是用于mobile的。
>3. iPhone 和 Android 的浏览器纵向和橫向模式皆有自动调整字体大小的功能。控制它的就是 CSS 中的 -webkit-text-size-adjust。-webkit-text-size-adjust 设为 none 或者 100% 关闭字体大小自动调整功能。
>4. 如果不想让iPhone横坚屏切换的时候调节文字，用 -webkit-text-size-adjust: 100%，不要用 none 属性，这会导致支持 none 属性的浏览器无法人为放大文字大小，严重影响可用性。
>5. 移动端浏览器网页的最小字体并没有统一，iPhone 和 Android的表现也不一样，有的对字体没有限制，有的最小只能到 12px，所以在项目中字体不要去设置小于 12px。
>6. 如果确实有需要将字体设置为小于 12px，那么可以用 css3 的属性 — transform；比如：transform: scale(0.8)。

```css
html {
  -webkit-text-size-adjust: 100%;
}
```
```html
<!-- 将字体大小设置为小于 12px -->
<p style="font-size: 12px; transform: scale(.85)">Hello World</p>
```
### 2. -webkit-overflow-scrolling
>1. 作用：-webkit-overflow-scrolling 属性控制元素在移动设备上是否使用滚动回弹效果.
>2. 属性值：auto 使用普通滚动, 当手指从触摸屏上移开，滚动会立即停止。
>3. 属性值：touch 使用具有回弹效果的滚动, 当手指从触摸屏上移开，内容会继续保持一段时间的滚动效果。继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。同时也会创建一个新的堆栈上下文。

使用场景：在苹果手机上使用overflow-y：scroll/auto，会使该属性的盒子滑动非常不流畅，会有卡顿的现象。在用了 -webkit-overflow-scrolling: touch 之后会解决 IOS 滑动卡顿的问题。
使用了 -webkit-overflow-scrolling: touch 属性之后会有一些bug，包括但不限于一下几种：
>1. 滚动过程中 scrollTop 属性不会变化，滚动条停下才会变化。
>2. 通过动态添加内容撑开容器，结果根本不能滑动的bug。
>3. 手势可穿过其他元素触发元素滚动。
>4. 会导致使用固定定位的元素，随着页面一起滚动，只有滚动停止时才会恢复原位。

```css
div {
  height: 200px;
  width: 100%;
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
}
```
### 3. -webkit-appearance
>1. -webkit-appearance 是一个 不规范的属性，它没有出现在 CSS 规范草案中。此属性非标准且渲染效果在不同浏览器下不同，有些属性值甚至不支持，要慎用。
>2. 作用：改变按钮和其他控件的外观，使其类似于原生控件。
>3. 在 IOS 浏览器中，一些元素，例如 input，有着默认的按钮样式。无法去修改，这个时候可以使用：-webkit-appearance: none;去掉苹果手机的按钮默认样式。

```css
input[type="buttom"], input[type="reset"], input[type="submit"], button {
  -webkit-appearance: none;
}
```
注意：如果在 radio 和 checkbox 中应用 -webkit-appearance: none;会使选择框消失。可以设置 -webkit-appearance: checkbox;进行覆盖。
```css
input[type="checkbox"] {
  -webkit-appearance: checkbox;
}
input[type="radio"] {
  -webkit-appearance: radio;
}
```
### 4. -webkit-tap-highlight-color
>1. 这个属性只用于iOS (iPhone和iPad)。当你点击一个链接或者通过Javascript定义的可点击元素的时候，它就会出现一个半透明的灰色背景。要重设这个表现，你可以设置-webkit-tap-highlight-color为任何颜色。
>2. 想要禁用这个高亮，设置颜色的alpha值为0即可。也可以设置 transparent(透明的)。

```css
a {
  -webkit-tap-highlight-color: transparent;
}
```