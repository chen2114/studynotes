---
title: 【学习笔记】之less
tags: [学习笔记, less]
categories: [学习笔记]
---
### 变量
``` less
@blue: #5B83AD;
.header {
  color: @blue;
}

@side: left;
.rounded {
　border-@{side}-radius: 5px;
}
```
### 计算功能
``` less
@base: 10%;
@filter: @base * 2;
@other: @base + @filter;

color: #888 / 4;
background-color: @base=color + #111;
height: 100% / 2 + filterl;

@var: 1px + 5; // 6px
width: (@var + 5 ) *2; // 被允许使用括号
border: (@width * 2) solid black; // 可以在符合属性中进行使用
```
### 嵌套
``` less
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
    &:hover { text-decoration: none }
  }
}
```
### 继承
``` less
.center{
  display: flex;
  justify-content: center;
  align-items: center;
}

.div:extend(.center) {
  width: 400px;
}
```
### 颜色函数
``` less
lighten(@color, 10%);     // 返回一个比@color低10％更轻的颜色
darken(@color, 10%);      // 返回一个比@color高10％较暗的颜色
saturate(@color, 10%);    // 返回比@color多饱和度10％的颜色
desaturate(@color, 10%);  // 返回一个比@color少饱和度10％的颜色
fadein(@color, 10%);      // 返回一个比@color少10％透明度的颜色
fadeout(@color, 10%);     // 返回一个比@color多10％透明度的颜色
fade(@color, 50%);        // 返回一个颜色透明度为50％的颜色
spin(@color, 10);         // 返回色调比@color大10度的颜色
spin(@color, -10);        // 返回一个比@color小10度色调的颜色
mix(@color1, @color2);    // 返回一个混合@ color1和@ color2的颜色
```
### Math函数
``` less
round(1.67); // returns `2`
ceil(2.4);   // returns `3`
floor(2.6);  // returns `2`
percentage(0.5); // returns `50%`
```
### Mixin
``` less
// 带参数的混合
.border-radius (@radius: 5px) {
        border-radius: @radius;
   -moz-border-radius: @radius;
-webkit-border-radius: @radius;
}
#header {
  .border-radius(4px);
}

.box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
  box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @arguments;
}
.box-shadow(2px, 5px);

// 匹配模式
.mixin (dark, @color) {
  color: darken(@color, 10%);
}
.mixin (light, @color) {
  color: lighten(@color, 10%);
}
.mixin (@_, @color) {// @_ 接受任意值
  display: block;
}

.class {
  .mixin(light, #888);
}
```
### 插入文件
``` less
@import "lib.less";
@import "lib";
@import "lib.css";
```
### 导引表达式
``` less
.generate-font(@n, @i: 12) when (@i <= @n) {
  .fs@{i} {
    font-size: @i * 1px;
  }
  .generate-font(@n, (@i + 1));
}

.generate-font(40);
// 运行结果
// .fs12 { font-size: 12px } .fs13 { font-size: 13px }......fs40 { font-size: 40px }
```
### 避免编译
有时候我们需要输出一些不正确的CSS语法或者使用一些 less不认识的专有语法。要输出这样的值我们可以在字符串前加上一个 ~，并将要避免编译的值用 “”包含起来。
``` less
.class {
  filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
}
```
### JavaScript表达式
``` less
@var: `"hello".toUpperCase() + '!'`; // @var :"HELLO!"

@str: "hello";
@var: ~`"@{str}".toUpperCase() + '!'`; //@var: HELLO!;

@height: `document.body.clientHeight`;

@color: color(`window.colors.baseColor`);
```