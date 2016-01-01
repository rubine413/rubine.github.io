---
title: HTML中meta标签用法及示例
date: 2015-10-12 14:15:21
tags: HTML
categories: HTML
toc: true
---

### 说明
[`<meta>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)标签是位于HTML文档`<head>`标签中的一个辅助性标签。它常用于定义网页文档的属性，如作者、关键字、日期、网页描述及其它的元数据等，这些元数据将服务于浏览器（如何布局或重载页面），搜索引擎和其它网络服务。

---
### 属性及用法示例
`<meta>`常用的属性主要有四个：`http-equiv`、`name`、`content`和`charset`。不同的属性有不同的参数值，这些不同的参数值就实现了不同的网页功能。其中`http-equiv`顾名思义，常用来做http协议上的一些限制。`content`属性用于指定`http-equiv`或`name`属性所设参数的具体值，`charset`为HTML5中新增属性，用于声明当前文档所使用的字符编码。

+ #### charset
  html5新增属性，声明当前文档所使用的字符编码
``` html
<meta charset="utf-8">
```
    相当于HTML 4.01中的如下代码
    ```html
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    ```
<!--more-->
+ #### name
    + ##### `author`
    文档作者
    ```html
    <meta name="author" content="name, email@gmail.com">
    ```
    + ##### `keywords`
    页面关键词
    ```html
    <meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
    ```
    + ##### `description`
    页面描述
    ```html
    <meta name="description" content="网页描述">
    ```
    + ##### `generator`
    生成页面的软件的标识符
    ```html
    <meta name="generator" content="Sublime Text3">
    ```
    + ##### `robots`
    定义爬虫工具的采集行为
    ```html
    <meta name="robots" content="index">
    <meta name="robots" content="noindex">
    <meta name="robots" content="follow">
    <meta name="robots" content="nofollow">
    ```
    + ##### `viewport`
    定义视窗初始大小及缩放比例，仅供移动设备使用
    ```html
    <meta name="viewport" content="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no">
    ```
+ #### http-equiv
    + ##### `Content-Type`
    指定文档的类型及编码格式，HTML5中请直接使用[`charset`](#charset)指定编码
    ```html
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    ```
    + ##### `X-UA-Compatible`
    设置浏览器以何种版本渲染当前页面
    ```html
    <!-- 指定IE和Chrome使用最新版本渲染当前页面-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    ```
    + ##### `refresh`
    自动刷新与跳转(重定向)页面
    ```html
    <!-- 3秒后刷新页面 -->
    <meta http-equiv="refresh" content="3" />
    <!-- 3秒后跳转到指定页面 -->
    <meta http-equiv="refresh" content="3;url=http://blog.csdn.net/haoaiqian" />
    ```
    + ##### `Expires`
    网页到期时间，到期后从服务器重新获取页面
    ```html
    <meta http-equiv="Expires" content="Wed, 21 Oct 2015 07:28:00 GMT">
    ```
    + ##### `Set-Cookie`
    设定cookie，如果网页过期，那么这个网页存在本地的cookies也会被自动删除
    ```html
    <meta http-equiv="Set-Cookie" content="User=Rubine; path=/; expires=Wed, 21 Oct 2015 07:28:00 GMT">
    ```
    + ##### `Cache-Control`
    指定请求和响应遵循的缓存机制，有以下几种设置：
        - no-cache：先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。
        - no-store：不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施）
        - public：缓存所有响应，但并非必须。因为max-age也可以做到相同效果
        - private：只为单个用户缓存，因此不允许任何中继进行缓存。（比如说CDN就不允许缓存private的响应）
        - max-age：表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。例如：max-age=60表示响应可以再缓存和重用 60 秒。

    ```html
    <meta name="Cache-Control" content="no-cache">
    ```
    注意，如果`Cache-Control`设置的max-age和[`Expires`](#Expires)同时存在，则后者被忽略
---
### 其他特殊设备或浏览器示例
``` html
<!-- ios添加智能 App 广告条 Smart App Banner（Safari） -->
<meta name="apple-itunes-app" content="app-id=550854415">

<!-- 设置苹果工具栏颜色 -->
<meta name="apple-mobile-web-app-status-bar-style" content="black"/>

<!-- 双核浏览器的渲染方式 -->
<!-- 默认webkit内核 -->
<meta name="renderer" content="webkit">
<!-- 默认IE兼容模式 -->
<meta name="renderer" content="ie-comp">
<!-- 默认IE标准模式 -->
<meta name="renderer" content="ie-stand"> 

<!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
<meta name="HandheldFriendly" content="true">

<!-- 微软的老式浏览器 -->
<meta name="MobileOptimized" content="320">
    
<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">

<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">

<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">

<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">

<!-- UC应用模式 -->
<meta name="browsermode" content="application">

<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">

<!-- windows phone 点击无高光 -->
<meta name="msapplication-tap-highlight" content="no">
 
<!-- Windows 8 磁贴颜色 -->
<meta name="msapplication-TileColor" content="#000"/>

<!-- Windows 8 磁贴图标 -->
<meta name="msapplication-TileImage" content="icon.png"/>
```