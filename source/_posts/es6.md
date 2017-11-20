---
title: ECMAScript 6 之 let 和 const 命令
date: 2016-09-10 19:25:23
categories: JavaScript
tags: [JavaScript, ECMAScript6]
---
学习前端有一段时间了，ECMAScript5 也算是掌握的差不多了。经过了这段时间的实践，也从中发现ECMAScript5 有很多不方便的地方。比如只有函数作用域，块级作用域需要模拟，还有面向对象也是不足够的，等等。后来我接触到了 ECMAScript6 果然在这个新版本中，很多 ES5 的遗留问题都得到改善。所以我近期找了一些 ES6 的资料书来学习。其中入门最佳的书籍，要推阮一峰老师的 《ECMAScript 6入门》 了。感谢一峰老师的知识的整理，提升了我学习 ES6 的效率。

### let命令
类似于`var`，__只在块中有效。变相的新增了块级作用域有没有__
```js
{
    let name = "suo";
    var gender = "male";
    console.log(name); // suo
}
console.log(name); //
console.log(gender); // yue
```

__1.适合使用for循环__
```js
for(let i = 0; i < 3; i++) {
    let i = "suo";
    console.log(i);
}
// suo suo suo

// 2.
```
知道注意的地方是，`let`用在`for`循环中是输出了三次`suo`，说明`for`中的两个`let`的作用域也不一样

__2.不存在变量声明提升__
```js
console.log(name);
let name = "suo";
// Uncaught ReferenceError: name is not defined
```
变量使用一定要在声明后使用，否则报错

__3.暂时性死区__
```js
var name = "suo";
{
    name ="yue";
    let name = "suo yue";
}
// Uncaught ReferenceError: name is not defined
```
一旦块中有`let`声明的变量，该变量即与块绑定，不受外界变量干扰

__4.不允许重复声明__
```js
{
    let name = "suo";
    let name = "yue";
}
// Uncaught SyntaxError: Identifier 'name' has already been declared
```

### 块级作用域
let 实际是给 JavaScript 新增了块级作用域
__1.作用域嵌套__
```js
{
    let name = "suo"
    {
        let name ="yue";
        console.log(name); // yue
    }
    console.log(name); // yue
}
```
作用域之间任意嵌套互不影响

__2.块级作用域域函数声明__
```js
function foo() {
    console.log("suo");
}
{
    function foo() {
        console.log("yue");
    }
    foo(); // suo
}
foo(); // suo
```
函数声明互不影响，不同环境运行不一样。避免使用函数声明，用函数表达式替代

__3.do表达式(提案)__
```js
let x = do {
    let a = 1;
    a = a + 1;
}
console.log(x); // Uncaught SyntaxError: Unexpected token do
```

### const 命令
与 c++ 类似，const 声明一个只读常量，一旦声明不可改变，同时也意味着声明必须初始化
```js
const PI = 3.1415926;
PI = 3; //Assignment to constant variable

const LENGTH; 
// Uncaught SyntaxError: Missing initializer in const declaration
```

__1.只在声明所在的块级作用域内有效__
```js
{
    const PI = 3.14;
}
console.log(PI); // VM143:4 Uncaught ReferenceError: PI is not defined
```

__2.不可重复声明__
```js
const PI = 3.14;
const PI = 3;
// Uncaught SyntaxError: Identifier 'PI' has already been declared
```

__3.const的本质__
`const`保证的是变量指向的内存地址不改变。对于引用类型来说，变量名本身就是一个指向实际的内存指针。所以总结来说`const`是让变量名与它指向的内存的地址，这一关系不变。相当于 C 语言中的指针常量而不是常量指针。
```js
const person = {
    name: "suo",
    gender: "male"
};
person.name = "yue";
console.log(person.name); // yue

person1 = {
    name: "yue",
    gender: "female"
};
person = person1; 
// Uncaught TypeError: Assignment to constant variable
```
