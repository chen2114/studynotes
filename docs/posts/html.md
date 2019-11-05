---
title: 【学习笔记】之html
tags: [学习笔记, html]
categories: [学习笔记]
---
### 初始化
  ``` html
  <!DOCTYPE html>
  <html lang="zh-CN">
  <head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta http-equiv="Cache-Control" content="no-cache, must-revalidate">
    <meta http-equiv="pragma" content="no-cache">
    <meta http-equiv="expires" content="0">
    <meta name="renderer" content="webkit">
    <title></title>
    <!-- CSS -->
    <link rel="stylesheet" type="text/css" href="css/init.css">

    <!-- JS -->
      <script type="text/javascript" src="js/jquery.min.js"></script>
  </head>
    <body>
      
    </body>
  </html>
  ```
### 移动端页面开发常用标签
  ``` html
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0" />
  <!-- 
  device-width - 设备的宽度
  initial-scale - 初始的缩放比例  
  minimum-scale - 允许用户缩放到的最小比例   
  maximum-scale - 允许用户缩放到的最大比例  
  user-scalable - 用户是否可以手动缩放 
  -->
  <meta content="telephone=no,email=no" name="format-detection">
  <!-- 禁止自动识别电话号码和邮箱 -->
  <meta content="yes" name="apple-mobile-web-app-capable">
  <!-- 苹果手机：会删除默认的工具栏和菜单栏，网站开启对web app程序的支持 -->
  <meta name="apple-mobile-web-app-status-bar-style" content="black" />
  <!-- 苹果手机：在web app应用下状态条（屏幕顶部条）的颜色,默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）。 -->
  <meta name="apple-touch-fullscreen" content="yes" />
  <!-- 苹果手机：如果把一个web app添加到了主屏幕中，那么从主屏幕中打开这个web app则全屏显示 -->
  <link rel="apple-touch-icon" href="/static/images/identity/HTML5_Badge_64.png" />
  <!-- 苹果手机：将应用添加到手机主屏幕，会有一个icon可以直接进入 -->
  ```
### flash代码
  ``` html
  <embed src="swf/f1.swf" width="130" height="450" quality="high"wmode="transparent" pluginspage="http://www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash" type="application/x-shockwave-flash">
  <object id="FlashID"classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" width="350" height="109">
  <param name="movie" value="images/logo.swf">
  <param name="quality" value="high">
  <param name="wmode" value="transparent">
  <param name="swfversion" value="11.2.0.0">
  <!--此 param 标签提示使用FlashPlayer6.0 r65 和更高版本的用户下载最新版本的FlashPlayer。如果您不想让用户看到该提示，请将其删除。-->
  <param name="expressinstall" value="/images/expressInstall.swf">
  <!--下一个对象标签用于非 IE 浏览器。所以使用 IECC 将其从 IE 隐藏。-->
  <!--[if!IE]>-->
  <object type="application/x-shockwave-flash" data="images/logo.swf" width="350" height="109">
  <!--<![endif]-->
  <param name="quality" value="high">
  <param name="wmode" value="transparent">
  <param name="swfversion" value="11.2.0.0">
  <param name="expressinstall" value="/images/expressInstall.swf">
  <!--浏览器将以下替代内容显示给使用FlashPlayer6.0和更低版本的用户。-->
  <div>
    <p><imgsrc="images/logo.png"></p>
  </div>
  <!--[if!IE]>-->
  </object>
  <!--<![endif]-->
  </object>
  ```
### iframe
  ``` html
  <iframe frameBorder="0" width="100%" height="100%" marginHeight="0" marginWidth="0" scrolling="no" allowtransparency="true" src="foot.html"></iframe>
  ```