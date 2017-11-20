---
title: 学习新技术：从 HTML4 到 HTML5 的改变
date: 2016-03-13 22:34:22
categories: HTML
tags: [HTML, HTML5]
---
很长时间都在和 HTML5 打交道，但是并没有区分 HTML5 和 HTML4 。但是今天我偶然看了一本 H5 的书籍 《H5网页设计入门必读》 ，读完之后，才想到原来这么多特性是 HTML5 的。由此我在这里做一个分类总结。总结一下 HTML5 到底有什么新的地方。

很久之前，我读了一本CSS的书籍《CSS权威指南》，里面详细概述了关于 __结构和表现分离__的思想。为什么谈到这个呢？原因是现在我们说的 H5 的改变很多都是基于这种思想。为了将 HTML 从表现化中脱离出来。

### HTML5新增特性
H5总得来说从6个方面做出了改变：
1.弃用过时元素标签
2.新增结构元素标签
3.新增富媒体元素标签
4.增强表单属性
5.新增JavaScript API
6.语义化，容错性提高

#### 弃用过时标签
```html
<!-- 框架家族 -->
<frame></frame>
<frameset></frameset>
<noframes></noframes>
<!-- 弃用 -->

<!-- 首字母缩写 -->
<acronym title="Suo Yue">SY</acronym> 
<!-- 弃用 -->
<abbr ttile="Suo Yue">SY</abbr> 
<!-- 替代 -->

<!-- 显示元素 -->
<!-- 弃用 -->
<font></font>
<big></big>
<center></center>
<strike></strike>
<!-- css替代-->

```

#### 新增结构元素
```html
<!-- section:集合理论上相关的内容 -->
<section>
    <h1>锁的博客</h1>
    <p>这是邵锁的博客</p>
</section> 

<!-- header:介绍和导航的辅助工具 -->
<section>
    <header>
        <h1>邵锁</h1>
    </header>
    <p>邵锁的博客</p>
</section>

<!-- footer:内部元素信息，作者版权相关内容 -->
<section>
    <header>
        <h1>邵锁</h1>
    </header>
    <p>邵锁的博客</p>
    <footer>
        <p>作者：邵锁</p>
    </footer>
</section>

<!-- aside: 侧边栏，相关性不大的内容 -->
<aside>
    <p>总计文章12篇</p>
</aside>

<!-- nav:全站导航信息 -->
<section>
    <header>
        <nav> 
        </nav>
    </header>
</section>

<!-- article:与aection相似，是section一个特殊类别，用于自包含内容 -->
<article>
    <header>
        <nav> 
        </nav>
    </header>
</article>

<!-- hgroup:不希望内容现在在文件大纲内 -->
<hgroup>
    <h1>suo yue</h1>
    <h2>suo suo</h2>
</hgroup>
```

#### 新增富媒体元素标签
```html
<!-- canvas:创建动态图像的环境 -->
<canvas id="suo" width="200" height="200"> 
var canvas = dociment.getElementById('suo');
var context = canvas.getContext('2d');
context.strokeStyle = '#666666';
context.strokeRect(20, 20, 100, 100);
<!-- 绘制一个正方形 -->
</canvas>

<!-- audio:音频 -->
<audio src="canon.mp3" autoplay></audio>
<audio src="canon.mp3" controls></audio>
<audio src="canon.mp3" autobuffer></audio>
<!-- 兼容音频格式 -->
<audio controls>
    <source src="canon.ogg" type="audio/ogg">
    <source src="canon.mp3" type="audio/mp3">
</audio>

<!-- video:视频 -->
<video src="movie.mp4" width="200" height="200"></video>
<!-- 兼容视频格式 -->
<video width="200" height="200">
    <source src="movie.ogv" type="video/ogv">
    <source src="movie.mp4" type="video/mp4">
</video>
```

#### 增强表单属性
```html
<!-- 以前的常规表单 -->
<form action="login.php" method="post">
    <input type="text" name="username">
    <input type="password" name="password">
</form>

<!-- placeholder:没有值则插入占位符，定位后删除，离开恢复 -->
<input type="text" name="username" placeholder="username">

<!-- autofocus:自动聚焦模式,文件加载后自动聚焦到某一表单栏 -->
<input type="text" name="username" autofocus>

<!-- required:将脚本变为标记 -->
<input type="text" name="username" required>

<!-- autocomplete:自动填写表单 -->
<input type="text" name="username" autocomplete="on">

<!-- datalist:一系列选项关联到输入栏 -->
<input type="text" name="username" list="suoyue">
<datalist id="suoyue">
    <option value="suoyue"></option>
    <option value="suo"></option>
    <option value="shaosuo"></option>
    <option value="yue"></option>
    <option value="shaosuoyue"></option>
</datalist>

<!-- type:新增类型 -->
<input type="search"> 
<input type="email"> 
<input type="url"> 
<input type="tel"> 
<input type="range" min="1" max="10"> 
<input type="number" min="1" max="10"> 
<input type="date"> 
<input type="datetime"> 
<input type="datetime-local"> 
<input type="time"> 
<input type="month"> 
<input type="color"> 

<!-- 自定类型:正则表达式 -->
<input pattern="[\d]5"> 

```

