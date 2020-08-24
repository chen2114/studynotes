---
title: 【学习笔记】之scss/sass
tags: [学习笔记, scss]
categories: [学习笔记]
---
### 编译风格
* nested：嵌套缩进的css代码，它是默认值。
* expanded：没有缩进的、扩展的css代码。
* compact：简洁格式的css代码。
* compressed：压缩后的css代码。

生产环境当中，一般使用最后一个选项
```
sass --style compressed test.sass test.css
```
### 变量
``` scss
$blue: #1875e7;
div {
  color: $blue;
}

$side: left;
.rounded {
　border-#{$side}-radius: 5px;
}
```
### 计算功能
``` scss
body {
　margin: (14px/2);
　top: 50px + 100px;
　right: $var * 10%;
}
```
### 嵌套
``` scss
div {
  h1 {
    color: red;
    border: {
      color: red;
      width: 1px;
    }
    &:hover {
      color: #ffb3ff;
    }
  }
}
```
### 注释
ASS共有两种注释风格。
标准的CSS注释 /* comment */ ，会保留到编译后的文件。

单行注释 // comment，只保留在SASS源文件中，编译后被省略。

在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。
### 继承
``` scss
.class1 {
  border: 1px solid #ddd;
}
.class2 {
　@extend .class1;
　font-size:120%;
}
```
### Mixin
``` scss
@mixin left($value: 10px) {
　float: left;
　margin-left: $value;
}
div {
　@include left(20px);
}
```
### 颜色函数
``` scss
lighten(#cc3, 10%) // #d6d65c
darken(#cc3, 10%) // #a3a329
grayscale(#cc3) // #808080
complement(#cc3) // #33c
```
### 插入文件
``` scss
@import "path/filename.scss";
@import "foo.css";
```
### 条件语句
``` scss
p {
　@if 1 + 1 == 2 { border: 1px solid; }
　@if 5 < 3 { border: 2px dotted; }
}
@if lightness($color) > 30% {
　background-color: #000;
} @else {
　background-color: #fff;
}
```
### 循环语句
``` scss
@for $i from 1 to 10 {
　.border-#{$i} {
　　border: #{$i}px solid blue;
　}
}

$i: 6;
@while $i > 0 {
　.item-#{$i} { width: 2em * $i; }
　$i: $i - 2;
}

@each $member in a, b, c, d {
　.#{$member} {
　　background-image: url("/image/#{$member}.jpg");
　}
}
```
### 自定义函数
``` scss
@function double($n) {
　@return $n * 2;
}
#sidebar {
　width: double(5px);
}
```