---
title: 框架初探：jQuery 初体验
date: 2017-03-13 22:34:22
categories: JavaScript
tags: [jQuery, JavaScript]
---
学过一段时间，也用过一段时间 JavaScript，总感觉原生的 JS 需要处理太多的兼容问题。有没有一个好的工具，把这些麻烦事件都给封装起来。当然是有的，自从用了 jQuery， 代码敲得少了，处理的事情却更多。现在写一个入门的博客，以便以后翻阅。

### jQuery 是啥
jQuery 本身是很小型框架的，这个有点像我们学习 C++ 中的 STL (标准模板库)，简单来说就是大神们帮我们把一些复杂的实现逻辑封装成一个函数或者类直接供我们来用。其中也考虑了健壮性和兼容性以及性能问题，真的是很好很方便的工具。就让我站在巨人们的身上敲代码吧。

### jQuery 原理
jQuery3.0.0 现在采用的是 ES5 语法并没有采用 ES6 语法的,而且全部采用的是严格模式，我仔细看了下未压缩源码总共有 10038 行，规模还算不小。
但是整体的结构的设计变化不大。

jQuery 的核心是一个 __立即执行的匿名函数__，接收两个参数，一个是 `global` 对象，一个是 `factory` 的函数对象。
下面是它的核心的源码结构，核心函数是507行完，大部分代码其实是在扩展里面
```js
( function( global, factory ) {
    "using strict";
    if ( typeof module === "object" && typeof module.exports === "object" ) {
        // 定义模块对象
        module.exports = global.document ?
            factory( global, true ) :
            function( w ) {
                if ( !w.document ) {
                    throw new Error( "jQuery requires a window with a document" );
                }
                return factory( w );
            };
    } else {
        factory( global );
    }
} )( typeof window !== "undefined" ? window : this, function( window, noGlobal ) {
    "using strict";
    // 代码
} );
```
这段代码的主要作用是判断 `global.document` 是否存在，不存在就抛出错误，存在就定义的模块为传进来的 `factory` 对象。
所以其实真正的操作全在 `factory` 的函数里。
比较在意的是 jQuery 的代码风格和我们大多数人平时的风格不一样，松散性太高，有些不习惯

下面我们看看 factory 函数里到底写了些什么呢
```js
function ( window, noGlobal ) {
    "using strict";

    var arr = [];
    var document = window.document;
    var getProto = Object.getPrototypeOf;
    var slice = arr.slice;
    // code
    // 这里是把一些数组的方法作了简化
    var
        varsion = "3.0.0",
        jQuery = function( selector, context ) {
            return jQuery.fn.init( selector, context );
        },
        // code
        fcamelCase = function( all, letter ) {
            return letter.toUpperCase();
        };
    jQuery.fn = jQuery.prototype = {
        // code
    };
    jQuery.extend = jQuery.fn.extend = function() {
        // code
    };
    // code
}
```
这 500 多行整体的结构大致就是这样的，学了 ES5 理解这些代码也不难，它首先处理了基本类型数组的一些方法，然后定义了 jQuery 选择器的方法初始化过程，然后定义 jQuery 的核心方法和原型的调用过程，最后增加了 jQuery 的扩展。具体 10000 多行代码也不细说了。下面看看怎么用吧。

### 开始用 jQuery
开始 jQuery 真的很简单
#### 引用
两种方式引用
1.下载后引用本地库
我们到官网下载jQuery的不同版本，然后把文件放到我的项目目录中，供调用
```html
<!DOCTYPE html>
<html>
<head>
    <title>jQuery初探测试</title>
    <script src="lib/jquery-3.0.0.js"></script>
    <script src="test.js"></script>
</head>
<body>
</body>
</html>
```
我下载的是最新的3.0.0版本，在html引用，后写一个js测试一下
```js
// test.js
$(document).ready(function() {
    console.log("hello world");
}); // hello world
```

2.在线直接引用
```html
<!DOCTYPE html>
<html>
<head>
    <title>jQuery初探测试</title>
    <script src="https://code.jquery.com/jquery-3.0.0.min.js"></script>
    <script src="test.js"></script>
</head>
<body>
</body>
</html>
```
测试结果相同
说明：中间有个 min 是压缩版，就是生产版本，没有的就是开发版本。不同场景使用不同版本，开发版本可以直接修改内容，生产版本已经压缩好了，一般就是直接用于产品了。

#### 基本用法
选择一个元素或者元素集合，对选中的元素进行操作，比如
```html
<!DOCTYPE html>
<html>
<head>
    <title>jQuery初探测试</title>
    <script src="lib/jquery-3.0.0.js"></script>
    <script src="test.js"></script>
</head>
<body>
    <h1>click me</h1>
    <h2>can't see me</h2>
</body>
</html>
```

```js
// 正常写法
$(document).ready(function() {
    $("h1").click(function() {
        $(this).addClass("color");
    });
    $("h2").mouseover(function() {
        $(this).hide();
    });
});

// 简写
$(function() {
    $("h1").click(function() {
        $(this).addClass("color");
    });
    $("h2").mouseover(function() {
        $(this).hide();
    });
});
```
也可在线测试 [__jquery在线测试__](http://runjs.cn/code/agukaw1z)

### 基本用法
具体全面的使用，参照[__API__](http://jquery.cuishifeng.cn/);
我现在一个方面实现几个简单的 demo

#### 模板
先写一个模板
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="dome">
    <script src="lib/jquery.3.0.0.js"></script>
    <title></title>
</head>
<body>
    <h1>demo1</h1>
</body>
</html>
```

```js
// 给body添加css
$(function() {
    $(document.body).css("color", "red");
});
```

#### 选择器
`$(selector, [context])`,第一个参数为选择器，第二个对象是可选项，选择器的当前环境
```js
// 实例1
$(function() {
    // 给div p 增加 css
    $("div p").addCss("color", "red");

    // 鼠标移动上去就隐藏
    $("div.myclass").mouseover(function() {
        $(this).hide();
    });
   
    // 找到页面中的第一个表单，选择里面的input,点击改变值
    $("input", document.forms[0]).click(function() {
        $(this).attr("value", "出来了");
    });
});
```

#### 动态创建
`$(html, [ownerdocument])`第一个参数为 html 片段，第二个是可选项，为 html 所属文档文档
```js
    // 创建一个hello的div插入到.hide中
    $("<div><p>hello</p></div>").appendTo(".hide");
    
    // 创建div元素并声明对象的属性
    $("<div>", {
        "class": "test",
        text: "click me",
        click: function() {
            $(this).toggleClass('test');
        }
    }).appendTo("body");

    // 创建一个imput
    $("<input>", {
        type: "text",
        val: "Test",
        focusin: function() {
            $(this).addClass("active");
        },
        focusout: function() {
            $(this).removeClass("active");
        }
    }).appendTo("form");
```

#### AJAX
简单的使用，jQuery.ajax(url, [setting])
```js
    // 发送一个ajax请求
  // Ajax
    var data = $.ajax({
        type: "get",
        url: "demo1.php",
        data: "name=suo&gender=male",
        asyns: true,
        cache: false,
        success: function() {
            console.log("good");
        }
    }).reponseText;
    console.log(data);
```
使用AJAX进行跨域请求
```js
 $(function() {
    $("input[name='username']").keyup(function() {
        var username = $(this).val();
        $.ajax({
            url: "http://101.132.34.184/demo/demo1.php",
            dataType: "json",
            type: "post",
            data: { "username": username },
            crossDomain: true,
            success: function(data) {
                console.log(data);
            }
        });
    });
});
```

```php
//demo/demo1.php
<?php
    // 允许跨域
    header("Access-Control-Allow-Origin:*");
    $userDB = ["shaosuo", "suoyue", "yue"];
    if(isset($_POST["username"])) {
        if(in_array($_POST["username"], $userDB)) {
            echo 0;
        } else {
            echo 1;
        }
    }
php?>
```

_还有一些用法，这里不一一测试了，做到这些也算是入门了，多查文档多熟悉语法，jQuery 用熟练了才算真正的好用_




