---
title: 《CSS权威指南》读后记<二>：盒子模型
date: 2015-10-20 19:66:44
categories: CSS
tags: CSS
---
之前一直在使用 height 和 width，但是经常会遇到问题。在看了 __盒子模型__这一章后，很多东西迎刃而解了。现在详细的记录这些知识点。

### 盒子模型
由四个部分组成，如下图
[img](/images/ds7.png)

#### 内容
高度与宽度决定内容大小
```css
p {height: 100px; width: 100px;}
```

#### 外边距
边框外部
```css
h1 {margin: 0.25in; background-color: red;}
<!-- 可以接受px,em,cm单位 -->

<!-- 单独设置 -->
h1 {margin: 10px 20px 15px 10px;}
<!-- top right bottom left 可以混合单位-->

<!-- 缺值规则 -->
<!-- 缺左外，使用右外，缺下使用上，缺右使用上 -->
h1 {margin: 10px 0;}
<!-- 10px 0 10px 0 -->

<!-- 单边外设置 -->
h1 {margin-lelf: 10px;}

<!-- 负外边距 -->
h1 {margin-left: -10px;}
```

#### 边框
外边距与内边距之间
```css
<!-- 默认为none -->
<!-- border-style的值很多 -->
<!-- none,hidden,dotted,dashed,solid,double,groove,ridge,... -->

<!-- 可以定义多样式 -->
p.new {border-style: solid;}
p.new {border-top-style: solid;}
p.new {border-style: solid dashed none;}

<!-- 边框宽度 -->
p.new {border-width: 1px;}
p.new {border-top-width: 2px;}
p.new {border-width: 2px 1px 3px 2px;}

<!-- 边框颜色 -->
p.new {border-color: #666;}
p.new {border-top-color: #666;}
p.new {border-color: #666 #344 #333 #455;}

<!-- 简写属性 -->
p.new {border: 3px solid #666}
<!-- 顺序不重要 -->
```

#### 内边距
边框与内容之间
```css
<!-- 同理margin -->
p.new {padding: 10px;}
p.new {padding-top: 20px;}
p.new {padding: 10px 20px 30px 40px;}
```
