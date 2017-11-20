---
title: 《JavaScript高级程序设计》读后记<七>：继承
date: 2015-12-06 13:33:12
categories: JavaScript
tags: [JavaScript, OOP]
---
上一篇博客，我深入理解了对象，可以通过一些方式来创建对象。而对于 OOP 来说，仅仅做到这些是不够的。我们学过 OOP 都知道，它有三大特性，继承，多态，封装。第一个就是继承，而 JS 却本身没有继承这一方法。所以我们需要通过 JS 其他的特性来实现继承。由于 JS 的函数是没有函数签名的。所以并不能做到“接口继承”，但是我们可以做到“实现继承”。

#### 理解继承

__(1)原型链__
__原型链__是实现继承的主要方法，那么原型链到底是什么样的东西呢？上一篇博客我们重点分析了原型这一属性和对象。并且给出了一个图来说明它们之间的关系。回顾一些，下面给出之前的图
[img](/images/dm5.png)
现在，我稍微改变一下原型属性的指向，我们让原型属性不指向它自己的原型对象，而是指向另一个函数的原型对象。如同所示，依次如此就构成了 __原型链__
[img](/images/dm6.png)
__各个函数之间通过原型对象构成一条链，所以称之为原型链__，下面模拟一下这个过程
```js
// 父类型
function Person() {
    this.prototype = true;
}
Person.prototype.getPersonValue = function() {
    return this.prototype;
};
// 子类型
function SubPerson() {
    this.subprototype = false;
}
// 继承，子类型原型等于父类型实例
SubPerson.prototype = new Person();
SubPerson.prototype.getSubValue = function() {
    return this.subprototype;
};

var subperson = new SubPerson();
// 测试
console.log(subperson.getPersonValue()); // true
console.log(subperson.getSubValue()); // false
```
由上可知，当我们的函数的原型属性等于另一个函数的实例时，我们就继承了它的原型属性和方法。同时我们仍然保留了自己原型属性和方法。

我们现在知道了使用原型链来实现继承，有了继承我们的 __原型搜索机制__也得到了扩展
- 搜索实例本身定义的属性
- 搜索实例原型
- 搜索继承的原型(多重继续)
- 搜索Object的原型(继承链顶端)

__要注意的方面：__
- __子类型给原型添加的方法要在父类型之后添加__(无论是新添加还是重新)
- __通过原型链实现继承，不能通过对象字面量创建原型方法，这样会重写原型链__

__原型链的缺陷：__
缺陷一，原型链实现的继承，原型变成了另外一个类型的实例，__原有的实例属性变成了原型属性__
```js
function Person() {
    this.name = ["suo", "yue"];
}
function SubPerson() {
}
// 继承
SubPerson.prototype = new Person();
// 实例
var sub1 = new SubPerson();
sub1.name.push("smile");
console.log(sub1.name); // (3) ["suo", "yue", "smile"]

var sub2 = new SubPerson();
console.log(sub2.name); // (3) ["suo", "yue", "smile"]
```
缺陷二，__子类继承了父类，但是没法给父类传递参数。__
基于这些缺陷，单一的使用原型链继承不太实用，怎么去解决这些问题呢？

__(2)构造函数__
上面两个缺陷是否有解决方法，结合我们之前学习的知识。我们思考一下，
__可以传参，而且不使用原型等于实例这种方法，怎么让子类型使用父类型的变量和方法呢呢？__
由此我们想到了之前学习的两个函数的方法，一个是`apply()`，一个是`call()`。__它们可以将函数调用的其他函数绑定本函数的作用域和参数__。现在我们来试试
```js
// 父类型
function Person(name) {
    this.name = ["suo", "yue"];
    this.addName = function() {
        this.name.push(name);
    }
}
// 子类型
function SubPerson(name, gender) {

    // 调用父类函数，并绑定子类函数和参数
    Person.call(this, name);
    this.gender = gender;
}

var subperson1 = new SubPerson("smile", "male");
var subperson2 = new SubPerson("cry", "female");
subperson1.addName();

// test
console.log(subperson1.name); // ["suo", "yue", "smile"]
console.log(subperson2.name); // ["suo", "yue"]
```
同理单独使用构造函数实现继承也是不行的，共享属性和变量就谈不上了，所以我们还是结合它们两者的优势重新实现继承吧。这和创建对象的模式有着异曲同工之妙。

__(3)组合继承__
结合以上我们来整合它们的技术，先上代码
```js
// 父类型
function Person(name) {
    this.name = name;
    this.nameGroup = ["suo", "yue"];
}
// 父类型函数原型方法
Person.prototype.getName = function() {
    return this.name;
};
Person.prototype.addName = function() {
    this.nameGroup.push(this.name);
}
// 子类型
function SubPerson(name, gender) {
    // 构造继承，调用父类型函数并绑定作用域和参数
    Person.call(this, name);
    this.gender = gender;
}
// 原型链继承，子类型函数原型等于父类型实例
SubPerson.prototype = new Person();
// 修正构造属性
SubPerson.prototype.constructor = SubPerson;
// 子类型函数原型方法
SubPerson.prototype.getGender = function() {
    return this.gender;
}
var subperson1 = new SubPerson("smile", "male");
var subperson2 = new SubPerson("cry", "female");

// test
subperson1.addName();
console.log(subperson1.getName());   // smile
console.log(subperson1.getGender()); // male
console.log(subperson1.nameGroup);   // ["suo", "yue", ""]
console.log(subperson2.getName());   // cry
console.log(subperson2.getGender()); // female
console.log(subperson2.nameGroup);   // ["suo", "yue"]

```

__(4)原型式继承__
基于已有的对象创建新对象，同时还不必因此自定义类型，看看代码
```js
function object(o) {
    function F() {}
    Person.prototype = o;
    return new F();
}
```
现在我们可能看不懂，但是结合原型链的思想，改变一下，或许就很明了了
```js
// person1是Person的实例
function object(person1) {
    function SubPerson() {}
    // 子类型的原型等于父类型的实例
    SubPerson.prototype = person1;
    return new SubPerson;
}

// test
subperson1 = object(person1);
```
咋一看这不就是将原型链的继承方法，用函数封装了一下吗。有啥区别
确实原理都是样的。区别就在于，__原型链方法是类型到类型。而原型式继承则直接是对象到对象__。测试一下，到底可不可以继承到属性和方法
```js
var person1 = {
    name: "suo",
    nameGroup: ["suo", "yue"];
    getName: function() {
        return this.name;
    }
};

// 继承方法
function createObj(o) {
    function P() {};
    P.prototype = o;
    return new P();
}

var subperson = createObj(person1);
console.log(subperson.getName()); // suo
```
很明显当然是可以的，我们由此也想到了，这和复制一个对象有什么区别呢？当然是有区别的，本质来说，这种继承方法是一个 __浅复制__，__虽然复制了对象的属性，但是引用型的属性仍然是共享的__。验证一下
```js
var subperson1 = createObj(person1); 
var subperson2 = createObj(person1);
subperson1.nameGroup.push("smile");
console.log(subperson2.nameGroup); 
// (3) ["suo", "yue", "smile"]
```

ES 新增了一个`Object.create()`方法规范了这个原型式继承。只有一个参数的情况下两者效果是一样的。第二个参数是可选的，作用是可以设置属性特性。这和我们之前谈到的`Object.defineProperties()`是同等效果的。测试一下
```js
var person = {
    name: "suo",
    age: 18
};
var subperson1 = Object.create(person, {
    name: {
        value: "yue",
        enumerable: false // 设置不可枚举
    },
    gerder: {
        value: "male"
    }
});
console.log(subperson1.name + ',' + subperson1.gender); 
// male, undefined 说明不能自己增加属性

for (prop in subperson1) {
    console.log(prop); // age 
}
```

__(5)寄生式继承__
寄生式继承与刚刚学习的原型式继承紧密相连，在原型式继承的基础上又封装了一道函数。直接上代码吧
```js
var person = {
    name: "suo",
    mess: "bye"
};
// 原型式继承
function creatObj(o) {
    function P() {};
    P.prototype = o;
    return new P();
}
// 寄生式继承
function createAnobj(o) {
    var clone = createObj(o);
    clone.sayBye = function() {
        return o.mess;
    };
    return clone;
}
// 测试
var anoperson = createAnobj(person);
console.log(anoperson.name + ',' + anoperson.sayBye());
// suo,bye
```
咋一看，这不就是在原型式的基础上加一个添加方法的函数吗。当然完全是，它还有另一个用途，如果对象不是自定义或者构造函数时，它也是有用的，随便举个例子
```js
var arr = [1, 2, 3];
function createAnobj(o) {
    var clone =  o; // 原型式函数不是必须的
    o.printFirst = function() {
        console.log(o[0]);
    }
    return clone;
}
var arr2 = createAnobj(arr);
arr.printFirst(); // 1
```

__(6)寄生组合继承__
我们知道原型链与构造函数的组合模式是最常见的继承方式，但是它也有不足的地方。它们单独来将每次调用一次父类型，组合起来就是调用了两次父类型。我们现在有一种方法来解决这个问题，就是组合寄生继承模式。__它的思路就是构造函数模式不变，不直接调用父类型函数，而是通过原型模式创建一个副本，然后让子类的原型等于这个副本。__
```js
// 父类型
function Person(name) {
    this.name = name;
    this.nameGroup = ["xiao", "ai"];
}

Person.prototype.getName = function() {
    return this.name;
}
Person.prototype.addName = function() {
    this.nameGroup.push(this.name);
}
// 子类型
function SubPerson(name, gender) {

    // 继承属性
    Person.call(this, name);
    // 自己属性
    this.gender = gender;
}

// 原型式模式
function createObj(o) {
    function P() {}
    P.prototype = o;
    return new P();
}

// 寄生模式
function inheritPrototype(p, subp) {
    // 创建对象，浅复制原型对象
    var prototype = createObj(p.prototype);
    // 增强对象，修正构造函数
    prototype.constructor = subp;
    // 指向对象，子类型指向创建并修正构造的父类型原型对象
    subp.prototype = prototype;
}

// 执行寄生模式
inheritPrototype(Person, SubPerson);

SubPerson.prototype.sayBye = function() {
    console.log("bye");
}


// 测试
var subperson1 = new SubPerson("suo", "male");
console.log(subperson1.getName()); // 父类型属性和方法
console.log(subperson1);
console.log(subperson1.gender); // 自己的属性
subperson1.addName(); // 父类型的方法
console.log(subperson1.nameGroup);
subperson1.sayBye(); // 自己的方法

var subperson2 = new SubPerson("yue", "female");
console.log(subperson2.nameGroup); 

// 输出
// suo
// male
// bye
// (3) ["xiao", "ai", "suo"]
// (2) ["xiao", "ai"]
```
写到这里，只能感叹一句，__寄生组合模式简直就是一个大杂烩啊__。
