---
title: 微信小程序 - 获取和设置cookie
date: 2019-08-21 16:23:54
author: huangkuiwei
img: http://pic.ossfiles.cn:9186/group1/M00/15/1D/rBgIBlx579L_a7lKAAAf12qXaR0168.jpg
tags: 
    - 微信小程序
    - JavaScript
---
### 1. 前言
小程序开发中我们需要获取到后端给的cookie进行请求验证,但是微信并没有帮我们保存cookie,那么我们要维持会话需要自己来保存cookie,并且请求的时候加上cookie

### 2. 获取cookie
在登录请求后读取返回值的header的cookie,并本地存储。其实一般情况下这里的cookie存的就是一个sessionId。
```javascript
App({
  getUser() {
    // request 是已经封装好了的请求方法
    return request.post('登录接口/地址', {
      // ... 请求参数
    }).then(data => {
      // 获取到接口返回的 sessionId
      if (data.header['Set-Cookie']) {
        let sessionId = data.header['Set-Cookie'].split(';')[0];
        wx.setStorageSync('sessionId', sessionId)
      }
      return data.data.datas
    })
  }
});
```

### 3. 请求带上cookie
```javascript
// request.js
 wx.request({
  // ...其它参数省略
  // 发送请求时，要将 cookie 同请求发送到后台
  header: {
    'cookie': wx.getStorageSync("sessionId"),
    // ...
  },
 })
```
