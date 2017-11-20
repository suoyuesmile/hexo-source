---
title: CSS 水平垂直居中的6种常用方法
date: 2015-10-25 18:25:43
categories: CSS
tags: [CSS, 垂直居中]
---
学了前端之后，也应该有一些收获了，现在写一些简单 demo， 练下手

前端布局中最常见的就是居中问题，由于浏览器的兼容性不一致，单一的居中方法会有很多缺陷
即使如此，还是有方法解决这些问题的。这里我总结了最常见的 6 种居中的方法，每种方法都在不同的浏览器下测试过。下面给出具体的代码。

### 水平垂直居中的 6 种常用方法
#### HTML 模板
```html
<!DOCTYPE html>
<html>
<head>
    <title>水平垂直居中</title>
    <link rel="stylesheet" type="text/css" href="center.css">
</head>
<body>
<!-- 绝对定位法 -->
<div class="demo1">
    <div class="absolute-center">demo1</div>
</div>

<!-- 负外边距法 -->
<div class="demo2">
    <div class="negative-center">demo2</div>
</div>

<!-- transform法 -->
<div class="demo3">
    <div class="transform-center">demo3</div>
</div>

<!-- table-cell -->
<div class="demo4">
    <div class="table-cell">
        <div class="block-center">
            demo4
        </div>
    </div>
</div>

<!-- inline-block法 -->
<div class="demo5">
    <div class="inline-center">
        demo5
    </div>
</div>

<!-- flexbox法 -->
<div class="demo6">
    <div class="flexbox-center">
        demo6
    </div>
</div>

</body>
</html>
```

#### CSS 布局
先写一个基本的样式
```css
/*容器*/
.demo1, .demo2, .demo3, .demo4, .demo5, .demo6 {
    margin: 0 auto;
    position: relative;
    height: 500px;
    width: 500px;
    border: 1px black solid;
}
/*样式*/
.absolute-center, .negative-center, .transform-center, .block-center, .inline-center, .flexbox-center {
    height: 200px;
    width: 200px;
    padding: 20px;
    background: #eee;
    line-height: 200px;
    text-align: center;
}
```
1.绝对定位法
```css
/*绝对定位法*/
.absolute-center {
    position: absolute;
    margin: auto;
    overflow: auto;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
}
```
2.负外边距法
```css
/*负外边距*/
.negative-center {
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -110px;
    margin-top: -110px;
}
```
3.tranfrom法
```
/*tranfrom法*/
.transform-center {
    position: absolute;
    margin: auto;
    top: 50%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
}
```
4.table-cess法
```css
/*table-cell*/
.demo4 {
    display: table;
}
.table-cell {
    display: table-cell;
    vertical-align: middle;
}
.block-center {
    margin: 0 auto;
}
```
5.inline-block法
```css
/*inline-block*/
.demo5 {
    text-align: center;
    overflow: auto;
}
.demo5:after, .inline-center {
    display: inline-block;
    verticel: middle;
}
.demo5:after {
    content: '';
    height: 50%;
    margin-left: -0.25em;
}
.inline-center {
    max-width: 99%;
}
```
6.flexbox法
```css
/*flexbox法*/
.demo6 {
    display: -webkit-box;   /* OLD:   */
    display: -moz-box;      /* OLD: Firefox (buggy) */
    display: -ms-flexbox;   /* MID: IE 10 */
    display: -webkit-flex;  /* NEW, Chrome 21–28, Safari 6.1+ */
    display: flex;          /* NEW: IE11,  */

    -webkit-box-align: center; 
        -moz-box-align: center; /* OLD… */
        -ms-flex-align: center; /* You know the drill now… */
    -webkit-align-items: center;
            align-items: center;

    -webkit-box-pack: center; 
        -moz-box-pack: center;
        -ms-flex-pack: center;

    -webkit-justify-content: center;
            justify-content: center;
}
.flexbox {
    display: -webkit-box; display: -moz-box;
    display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;

    -webkit-box-align: center; -moz-box-align: center;
    -ms-flex-align: center;
    -webkit-align-items: center;
            align-items: center;
}
```
