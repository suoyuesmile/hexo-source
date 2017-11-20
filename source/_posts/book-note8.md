---
title: 《JavaScript高级程序设计》读后记<八>：BOM
date: 2015-12-08 22:31:11
categories: JavaScript
tags: JavaScript
---
前几章学了 JavaScript 的基础的知识，理论性太强。需要思考理解的东西远远多于实战的。我最近看了 BOM 这一章，才真正的感受到了 JavaScript 真的很强大，特别是在于操作浏览器这方法。不多写了，已经等不急实战了。

### 理解BOM

#### 使用 Window 对象
说明一下，在浏览器中 window 对象有两重身份，一重是 JavaScript 访问浏览器的接口，另一重是 ES 规定的 Global 对象，因此可以访问`parseInt()`等方法
测试一下
```js
var name = "suo";
function sayName() {
    console.log(this.name);
}
sayName(); // suo
console.log(window.name); // suo
window.sayName(); // suo
```
说明全局的变量和方法，使用 window均能访问到

全局变量的window的变量有说明差异呢，有一点就是定义在window上的属性可以使用delete删除，而定义的全局变量不可以
```js
var name = "suo";
window.gender = "male";

delete name;
delete window.gender;

console.log(name); // suo
console.log(window.gender); // undefined
```

另外尝试访问未声明的变量会抛出错误，但是通过查询window对象，可以知道某个可能未声明的对象是否存在
```js
var newname = oldname; // 报错
var newname = window.oldname; // 没报错
```

说了这么多，我们来用 window 对象操作窗口
(1)控制窗口位置
```js
var leftPos = (typeof window.screenLeft == "number") ?
    window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ?
    window.screenTop : window.screenY;

console.log(leftPos);
console.log(topPos);

// IE(5,7-11) edge firefox 都能正常显示位置
// chorme 始终显示 0 0
// 令人费解
```
除此之外还可以改变位置
```js
// 移动多少像素
window.moveBy(20, 30);
// 移动到哪个位置
window.moveTo(100, 100);

// 测试结果，除了IE以外，其他浏览器都默认禁用了
```

(2)控制窗口大小
```js
// 页面视图区大小
// 兼容的处理
var pageWidth = window.innerWidth,
    pageHeight = window.innerHeight;
if (typeof pageWidth != "number") {
    if (document.campatMode == "CSS1Compat") {
        pageWidth = document.documentElement.clientWidth;
        pageHeight = document.documentElement.clientHeight;
    } else {
        pageWidth = document.body.clientWidth; // IE6
        pageHeight = document.body.clientHeight;
    }
}
console.log(pageWidth + ',' + pageHeight); // 1080,1008
```
我们还可以调整窗口大小
```js
// 调整的像素
window.resizeBy(300, 200);
// 调整到多少
window.resizeTo(100,100);
// 仍然除了IE以外其他都禁用了的
```

(3)打开窗口
使用`window.open()`函数打开窗口，其中有四个参数：URL， target， 特性字符串，是否取得历史纪录中那个页面。第一个参数不用说，第二个参数有几个可以是特殊值：`_self`,`_parent`,`_top`,`_blank`。举个例子
```js
// 打开窗口
var local = window.open("http://localhost:4000", "local", "height=500,width=500,top=100,left=100,resizable=yes");
// 关闭窗口
local.close()
// 强制关闭
top.close();
```
检测窗口是否被屏蔽
```js
var blocked = false;
try{
    var local = window.open("http://localhost:4000", "_blank");
    if (local == null) {
        blocked = true;
    }
} catch (ex) {
    blocked = true;
}
if (blocked) {
    console.log("窗口被屏蔽");
}

```

(4)使用定时器
window 对象提供两种定时器，一种是超时定时器`setTimeout()`,另一种是间歇定时器`setInterval()`。它们都提供两个参数，一个是执行的代码，一个是时间毫秒。看一下代码
```js
// 超时定时器
var timer = setTimeout(function() {
    console.log("ok");
    }, 1000); // 经过一秒后ok

// 取消超时定时器
clearTimeout(timer); // 没打印

// 间歇定时器
var max = 3; // 设置最多定时次数
var num = 0;
var interval = setInterval(function() {
    num++;
    if (num == max) {
        clearInterval(interval);
    }
    console.log("ok");
    }, 1000); // 没经过一秒打印一个ok，打印三次后结束打印

// 使用超时定时器模拟间歇定时器
var max = 3;
var num = 0;
function timerFunction(){
    console.log("ok");
    num++;
    if (num < max) {  
        setTimeout(timerFunction, 1000);
    } else {
        console.log("done");
    }
}
setTimeout(timerFunction, 1000);

```
开发环境中很少真正使用间歇定时器，一般用超时定时器模拟它，原因在于当执行函数时间大于间歇时间时，后一个间歇定时器在前一个调用结束之前调用。

(5)使用系统对话框
由于系统对话框带来的用户体验相当差，现在用的很少，这里随便提一下
```js
// 验证框与弹出框
if (confirm("are you sure?")) {
    alert("baici");
} else {
    alert("shagua");
}
// 文本框
var name = prompt("you name?");
alert("welcome " + name);
// 打开find,print
find();
print(); // 打印对话框
```

#### 使用 location 对象
location对象是BOM最有用的对象之一，提供了文档有关的信息，还提供了一些导航功能。它是一个特别的对象，即是window属性，也是document属性，也就是说location,window.location,document.location是同一个东西。我们来试试它的功能。
```js
console.log(location.hash); // 无
console.log(location.host); // localhost:4000
console.log(location.href); // http://localhost:4000/
console.log(location.port); // 4000
console.log(location.protocol); // http:
console.log(location.search);
```

#### 使用 navigator对象


#### 使用 screen 对象
用来识别客户端浏览器的，用处不大，表明客户端能力的。一般用于客户端能力检测

#### 使用 history对象
保存用户上网的历史记录。处于安全考虑，开发人员无法知道历史记录的具体的URL,但是可以通过go()方法在历史记录中任意跳转。这个方法只接受一个参数，正数前进，负数后退
```js
// go方法前进后退
history.go(-1);
history.go(1);

// 简写的两个方法
history.back();
history.forword();
```