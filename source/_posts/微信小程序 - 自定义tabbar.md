---
title: 微信小程序 - 自定义tabbar
date: 2019-02-09 23:21:56
author: huangkuiwei
img: http://pic.ossfiles.cn:9186/group1/M00/15/1D/rBgIBlx579L_a7lKAAAf12qXaR0168.jpg
tags: 
    - 微信小程序
    - JavaScript
---
现在微信支持自定义 `tabbar` 了，这对于那些需要自己定义 tab 栏的开发者来说时一个大好事，因为可以不要再使用这万年不变的 tabbar 样式了。
#### 1. 配置信息：custom: true
>1. 在 app.json 中的 tabBar 项指定 `custom` 字段，同时其余 tabBar 相关配置（`主要是 list 字段要完整`）也补充完整。
>2. 所有 tab 页的 json 里需声明 usingComponents 项，也可以在 app.json 全局开启。

```json
{
  "tabbar": {
    "custom": true,
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "栏目1"
      },
      {
        "pagePath": "pages/logs/logs",
        "text": "栏目2"
      }
    ]
  }
}
```

#### 2. 新建 tabbar 代码文件
>1. custom-tab-bar/index.js
>2. custom-tab-bar/index.json
>3. custom-tab-bar/index.wxml
>4. custom-tab-bar/index.wxss

上面的文件以及命名是不能自己去更改的，否则小程序找不到对应的 tabbar 代码文件。

#### 3. 编写代码
编写代码和常规的方式差不多，不过还是有些地方要注意：
>1. 开发者需要提供一个`自定义组件`来渲染 tabBar，所有 tabBar 的样式都由该自定义组件渲染。
>2. 推荐用 fixed（实测：`不使用 fixed 定位把组件固定在在页面的话，会存在一些问题`） 在底部的 `cover-view + cover-image` 组件渲染样式，以保证 tabBar 层级相对较高。
>3. 如需实现 tab 选中态，要在当前页面下，通过 `getTabBar` 接口获取组件实例，并调用 setData 更新选中态。
>4. `getTabBar 接口只有在自定义 tabbar 的时候才能正常调用`。

实现 tab 选中态实例：
```javascript
// tabbar: custom-tab-bar/index.js
Component({
  data: {
    // 默认选中的 tabbar
    selected: 0,
    list: [
      {
        pagePath: '/pages/index/index',
        text: '栏目一'
      },
      {
        pagePath: '/pages/logs/logs',
        text: '栏目二'
      }
    ]
  },
  methods: {
    switchTab(e) {
      let url = e.currentTarget.dataset.path;
      wx.switchTab({
        url
      })
    }
  }
})
```
```html
<!-- tabbar: custom-tab-bar/index.wxml -->
<cover-view>
  <!-- 这里 class 为 active 的时候为该 tab 的选中态，样式和别的 tab 不一样，默认就是 selected 为 0，就是第一个 tab 栏 -->
  <cover-view 
    bindtap="switchTab" 
    wx:for="{{list}}" 
    wx:key="index" 
    class="{{selected === index ? 'active' : ''}}"
    data-path="{{item.pagePath}}"
  >
    {{item.text}}
  </cover-view>
</cover-view>
```
```css
/* tabbar: custom-tab-bar/index.wxss */
/* 样式就省略了，要记得使用 position: fixed */
```
其它 tab 页面：
```javascript
Page({
  onLoad() {
    // 获取当前 tabbar 实例，配合 setData 修改 tab 的选择态
    this.getTabBar().setData({
      // 将 selected 修改为要打开页面的 index
      selected: 1
    })
  }
})
```


















