---
title: 【学习笔记】之css
tags: [学习笔记, css]
categories: [学习笔记]
---
### 鼠标悬停翻转X 翻转Y 旋转翻转X
  ``` css
  .flip-x {transition: .6s ease-in-out;}
  .flip-x:hover {transform: rotateX(360deg);}
  ```
### 翻转Y
  ``` css
  .flip-y {transition: .6s ease-in-out;}
  .flip-y:hover {transform: rotateY(360deg);}
  ```
### 旋转360°
  ``` css
  .rotate360 {transition: .6s ease-in-out;}
  .rotate360:hover {transform: rotate(360deg) scale(1.25);}
  ```
### 文字单行超出…
  ``` css
  {white-space:nowrap; overflow: hidden; text-overflow: ellipsis;}
  ```
### 文字多行超出…
  ``` css
  {display: -webkit-box;-webkit-box-orient: vertical;-webkit-line-clamp: 2; overflow: hidden;}
  ```
### Flex布局【IE11+】
  >注意：设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效
#### 设置在容器上的属性
  * flex-direction: row | row-reverse | column | column-reverse; item的排列方向
  * flex-wrap: nowrap | wrap | wrap-reverse; 换行方式
  * flex-flow: row nowrap; flex-direction和flex-wrap的简写
  * justify-content: flex-start | flex-end | center | space-between | space-around; item在主轴上的对齐方式
  * align-items: flex-start | flex-end | center | baseline | stretch; item在交叉轴上如何对齐
  * align-content: flex-start | flex-end | center | space-between | space-around | stretch; 多根轴线的对齐方式,如果项目只有一根轴线，该属性不起作用
#### item的属性
  * order: 0; 定义item的排列顺序
  * flex-grow: 0; 定义item的放大比例
  * flex-shrink: 1; 定义item的缩小比例
  * flex-basis: auto; 定义了在分配多余空间之前，item占据的主轴空间
  * flex: 0 1 auto; flex-grow, flex-shrink 和 flex-basis的简写
  * align-self: auto | flex-start | flex-end | center | baseline | stretch; 允许单个iem有与其他项目不一样的对齐方式，可覆盖align-items属性
### div模拟table
  ``` css
  .eui-display-table {display: table; width:100%;}
  .eui-display-table .thead {display: table-header-group;}
  .eui-display-table .tbody {display: table-column-group;}
  .eui-display-table .tfoot {display: table-footer-group;}
  .eui-display-table .tr {display: table-row;}
  .eui-display-table .th {display: table-cell; vertical-align: middle;}
  .eui-display-table .td {display: table-cell; vertical-align: middle;}
  ```
### 文字单行时居中，超过单行时居左
  ``` html
  <div class="eui-text-align-center">
    <div class="con">
      
    </div>
  </div>
  ```
  ``` css
  .eui-text-align-center {text-align: center;}
  .eui-text-align .con {display:inline-block; text-align: left;}
  ```
  
### 图片或背景变灰色
  ``` css
  img {-webkit-filter: grayscale(1);}
  a {-webkit-filter: grayscale(1); cursor: no-drop;}
  ```
### 输入框placeholder样式
  ``` css
  input::-webkit-input-placeholder {color: #4d8fd9;}
  ```
### 插入图片实现cover
  ``` css
  {display: block; width: 100%; height: 100%;object-fit: cover;}
  ```
### content获取属性值
  ``` html
  <a href="#" data-label="Country scenery"></a>
  ```
  ``` css
  a:before {content: attr(data-label);}
  ```
### 底部自适应
  内容不超过一屏时，底部固定在浏览器底部，内容超过一屏时，底部跟随在页面之后，将所有页面内容放在写了padding-bottom的`<div class="eui-con">`中，padding-bottom的值与底部的高度相同
  ``` html
  <body>
	  <div class="eui-con">
		  ……
      <div class="eui-footer">
        
      </div>
    </div>
  </body>
  ```
  ``` css
  html, body {height: 100%;}
  body {position: relative; min-height: 100%; height: auto !important; height: 100%}
  .eui-con {padding-bottom: 50px;}
  .eui-footer {position: absolute; bottom: 0; width: 100%; height: 50px; overflow: hidden;}
  ```
### 鼠标悬停图片放大
  ``` css
    a img {display: block;-webkit-transition: 0.5s,-webkit-transform 0.5s; transition: 0.5s, transform 0.5s;}
    a:hoverimg {-webkit-transform: scale3d(1.1,1.1,1); transform: scale3d(1.1,1.1,1);}
  ```
### 滚动条样式修改
  ``` css
  ::-webkit-scrollbar {width: 8px; height: 8px;} /* 定义滚动条高宽及背景高宽分别对应横竖滚动条的尺寸 */
  ::-webkit-scrollbar-track {background: none;} /* 定义滚动条轨道内阴影+圆角 */
  ::-webkit-scrollbar-thumb {border-radius: 10px; background-color: rgba(0,0,0,.15);} /* 定义滑块内阴影+圆角 */
  ```
### iframe
  ``` html
  <iframe frameBorder="0" width="100%" height="100%" marginHeight="0" marginWidth="0" scrolling="no" allowtransparency="true" src="foot.html"></iframe>
  ```
### 鼠标经过半透明
  ``` css
  .hover-opacity a:hover {filter: alpha(opacity=75); opacity: 0.75;}
  ```
### 鼠标经过图片半透明
  ``` css
  .hover-opacity a img {transition: 0.2s;}
  .hover-opacity a:hover img {filter: alpha(opacity=75); opacity: 0.75;}
  ```
### CSS三角形
  ``` css
  {border-width: 6px;border-style: solid; border-color: transparent transparent#fff transparent;}
  ```
### pie.htc
  ``` css
  {behavior:url(css/pie.htc);}
  ```
### IE8背景图全屏拉伸
  ``` css
  {background: url(../images/bg.jpg) no-repeat top center fixed; background-size: cover; filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='images/bg.jpg',sizingMethod='scale');}
  ```
### 媒介
  ``` css
  @media screen and(min-width: 300px) {

  }
  @media screen and(max-width: 480px) {

  }
  ```
### 表格单元格防止撑开
  ``` css
  table {table-layout: fixed;word-wrap: break-word;word-break;break-all;}
  ```
### CSS Hack大全
  ``` html
  <!--[iflt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7"><![endif]--> IE7以下版本
  <!--[if IE 7]><html class="no-js lt-ie9 lt-ie8"><![endif]--> IE7
  <!--[if IE 8]><html class="no-js lt-ie9"><![endif]--> IE8
  <!--[ifgt IE 8]><html class="no-js"><![endif]--> IE8以上
  ```
  ``` css
  /* webkit and opera */
  @media all and(min-width:0) {
    .content.test {background: #0f0;}
  }
  /* webkit */
  @media screen and(-webkit-min-device-pixel-ratio: 0) {
    .content.test {background: #ff0;}
  }
  /*FireFox*/
  @-moz-document url-prefix() {
    .content.test {background: #f0f;}
  }
  /*IE6、IE7、IE8*/
  @media\0screen\,screen\9 {
    .selector{property: value;}
  }
  /*IE9+*/
  @media all and(min-width:0){
    .content.test{background: #f009;}
  }
  /*IE10+*/
  @media screen and(-ms-high-contrast: active),(-ms-high-contrast: none) {
    .content.test {background: #0ff;}
  }
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
