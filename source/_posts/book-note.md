---
title: 《JavaScript高级程序设计》读后记<一>：数据类型
date: 2015-11-27 08:25:46
categories: JavaScript
tags: JavaScript
---
经过一段时间的学习，深感 JavaScript 知识点还是挺多。之前一直是看视频教程和博客学习 JavaScript 。但是总觉得缺点什么，不管是视频还是博客，它们强调的始终是单一的知识或者是技术点。并没有对整个语言有一个更加全面的概述。基于这些学习上的不足，我向师哥师姐们询问到了这方面的问题，于是乎他们向我推荐了这本书，《JavaScript高级程序设计》第三版。无奈由于时间原因，一直放在书桌上搁置了好一段时间。就在近几天我终于有机会专心看这本书了。经过一个星期的学习，感觉大有收获，所以写下这篇博客，记录学习中的偏向于重点知识的理解和体会。

### 理解变量与数据类型
__1.ES的变量是松散变量__，也就是说每一个变量仅仅是用于保存一个值的占位符，它可以保存任何值。变量未声明或未初始化之前的值是`undefined`,定义之后的值是赋给它的任何值。到这里就一个小疑问，通常我们学习其他语言都会有，一个值和类型相对应，那么问题来了，`undefind`是值那么它的类型又是啥呢？做个简单的例子测试一下看看
```js
var a; 
alert(a); // undefined
alert(typeof a); // undefined
```
原来值为`undefined`的类型也是`undefined`。
由此我们可能会想到另外一个值，那就是`null`,是不是`nUll`的类型也是`null`呢？
```js
var a = null; // null
alert(a); // object
alert(typeof a);
```
`null`的类型竟然是`object`，估计有不少人会大跌眼镜吧，为啥会这样？凭什么？
其实这是正确的，`JavaScript`的`null`被认为是空对象的引用
这里还要解释下什么是空对象的引用？简单的说就是一个变量本该保存一个对象，但是还没有保存对象，这个时候就应该把这个变量来保存`null`，显示的表示这个变量是一个空对象的变量。

__2.`undefined`和`null`又有哪些区别和联系呢？__
首先说联系`undefined`其实是`null`派生出来的，在代码层上表现为
```js
    alert(undefined == null); // true
    alert(undefined === null); // false
```
第`2`个为`false`好说，至少是他们类型是不一致的。第一个为什么为`true`呢？
原来这个`==`符是类似相等符，不是绝对相等，`null`和u`ndefined`的值是类似的。
区别上面也很容易看出来
- 用途不一致：`null`用于空对象指针，给还没对象的变量赋值。`undefined`是来区别变量的未初始化或声明的。
- 常用于：`null`常用与显示的赋值变量，`undefined`不会这么做.

__3.基本类型__
ES 中有 5 种简单数据类型和一种复杂数据类型，分别为`undefined`，`null`，`boolean`，`number`，`string`，`object`。其中`object`实质是一组无序名值对组成的。
具体理解的是`object`这一类型。通过执行`new`来创建对象
```js
var o = new Object();
var o = new Object; // 都可以
```
`object`的每一个实例下面都有属性和方法
- `constructor`:创建当前对象的函数，对于上面就是`Object()`这个构造函数
- `hasOwnProperty(propertyName)`:检查属性在当前对象中是否存在
- `isProtptypeOf(object)`:检查属性是否是当前对象原型
- `propertIsEnumerable(propertypeName)`:检查对象是否可以用`for-in`来枚举
- `toLocaleString()`:返回对象字符串表示，与执行环境对应
- `toString()`:返回对象字符串表示
- `valueOf()`:返回对象的字符串，数值，布尔值表示
要具体理解这些函数和属性，需要后面面向对象的思想做铺垫。
ES 中 Object 是所有对象的基础。因此所有对象都具有这些基本的属性和方法。






