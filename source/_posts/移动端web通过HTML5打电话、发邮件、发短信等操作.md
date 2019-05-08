---
title: 移动端web通过HTML5打电话、发邮件、发短信等操作
date: 2018-06-09 15:22:08
author: huangkuiwei
img: http://www.logicsolutions.com/wp-content/uploads/2015/06/html5.png
tags: 
    - HTML
---
## 一. 前言
如果需要在移动浏览器中实现拨打电话，调用sms发短信，发送email等功能，移动手机WEB页面(HTML5)Javascript提供的接口是一个好办法。通过url的href链接的方式，实现在各种主流手机浏览器，进行拨打电话等功能。
## 二. 拨打电话
```html
<!-- 最常用的方法 -->
<a href="tel:10010">联系我们</a>

<!-- 使用wtai协议进行拨打电话 -->
<a href="wtai://wp/mc;10010">联系我们</a>
```
>在电话号码前面可以加上 + （加号）表示国际号码。
## 三. 发送短信
```html
<!-- 格式：sms:<phone_number>[,<phone-number>]*[?body=<message_body>] -->

<!-- 给10010发短信 -->
<a href="sms:10010">发送信息</a>

<!-- 给10010发送内容为"1234"的短信 -->
<a href="sms:10010?body=1234">发送信息</a>

<!-- 给10010和10086发送内容为"1234"的短信 -->
<a href="sms:10010,10086?body=1234">发送信息</a>
```
>给多个联系人发送短信时，联系人之间用逗号分隔。
## 四. 发送邮件
```html
<!-- 给hello@163.com发送邮件 -->
<a href="mailto:hello@163.com">发送邮件</a>

<!-- 给hello@163.com和world@126.com发送邮件 -->
<a href="mailto:hello@163.com,world@163.com">发送邮件</a>

<!-- 给hello@163.com发送主题为“testing”的邮件 -->
<a href="mailto:hello@163.com?subject=testing">发送邮件</a>

<!-- 给hello@163.com发送主题为“testing”的邮件，并抄送给world@163.com -->
<a href="mailto:hello@163.com?subject=testing mailto&cc=world@163.com">发送邮件</a>
```
## 五. GPS定位
```html
<!-- 格式：geopoint:[经度],[纬度] -->
<a href="geopoint:108.954823,34.275891">我的位置</a>
```