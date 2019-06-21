---
title: 微信小程序 - scroll-view组件设置高度设置百分比的问题
date: 2018-12-23 20:52:22
author: huangkuiwei
img: http://pic.ossfiles.cn:9186/group1/M00/15/1D/rBgIBlx579L_a7lKAAAf12qXaR0168.jpg
tags: 
    - 微信小程序
    - JavaScript
---
`scroll-view`组件是一个可滚动视图区域。使用竖向滚动时，需要给scroll-view一个固定高度，通过 WXSS 设置 height。如果需要组件可以上下滚动，需要设置一个属性 `scroll-y`,当组件内的内容高度超过组件的规定高度时，组件就可以上下滚动。
```html
<scroll-view scroll-y style="height: 200rpx; background: #cccccc">
  <view>Hello World1</view>
  <view>Hello World2</view>
  <view>Hello World3</view>
  <view>Hello World4</view>
  <view>Hello World5</view>
  <view>Hello World6</view>
</scroll-view>
```
![效果图](/medias/postimages/17.gif "效果图")
一切正常，现在要改变一种方式，给 scroll-view 组件设置一个百分比，scroll-view 组件的高度随着它父元素的高度动态变化，代码如下：
```html
<scroll-view scroll-y>
  <view>Hello World1</view>
  <view>Hello World2</view>
  <view>Hello World3</view>
  <view>Hello World4</view>
  <view>Hello World5</view>
  <view>Hello World6</view>
</scroll-view>
```
```css
page {
  height: 100%;
}
scroll-view {
  height: 20%;
  background: #cccccc
}
```
设置页面的高度为100%，scroll-view 组件的高度为页面的20%，实测也没有问题。其实这里的高度还是固定的，就是父元素的20%，我们现在要更近一步，要设置一个 max-height，最高的高度，如果小于这个高度，就不需要上下滚动，要是超出了这个高度，就需要上下滚动了。
修改 scroll-view 组件的样式：
```css
page {
  height: 100%;
}
scroll-view {
  max-height: 20%;
  background: #cccccc
}
```
效果如下：
![效果图](/medias/postimages/18.gif "效果图")
很明显，出问题了，max-height 限制住了组件的最高高度，但是 scroll-view 的 scroll-y 属性并没有生效，没有出现上下的滚动条，我们都知道在 H5 里面可以给需要滚动的元素设置 overflow: auto，元素就会产生滚动条，把这个元素放到这里来试试：
```css
page {
  height: 100%;
}
scroll-view {
  max-height: 20%;
  background: #cccccc;
  overflow-y: auto;
}
```
这时，元素又出现了上下滚动条，而且这个时候 scroll-y 属性都可以省略，照样没问题。