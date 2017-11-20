---
title: 《JavaScript高级程序设计》读后记<六>：对象
date: 2015-12-05 12:43:11
categories: JavaScript
tags: [JavaScript, OOP]
---
上篇博客谈到了引用类型，array, function 等等。它们都有一个共同的特点就是，它们都继承于 object，它们都是对象，拥有属性和方法。我虽然搞清楚了它们的很多特性和方法，也可以用它们做一些事情。但是对象这个东西还是有很多东西是有些模糊的，这些天我看完了面向对象这一章。里面很多设计和 c++, Java 面向对象很相似，有共通的地方，但是也有很多地方是 JS 独有的特性。现在我整理出了一条思路，写下这篇博客，希望能更加透彻的理解面向对象的思想。

### 理解 JavaScript 面向对象
前面已经接触过了很多对象，现在稍稍回顾一下，话不多说，上代码
```js
// 直接new对象
var person = new Object;
person.name = "suo";
person.getName = function() {
    console.log(this.name);
}; 
person.getName(); // suod

// 字面量创建对象
var person1 = {
    name: "suo",

    getName: function() {
        console.log(this.name);
    }
}; 
person.getName(); // suo
```
这两种创建对象的方法都是最初级的，第二个比一个好那么一点点
在使用更高级的创建方法之前要讨论一下对象的属性

#### 理解属性的特性

__ES5 定义了特性(attribute)，它是内部使用的，用来描述属性(property)的特征__。这句话怎么解释呢？特性和属性，我们所知道的属性，就是对象的属性，方法。但是这些属性有一些特征，而把属性的特征称为特性。并且用两对方括号表示 [[Enumerable]]

下面具体谈谈这些属性的特性。__分为两种特性，一种是数据特性，一种是访问器特性__
- [[Configurable]]: 能否删除而重新定义属性，能否修改属性特性
- [[Enumerable]]: 能否通过 for-in 返回属性
- [[Writable]]: 能否修改属性的值
- [[Value]]: 包含属性的值

下面我们举个例子具体说明这些特性的意义
```js
var person = {
    name: "suo"
};
```
`name`是对象`person`的属性，[[Value]] 的值就应该是`"suo"`，其中`name`是可以删除和重新定义的。可以通过 for-in 返回属性，可以修改值，由此我们得出上面三个特性的默认值都是`true`

既然属性的特性有默认的值，那么是否可以修改呢，答案是可以的
ES5 有一个方法可以修改属性的特性值`Object.defineProperty()`
```js
var person = {};
Object.defineProperty(person, "name", {
    writable: false, // 设置特性为不能修改
    value: "suo"     // 设置属性的值为"suo"
});
console.log(person.name); // suo
person.name = "yue"; // 严格模式下导致错误
console.log(person.name); // suo
```
我们看到我们可以通过这个方法直接为属性设置值，同时设置了只读权限

现在我们知道属性的特性是可以重新定义和修改的，但是我们也知道特性里有一个 [[Configurable]] 可以控制是否可以修改特性，也就是设置`Configurable`为`false`，就不能再修改这个对象属性的特性了(除`writable`以外)，下面我们验证一下
```js
var person = {};
Object.defineProperty(person, "name", {
    configurable: false, //设置不能删除属性，不能重新定义特性
    value: "suo"
});
Object.defineProperty(person, "name", {
    value: "yue" // 出错。不能重新定义特性
});
delete person.name; // 不起作用，严格模式报错
console.log(person.name);
```
现在看看访问器特性，包含 getter 和 setter 函数，读取访问器属性调用 getter，写入调用 setter
同样的访问器特性也是四个，其中前两个和数据特性一样，后两个如下
- [[Get]]: 读取属性时调用的函数，默认值是`undefined`
- [[set]]: 写入属性时调用的函数，默认也是`undefined`

同样我们来看看代码
```js
// 假设suo是男生，yue是女生，当改变对象名字的时候，使得性别也改变
var person = {
    _name: "suo", // 只能通过对象方法访问
    gender: "male"
};
Object.defineProperty(person, "name", {

    get: function() {
        return this._name;
    },

    set: function(name) {
        if (name === "yue") {
            this.gender = "fel" + this.gender;
        }
    }
});
person.name = "yue";
console.log(person.gender); // female
```
__说了这么多，那么使用这些特性有什么好处呢？__
`get`函数内只能读，`set`函数内只能写，这样就完美的实现了读写分离，支持这个方法的需要 IE9 以上
上面用的都是单个属性的操作，也有可以一次操作多个属性的方法`defineProperties()`
```js
var person = {
    _name: "suo",
    gender: "male"
};
Object.defineProperties(person, {
    _name: {
        writable: false,
        value: "yue"
    },

    gender: {
        writable: true,
        value: "male"
    },

    name: {
        get: function() {
            return this._name;
        },

        set: function(name) {
            if ("yue" === this._name) {
                this.gender = "fe" + this.gender;
            }
        }
    }
});
person.name = "yue";
console.log(person.gender); // female
```
这个方法使用的效果和上面单个方法别无二致，唯一不同的是，这些特性是同一时间创建的
现在我们来读取这些特性，使用方法`getOwnPropertyDescriptor`
```js
var descriptor = Object.getOwnPropertyDescriptor(person, "_name");
console.log(descriptor.value); // yue
```
前面谈了那么多关于属性的特性，现在我们该进入正题了

#### 创建对象的方法
__(1)工厂模式__
 考虑到`ES`没法创建类，所以就采用了函数封装特定的接口创建对象，下面直接上代码
```js
// 工厂方法：批量生产对象
function createPerson(name, gender) {
    var o = new Object;
    o.name = name;
    o.gender = gender;
    o.getName = function() {
        return o.name;
    };
    return o;
}
var person1 = createPerson("suo", "male");
var person2 = createPerson("yue", "female");
```
这样写的好处就在于，本来需要单个创建的对象，通过函数的封装，可以批量的创建对象了。
其实它是有缺点的，__虽然解决了相似对象创建的问题，但是对象的识别没法解决__。

__(2)构造函数__
我们知道引用类型是通过原生的构造函数创建对象，其实构造函数是可以自定义的。所以我们现在可以通过自定义的方式来创建对象，上代码
```js
function Person(name, gender) {
    this.name = name;
    this.name = gender;
    this.getName = function() {
        return this.name;
    };
}
var person1 = new Person("suo", "male");
var person2 = new Person("yue", "female");
```
这种方式和工厂模式其实很相似，大家可能一看就知道了。它们之间有些稍稍不同的地方，这些其实很好解释，知道构造函数生成对象的过程，就很容易理解了，这两种方式其实是一个原理。
__构造函数，new 的过程__:
- 创建一个新对象
- 将作用域给新对象
- 给对象添加属性
- 返回对象

现在也许大家都明白了，原来我们使用的工厂模式，其实就是在模拟构造函数生成对象的过程
构造函数生成的对象都有一个 constructor 属性，它就是指向构造函数本身的
```js
console.log(person1.constructor == Person); // true
console.log(person1 instanceof Person); // true
```
由此我们可以看出，__构造函数相比工厂模式的好处就在于，它解决了对象的识别问题__，我们可以通过这些方式来判断，这个对象到底是由哪个函数构造的。
当然这种模式也是存在缺陷的，虽然我们利用工厂模式和构造函数，生产了很多对象。但是每生产一个对象，就要给对象里的属性和方法分配一块内存，然而对象的方法很多都是一样的。__这样就导致了内存的大量的浪费__，我们能否让这些方法共享呢？现在尝试独立这些共享方法
```js
function Person(name, gender) {
    this.name = name;
    this.gender = gender;
    this.getName = getName;
}
function getName() {
    return this.name;
} // 独立方法
var person1 = new Person("suo", "male");
console.log(person1.getName()); // suo
```
虽然这是可行的，但是污染了全局作用域。`getName`虽然是全局的，单真正确是用在构造函数里面。这在开发中是一种及其不好的做法。

__(3)原型模式__
我们之前谈到函数类型，它都有一个 prototype 属性，当时没有过多的研究，现在可以好好探究一下了。首先这个属性是一个指针，指向一个对象，这个对象里面包含了所有实例共享的属性和方法。到这里你肯定明白了，__原来这个原型属性就是为了解决构造函数无法共享属性和方法的啊__。上代码
```js
function Person() {}
Person.prototype.name = "suo";
Person.prototype.gender = "male";

Person.prototype.getName = function() {
    return this.name;
};
var person1 = new Person();
var person2 = new Person();
console.log(person1.getName()); //suo
console.log(person2.getName()); //suo
```
很明显，原型对象的属性和方法，对于对象来说都是公有的，大家都一样。
下面继续深挖一下原型对象这种东西，有助于之后学习的理解
__只要创建函数，就会有一个 prototype 属性，这个属性指向了函数的原型对象。默认情况下所有的原型对象都会获得一个 constructor 属性，这个 constructor 是一个指向这个 prototype 所在的函数的指针__。这个关系有点微妙，我还是画个图说明一下吧
![img](/images/dm3.png)
代码验证一下
```js
console.log(Person.prototype.constructor); // f Person(){}
console.log(Person.prototype.constructor == Person); // true
```
其中我们可以通过`isProtptypeOf()`方法验证对象原型的对应关系
```js
    console.log(Person.prototype.isPrototypeOf(person1)); // true
```
`ES5`新增了`getPrototypeOf()`方法来获取原型值
```js
console.log(Object.getPrototypeOf(person1));

// {  name: "suo", gender: "male", getName: ƒ, constructor: ƒ }
//    gender: "male"
//    getName: ƒ ()name:"suo"
//    constructor: ƒ Person()
//    __proto__: Object
```
上面我们通过这个方法把原型的全部内容打印了出来，这些都清楚了是吧
虽然可以通过对象访问原型的值，但是不能通过对象重写原型的值，这个原因很简单，因为我们知道，原型的属性和方法都是共享的，如果随便一个实例都能改动的话，原型就乱套了，__改变一个原型的属性就会影响其他的实例__。因此原型是不允许实例改变的。__如果实例的属性名字与原型名字重名的话，它会屏蔽原型的属性__。测试一下
```js
person1.name = "yue";
console.log(person1.getName()); // yue
console.log(person2.getName()); // suo

delete person1.name; // 删除属性
console.log(person1.getName()); // suo
```
我们现在知道了，实例的属性是可以和原型重名的，那么怎么判断它到底是谁的属性呢，__`hasOwnProperty()`可以来检测这个属性到底是实例的还是原型的__
```js
person1.name = "yue";
console.log(person1.getName()); // yue
console.log(person1.hasOwnProperty("name")); // true
delete person1.name; // 删除属性
console.log(person1.getName()); // suo
console.log(person1.hasOwnProperty("name")); // false
```
由此可知，`true`就是实例的，`false`就是原型的
现在我们没那么严苛，__我们想要判断该属性是否可以被实例访问到，这里就有`in`方法可以判断__
```js
person1.name = "yue";
console.log("name" in person1); // true
delete person1.name;
console.log("name" in person1); // true
```
__除此之外，如果我们还想得到所有实例的属性，也有种方法使用，for-in__，当然有些不可枚举的属性是访问不到的。我们尝试去做一下
```js
for (prop in person1) {
    console.log(prop);
}
// name
// gender
// getName
```
我们发现没有 protptype 属性，原因是它都是不可枚举的，[[enumerable]] 为`false`
除此之外我们还可以使用`Object.keys()`方法来枚举对象属性
```js
console.log(Object.keys(Person.prototype));
// (3) ["name", "gender", "getName"]
```
关于原型的写法，同样我们可以通过字面量来批量写原型属性，方法
```js
function Person() {}
Person.prototype = {
    constructor: Person, // 构造函数还原
    name: "suo",
    gender: "male",
    getName: function() {
        return this.name;
    }
};
var person1 = new Person();
```
要注意的一点是，字面量本身也是一个对象，__原型指向字面量后指针就跑歪了，所以要我们要让它重新指向函数。这种方式仍然有个问题就是__，本身 prototype 属性是不可枚举的，现在把 constructor 加上去后，导致变成可枚举的了，现在属性的特性的知识就有用武之地了，我们可以直接设置它的特性为不可枚举的，那不就可以了吗。
```js
function Person() {}
Person.prototype = {
    name: "suo",
    gender: "male",
    getName: function() {
        return this.name;
    }
};
var person1 = new Person();
// 添加属性值并设置特性不可枚举
Object.defineProperty(Person.prototype, "constructor", {
    enumerable: false,
    value: Person
    });
```
我们之前谈到不能用实例修改原型，但是我们可以直接在原型上做修改啊，修改的原型后会对实例有什么影响呢？测试一下
```js
Person.prototype.name = "yue";
console.log(person1.name); // yue
```
由此我们可知，__即使是先创建了实例，修改原型属性后，实例访问的原型属性也会修改，说明原型是动态的__，这其实也很简单说明。实例和原型本身就不是绑定的，我们通过原型访问，是通过指针访问的。原型里的属性改变了，我们再次通过指针访问时，当然也是改变后的属性了。
说了这么多，看起来原型挺好的，但是光有原型也是不够的，我们知道原型的方法和属性都是共享的，那么我私人的属性和方法该怎么办呢？__那么为什么不把这两种模式结合起来呢？__

__(4)组合模式(构造+原型)__
结合构造和原型，我们来试着创建一个对象吧
```js
function Person(name, gender) {
    this.name = name;
    this.gender = gender; 
}

Person.prototype = {
    // constructor: Person,
    origin: "monkey",   // 起源
    getName: function() {
            return this.name;
    }
};

Object.defineProperty(Person.prototype, "constructor", {
    enumerable: false,
    value: Person
});

var person1 = new Person("suo", "male");
var person2 = new Person("yue", "female");
console.log(person1.name === person2.name); // false
console.log(person1.origin === person2.origin);  // true
```
我们使用图示来说明这个方式
[img](/images/dm4.png)

__(5)动态原型__
学过了其他 oop 语言，像c++，Java 都是用类封装所有的属性和方法，倒是觉得 ES 的比较奇怪了。现在就有一种方法来动态的创造原型，__需要时才创建原型属性和方法__
```js

function Person(name, gender) {
    this.name = name;
    this.gender = gender;
}
if (typeof this.getName != "function") {
    Person.prototype.getName = function() {
        return this.name;
    };
}
var person1 = new Person("suo", "male");
console.log(person1.getName()); // suo
```
__只有当调用这个方法时，这个方法不存在，它才会被添加在原型里__，使用动态模式不能用字面量方法给原型赋值，原因就是，它会将 constructor 导向新的对象，之前也遇到过。

__(6)寄生构造__
一句话，使用构造函数的工厂模式创建对象
```js
function Person(name, gender) {
    var o = new Object;
    o.name = name;
    o.gender = gender;
    o.getName = function() {
        return this.name;
    }
    return o;
}
```
咋一看，__这不就是在构造函数里再造一个工厂模式吗？__到底有啥用
其实这个寄生构造函数的用途，__在于对原生的构造函数进行修改，重新造一个构造函数__，比如下面
```js
function SpecialArray() {
    // 创建数组对象
    var arr = new Array;
    // 添加参数
    arr.push.apply(arr, arguments);
    // 添加方法
    arr.toUpdateJoin = function() {
        return this.join("|");
    };
    // 返回数组
    return arr;
}
var arr = new SpecialArray("suo", "yue");
console.log(arr.toUpdateJoin()); // suo|yue
```
这样就改造好了，把原来数组的连接改成了`|`。

__(7)稳妥构造__
某个人发明了稳妥对象概念，什么是稳妥对象呢，__它其实就是没有公共属性，不引用 this 对象__。借鉴寄生构造函数，实现这个稳妥构造，上代码
```js
function Person(name, gender) {
    var o = new Object;
    o.getName = function() {
        return name; //注意这里没this
    };
    return o;
}
var person = new Person("suo", "male");
console.log(person.getName()); // suo
```
这种模式非常安全，里面没有公共属性和 this ，这样外界要访问到`name`，只能通过函数来访问了。写了这么多也差不多把面向对象搞清楚了一半，下一篇博客，专门研究面向对象里的 __继承__。

