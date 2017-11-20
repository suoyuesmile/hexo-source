---
title: 《CSS权威指南》读后记<一>：选择器
date: 2015-10-15 18:25:43
categories: CSS
tags: CSS
---
最初接触网页制作是用的微软的 frontPage。只是为了给朋友搭建一个许嵩的主题网站，完成一个小小的课外作业，意外的拿到了不错的分数。虽然并没有用代码取写网页，但是制作的过程也是很有趣的。现在出于某种兴趣对网页制作进行了学习，才发现原来以前玩儿式的制作，现在写代码也可以弄出来，而且更加好看，标准。学习完 HTML 后，开始学习 CSS，现在读了这本 __《 CSS 权威指南》第三版__，感觉收益匪浅，所以记下里面重要的内容以便以后学习之用。

### CSS选择器
首先写一个大致的 html 页面做模板
```html
<!doctype>
<html>
    <meta charset="utf-8">
    <link rel="stylesheet" href="selector.css">
    <head>
        <title>suo的个人网页</title>
    </head>
    <body>
        <h1>锁的生活日志</h1>
        <h2>今天是个好日子</h2>
        <h3>哈哈</h3>
        <h4>呵呵</h4>
        <h5>嘻嘻</h5>
        <h6>吼吼</h6>

        <p>最初接触网页制作是用的微软的 pagefont。只是为了给朋友搭建一个许嵩的主题网站，完成一个小小的课外作业，意外的拿到了不错的分数。</p>

        <p>虽然并没有用代码取写网页，但是制作的过程也是很有趣的。现在出于某种兴趣对网页制作进行了学习，才发现原来以前玩儿式的制作，现在写代码也可以弄出来，而且更加好看，标准。</p>

        <p>学习完 HTML 后，开始学习 CSS，现在读了这本《CSS权威指南》第三版，感觉收益匪浅，所以记下里面重要的内容以便以后学习之用。</p>

        <p class="cselect">类选择器</p>
        <p class="cselect1 cselect2">多类选择器</p>

        <ul>
            <li>1.what</li>
            <li>2.how</li>
            <li>3.why</li>
        </ul>

        <a href="www.baidu.com" class="visited">百度</a>

    </body>
</html>
```
__使用 css 选择器__
```css
h1 {color: red;}
h2 {font: 20px;}
```

#### 元素选择器
```css
html {color: black;}
h1 {color: green;}
h2 {color: red;}
```
__1.选择器分组__
```css
h1, h2, h3, h4, h5, h6 {color: red;}
h1, h3, h5 {backgroud: green;}
```
__2.通配选择器__
```css
* {color: red;}
```
__3.声明分组__
```css
h1, h3, h5 {color: red; background: green;}
h2, h4, h5 {color: red};
```

#### 类选择器与 ID 选择器
```css
<!-- 特定 -->
p.cselect {font-weight: bold;} 
<!-- 通用 -->
.cselect {color: red;}
```
__1.多类选择器__
```css
<!-- 顺序不限 -->
.cselect1.cselect2 {color: grey;}
p.cselect1.cselect2 {color: black;}
```
__2.id 选择器__
```css
p#idselect {color: yellow;}
<!-- 忽略 -->
#idselect {color: yellow;}
```
__3.类选择器与 ID 选择器的选择__
- ID 选择器只能用一次，类选择器可以多次
- ID 选择器不能结合使用，不能有空格来定义不同属性
- 它们都区分大小写

#### 属性选择器
```css
<!-- 现在p中有class的全变红色 -->
p[class] {color: red;}

```
__1.多属性选择__
```css
p[class][id] {color: red;}
```
__2.值属性选择__
```css
p[class="cselect"] {color: red;}
a[href="www.baidu.com"] {color:black;}
```
__3.部分值属性选择__
```css
p[class~="cselect"] {color: red;}
```
__4.子串匹配属性选择器__
```css
<!-- 开头包含串 -->
p[class^="cs"] {color； red;}
<!-- 结尾包含 -->
p[class$="select"] {color:red;}
<!-- 包含即可 -->
p[class*="ele"] {color: red;}
```
__5.特定属性选择器__
```css
*[lang|="en"] {color: red;}
```

#### 后代选择器
```css
ul li {color: red;}
ul li a img {height: 100px;}
.cselect a, #idselect a {color: red;}
```
如上所示，默认的后代选择器，可以忽略层次，选择 ul 下面所有的 li，并不只是直属的
__1.选择子元素__
```css
ul > li {color: red;}
```
选择的是 ul 下直属的 li
__2.选择兄弟元素__
```css
p + .cselect {color:red;}
```
选择的是相同父亲下，紧邻的下一个元素

#### 伪类和伪元素选择器
```css
<!-- a.visited {color: red;} -->
a:visited {color: red;}
```
__1.链接伪类选择器__
```css
a:link {color: blue;}
<!-- 已访问 -->
a:visited {color: red;}
```
__2.动态伪类选择器__
```css
<!-- 输入聚集时 -->
input:focus {background: silver; font-weight: bold;}
<!-- 鼠标停留时 -->
a:hover {color: red;}
<!-- 用户激活时 -->
a:active {color: green;}
```
__3.伪元素选择器__
```css
<!-- 只能用于块元素 -->
<!-- 首字母 -->
p:first-letter {color: red;}
<!-- 首行 -->
p:first-line {color: red;}
<!-- 之前 -->
p:before {color: red;}
<!-- 之后 -->
p:after {color:red;}
```

#### 权重计算
css权重越大优先级越高
```css
<!-- 一等：行内样式：1000 -->
<!-- 二等：id选择器：100 -->
<!-- 三等：类选择器，伪类，属性：10 -->
<!-- 四等：类型选择器，伪元素选择器：1 -->
.cselect #idselect div p[name="suo"] {color: red;}
<!-- 10 + 100 + 1 + 11 = 122 -->
```