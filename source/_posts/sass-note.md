---
title: 前端工程化之路：初探 Sass 技术
date: 2016-03-23 19:25:23
categories: 前端工具
tags: [css, sass]
---
对于 CSS 来说这门语言就如同是写给设计者们的。语言本身是不需要编译的。而且写起来简单明了，但是对于程序员来说没有一种编程的感觉。对于一些重复冗余的代码，无力提高编写效率。现在推出了两个工具 Sass 和 Less，决定尝试一下，慢慢的适应用编程的方式来写 css。因此写了这篇入门的博客，以便以后忘记了的地方能很快捡起来。

### Sass 语法

#### 变量
```scss
// 普通变量：全局使用属性值
$mainColor: #666666;
$color: #ff0000;
body {background-color: $mainColor; color: $  color;}

// 默认变量
$color: #ff0000;
$color: #000 !default; // 组件化开发时有用

// 特殊变量：名字属性或者其他的
$borderDire = top;
.border-#{$borderDire} {
    border-#{$borderDire}: $color 1px solid;
}

// 多值变量
$pList = 5px 10px 15px 20px;
p {margin: $pList;}
$hSize = (h1: 10px, h2: 15px, h3: 20px);

// 全局变量(将新增)
$color: #333333;
$color: #666666 !global;
```

#### 嵌套
```scss
// 选择器嵌套
body {
    .main {
        color: $color;
    }
}

// 等同于
body .main {color: $color;}

// 属性嵌套
.main {
    border: {
        left: {
            width: 2px;
        }
        top: {
            width: 3px;
        }
    }
}

// 等同于
.main {border-left-width: 2px; border-top-width: 3px;}
```

#### 导入
```scss
// file: _reset.scss
* {margin: 0; padding: 0;}

// file: test.scss
@import 'reset';
body {
    .main {
        color: $color;
    }
}

//等同于
* {margin: 0; padding 0;}
body .main {color: #color;}
```

#### 混合
```scss
// 无参
@mixin center-block {
    margin: 0 auto;
}
.main {
    width: 600px;
    height: 300px;
    @include center-block;
}

// 有参
@mixin box-sizing ($sizing) {
    -webkit-box-sizing: $sizing;
       -moz-bix-sizing: $sizing;
                sizing: $sizing;
}
.box-border {
    border: 1px $color solid;
    @include box-sizing(border-box);
}

// 多参
@mixin border-default($borderWidth: 1px, $borderStyle: dashed) {
    border: $borderWidth $borderStyle #666;
}
div p {@include border-default;}

// media问题
@mixin screen-max($res) {
    @media only screen and (max-width: $res) {
        @content;
    }
}
@include screen-max(600px) {
    body {
        color: red;
    }
}
```

#### 函数
```scss
$baseFontSize: 10px !default;
$greyColor: #cccccc !default;

@function pxToRem($px) {
    @return $px / $baseFontSize * 1rem;
}

p {
    font-size: pxToRem(20px); // 自定义
    color: darken($greyColor, 20%); // 内置函数
}
```

#### 继承
```scss
.main {height: $height; width: $width;}
.left {@extend .main; color: $color;}
.right {@extend .main; color: #777777;}
```

#### 运算
```scss
.main {width: 500px;}
.left {width: 200px;}
.right {width: 500px - 200px - 30px;}
```

#### 条件
```scss
// 双目判断
$size = 10;
$color = red;
p {
    @if $size < 10 {
        font-size: 10px;
    }
}

p div {
    @if $color == yellow {
        color: darken(yellow, 20%);
    } @else if $color == green {
        color: darken(green, 30%);
    } @else {
        color: $color;
    }
}

// 三目判断
if(true, 1px, 2px) // 1px

```

#### 循环
```scss
// 1-10
@for $i from 1 through 10 {
    .item-#{$i}: {
        display: none;
    }
}
// 1-10
@for $i from 1 to 11 {
    .item-#{$i}: {
        display: none;
    }
}
```


### Sass 安装
1.安装 Ruby
2.安装 Sass
```sh
gem install sass
```

### Sass 编译
```sh
# 单文件编译
sass test.scss test.css
# 文件夹监听编译
sass --watch scssDir:cssDir 
# 逆向转换
sass-convert test.css test.scss
```

### Sass 调试
```sh
# 开启调试
sass --watch scssDir:cssDir --debug-info
sass --watch scssDir:cssDir --sourcemap
```
