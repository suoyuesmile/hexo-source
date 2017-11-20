---
title: 《JavaScript高级程序设计》读后记<三>：闭包
date: 2015-11-28 11:28:15
categories: JavaScript
tags: JavaScript
---
在上一篇博客里，谈到了作用域，留下了一个问题，怎样从函数外访问到函数内部的局部变量。这是在我完全理解闭包和函数后，写的一篇博客。

### 理解函数与闭包
前面我们知道了作用域最小单位就是函数，那么函数是怎么表示的呢？
定义函数有两种方式：一种是函数声明，另一种是函数表达式。
```js
// 函数声明
function foo(arg0, arg1, arg2) {
    // code...
}
```
我们知道函数也是一种引用类型，所以函数，也有自己的属性，其中一个就是`name`
```js
console.log(foo.name);  // foo
```
由此我们也更加确定了，函数也是一个对象，里面包含各种属性和方法
对于函数还有一个特别的性质，那就是函数声明提升，如同变量声明提升，函数声明也是可以提升的
```js
sayBye();  // bye bye
function sayBye() {
    alert("bye bye");
}  
```
函数声明提升会带来一个问题
```js
// 反例
var a = 1;
if (a > 1) {
    function saybye() {
        // code...
    }
}
else (a < 1) {
    function saybye() {
        // code...
    }
}
```
上面的代码，很容易看出来，函数声明提升后，可能产生和预期不一样的效果
接下来我们看下函数表达式
```js
var foo = function(arg0, arg1, arg2) {
    //code..
};
```
__这个表达式怎么解释呢？__
就如一个赋值语句一样，将后面的匿名函数赋值给`foo`变量，有人可能有疑问，函数也可以赋值吗？在 js 里面来说，这是可以的，因为 js 里面函数也是一种引用类型，当作对象赋值给变量

__那么函数表达式是否也有函数提升呢？__
下面验证一下
```js
foo(); // VM225:1 Uncaught TypeError: foo is not a function
var foo = function() {
    alert("ok");
};
```
答案是没有，函数并没有提升上去，因为在这里函数作为了对象赋值给了变量，并不是声明，这里的赋值同样的也是引用赋值。此时`foo`指向的是堆中的内存块。

__谈谈闭包__
闭包是有权访问另一个函数作用域中的变量的函数，创建闭包的常见方式，就是在一个函数内部创建另一个函数
```js
// 要访问foo中的a
function foo() {
    var a1 = 1;
}
foo();
console.log(a1);
// VM309:4 Uncaught ReferenceError: a1 is not defined
```
显然访问不了
```js
// 使用闭包函数访问
function foo() {
    var a2 = 2;
    var b = function() {
        console.log(a2);
    }
    return b();
}
foo(); // 2
```
__下面解释一下是怎么做到的__
`a2`是`foo`的局部变量，而是`b`函数的外部变量，而`b`是可以访问到的，`b`函数在闭包函数`b`返回到外部的函数或全局变量
```js
// 改写成立即执行函数
function foo() {
    var a3 = 3;
    return (function() {
        console.log(a3);
        })();
}
foo(); // 3
```
将`b`函数改写成立即执行函数更加清晰紧凑了，相当于函数里面将一个执行的函数返回到外部去
就好像在函数内外构建了一个桥梁。
__闭包有闭包的好处，但是它也有不少缺陷__
闭包会携带包含它的函数的作用域，因此比其他函数占用更多的内存，过度的使用闭包可能会导致内存占用过多
__闭包的限制与解除__，闭包只能取得包含函数中的任何变量的最后一个值，闭包保存的是整个变量对象，而不是某个特殊的变量
下面我们来看看代码
```js
function foo() {
    var arr = new Array;
    for (var i=0; i<10; i++) {
        arr[i] = function() {
            return i;
        }
    }
    return arr;
}
alert(foo());
```
谈到闭包的缺点，必须要说的一个东西就是 Js 的垃圾回收机制，下篇博客，我将重点研究关于 JS 的垃圾回收机制，

