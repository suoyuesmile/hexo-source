---
title: 《JavaScript高级程序设计》读后记<五>：引用类型
date: 2015-12-02 16:44:12
categories: JavaScript
tags: JavaScript
---
前几篇博客，谈到了变量有 2 类数据类型，5 种基本数据类型和引用数据类型。同时也阐述了基本数据类型和引用数据类型的区别。但是一直没有提到具体的引用类型，今天专门看了引用类型一章，发现里面涉及的内容还挺多的。一时难以记住全部的内容，所以在这里写一篇博客，留做以后的学习作参考。

### 理解引用类型
引用类型的值(对象)是引用类型的实例，这和基本类型也是一样的，基本类型的值当然也是基本类型的实例。它们唯一的区别在于引用类型是一种数据结构，它的数据的组织更加复杂些。
前面也提到过，对象的创建是用 __new__ 方法创建的。这里的所有的引用类型都类似，可以用 new 方法创建。
```js
var person = new Object; // 同样的括号可以省略
```

#### Object类型
除了上面那种创建 object 方法外还有种方法，使用对象字面量来表示
```js
var person = {
    name : "suo",
    gender : "male"
};
```
除此之外还有另一个写法，就是空对象，后添加属性，这也是可以的
```js
var person = {};
person.name = "suo";
person.gender = "male";
```
同时我们可以看到，object 访问属性是通过`.`来访问，其实也有另一种方法，通过`[]`来访问
```js
var person = {};
person["name"] = "suo";
person["gender"] = "male";
console.log(person["name"]+','+person["gender"]); // suo,male
```
__那么问题来了，什么时候用`.`，什么时候用`[]`呢？__
`[]`表示法有个优点，就是他可以通过变量访问属性
```js
var person = {};
person["name"] = "suo";
person["gender"] = "male";
var suo = "name"; // 将属性字符串赋值给变量
console.log(person[suo]+','+person["gender"]); // suo,male
```
除此之外，都用`.`来表示，毕竟`.`表示更加方便简洁

#### Array类型
上面提到了 object 类型，下面具体谈一下 array 类型。其实感觉他俩挺像的，为什么这么说呢？
之前一段时间我一直在学习数据结构。真正的体会到了，很多看似不同的东西，其实在结构上是很相似的，甚至只是改进了一下数据的组织方式而已。
我们可以想象一下`object`的数据结构
```js
{
    key1 : value1,
    key2 : value2,
    ...
};
```
其中`value`也可以是函数，对比一下数组的
```js
[
    0 : value1,
    1 : value2,
    ...
];
```
其中数组中的下标是在字面量中省略的
很明显，它们的数据结构都是线性的序列，区别在于两点
1.object 的元素，可以是任意类型的。array 的一般是同一类类型的元素
2.object 是关联容器结构，array 是顺序容器结构。array 是寻秩访问，object 是寻关键码访问。
__谈到了它们之间的区别与联系，现在具体说一下 array 吧__
创建 array 也是有两种方法，一种是 new ,一种是字面量。和 object 差别不大
```js
var arr = new Array; // 同理可省略
for (var i = 0; i < 10; i++) {
    arr[i] = i + 1;
    console.log(arr[i]);
} // 1 2 3 4 5 6 7 8 9 10
```
省略括号表示创建一个空的数组，数组不仅可以创建空数组，还可以创建想要的形式的数组
```js
var arr = new Array(3); // 创建包含三个元素的数组
var arr1 = new Array("hello"); // 创建包含一项元素"hello"的数组
```
另外一种方式是字面量的，这种方式创建数组更加灵活
```js
var arr = []; // 空数组
var arr1 = [1, 2, 3, 4];
var arr2 = ["hello", "world", "!"];
var arr3 = [ , , ]; // 不建议使用，会生成undefined变量
```
数组的长度可以用 length 得到
```js
var arr1 = [1, 2, 3, 4];
console.log(arr1.length); // 4
arr1[arr1.length] = 5; // 添加一项
console.log(arr1[4]); // 5
```
之前谈到可以用`instanceof`来检测数组，但是它只能在一个全局内检测。所以 ES5 新增了一个方法来检测数组。
```js
var arr = [1, 2];
console.log(Array.isArray(arr)); // true
```
`Array`有很多类型的方法，我们先归一下类，以后再慢慢细究吧。
(1)转换方法
```js
var colors = ["red", "blue", "green"];
console.log(colors.toString()); // red,blue,green
console.log(colors.valueOf()); // red,blue,green
console.log(colors); // red,blue,green
console.log(colors.join("|")); // red|blue|green
```
(2)栈方法
```js
var colors = ["red", "blue", "green"];
colors.push("yellow")
console.log(colors); // ["red", "blue", "green", "yellow"]
colors.pop(); 
console.log(colors); // ["red", "blue", "green"]
```
(3)队列方法
```js
var colors = ["red", "blue", "green"];
colors.push("yellow", "black");
var pop = colors.pop(); // black 
var head = colors.shift();  // red
colors.unshift("red", "black")
console.log(pop+','+head); 
console.log(colors); // ["red", "black", "blue", "green", "yellow"]
```
(4)重排序方法
```js
// 反转组项顺序
var num = [1, 3, 2, 5,  4];
num.reverse();
console.log(num); // 4,5,2,3,1
//升序排序
num.sort();
console.log(num); // 1,2,3,4,5
```
(5)操作方法
```js
// 粘贴(新数组)
var colors = ["red", "blue", "green"];
var colors2 = colors.concat("yellow", ["black", "white"]);
console.log(colors2);
// (6) ["red", "blue", "green", "yellow", "black", "white"]

// 截切(新数组)
var colors = ["red", "blue", "green"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(0, 1); // 不包含1
console.log(colors2); // ["blue", "green"]
console.log(colors3); // ["red"]co

// 替换(原数组上操作)
var colors = ["red", "blue", "green"];
colors.splice(1, 1);
console.log(colors)
colors.splice(1, 0, "yellow");
console.log(colors)
colors.splice(1, 1, "black");
console.log(colors)
//(2) ["red", "green"]
//(3) ["red", "yellow", "green"]
//(3) ["red", "black", "green"]
```
(6)位置方法
```js
// 正向查找
var num = [1, 3, 2, 5, 4]; 
console.log(num.indexOf(3)); // 1
// 逆向查找
console.log(num.lastIndexOf(3)); // 1
```
(7)迭代方法
```js
// 每项都
var num = [1, 3, 2, 5, 4];
var res = num.every(function(item, index, array) {
    return (item > 2);
    });
console.log(res); // false
// 每项结果
var res2 = num.filter(function(item, index, array) {
    return (item > 2)
    });
console.log(res2); // [3, 5, 4]
// 每项运行
var res3 = num.forEach(function(item, index, array) {
    num = num * 2;
    });
console.log(res3);
// 每次结果数组
var res4 = num.map(function(item, index, array) {
    return (item * 2)
    });
console.log(res4);
// 任一项
var res5 = num.some(function(item, index, array){
    return (item > 2);  
    });
console.log(res5);
```
(8)归并方法
```js
// 正向归并
var num = [1, 3, 2, 5, 4];
var sum = num.reduce(function(prev, cur, index, array){
    return prev + cur;
    });
console.log(sum); // 15
// 逆向归并
var num = [1, 3, 2, 5, 4];
var sum = num.reduceRight(function(prev, cur, index, array){
    return prev + cur;
    });
console.log(sum); // 15
```

#### Date类型
我们可能学习过 Java 中的 Date 类型，其实 ES 也是借鉴它构建的。使用的也是 UTC 来保存日期。保存的日期的范围为 1970.1.1 前后的 1 亿年。
使用`new`创建
```js
var now = new Date;
console.log(now); // Sun Dec 02 2016 11:10:38 GMT+0800 (中国标准时间)
```
__由此得知，Dete对象默认创建的是当前的时间，那怎么得到特定的时间呢?__
有两种方法：一种是`Date.parse()`，另一种是`Date.UTC()`
```js
var someDate = new Date(Date.parse("Nov 12, 1995"));
console.log(someDate); // Fri Nov 12 1995 00:00:00 GMT+0800 (中国标准时间)
// 省略Date.parse也是可以的
var someDate2 = new Date("Sep 22, 1996");
console.log(someDate2); // Sun Sep 22 1996 00:00:00 GMT+0800 (中国标准时间)
```
上面的方法返回的是日期对象，下面再测试一下`Date.UTC()`
```js
var someDate = new Date(Date.UTC(1995, 11, 12));
console.log(someDate); // Sun Dec 12 1995 08:00:00 GMT+0800 (中国标准时间)
// 省略
var someDate2 = new Date(1995, 11, 12);
console.log(someDate2); //Tue Dec 12 1995 00:00:00 GMT+0800 (中国标准时间)
```
由此我们得知，这两种方法都是可以自动调用的，调用哪一种方法取决于传入的参数。
`ES5`新增了一种`now()`方法，可以取得当前时间的毫秒数，那么我们可以用它做一些有用的事
```js
var start = Date.now();
for (var i = 0; i<1000000; i++) {
    sum = i*2;
}
var end = Date.now();
var runtime = end - start;
console.log(runtime); // 10(ms)
```
用+操作符也可以达到同等目的
```js
var start = new +Date();
```
因为时间本身是毫秒数，所有可以用`>`或者`<`比较日期
```js
var date1 = new Date(2016, 2, 3);
var date2 = new Date(2015, 3, 4);
console.log(date1 > date2); // true
```
格式化日期
```js
var date = new Date;
console.log(date.toDateString()); // Sun Dec 02 2016
console.log(date.toTimeString()); // 11:53:46 GMT+0800 (中国标准时间)
console.log(date.toLocaleDateString()); // 2016/12/2
console.log(date.toUTCString()); // Sun Dec 02 2016 11:53:46 GMT
```
日期组件方法，都是一些 get，set 方法，这里就不一一说了
```js
var date = new Date;
console.log(date.getTime()); // 1506830282288(ms)
```

#### RegExp类型
我学过 PHP 的正则表达式，学过 Java 的正则表达式，很有意思的是现在又学`JS`的正则表达式，相对来说容易很多了，除了调用的方法名有所不同以外，其他的内容几乎无差别
首先创建一个正则表达式，同样是两种方式
```js
var pat = new RegExp("pat","flags"); // new对象
var pat = /pat/flags; // 字面量
```
稍微解释一些这个表达式的含义，`/`这个是表达式的定界符，就是隔离正则与其他字符的一个分界，为其他字符也可以，表达式后面的`flags`是一个标记，就是来切换正则表达式匹配规则的模式。
常见的模式有 g 表示全局模式，i 不区分大小写，m 多行模式，举几个例子说明下
```js
var pat = /suo/i;
```
解释一下，`suo`是正则法则，i 是模式，也就是匹配字符`suo`,且不区分大小写
使用`test`来测试是否匹配上
```js
var pat = /suo/i;
console.log(pat.test("Suo")); // true
console.log(pat.test("SUO")); // true
console.log(pat.test("sso")); // false
```
ES5 规定，使用字面量创建正则必须像直接调用 RegExp 构造函数一样，每次都要创建新的实例。
下面我们来看看实例的属性和方法
```js
var pat = /suo/i;
console.log(pat.global); // false
console.log(pat.ignoreCase); // true
console.log(pat.multiline); // false
console.log(pat.lastIndex); // 0
console.log(pat.source); // suo
```
这些属性都是正则表达式本身具有的一些属性，没啥用，
但是它的两个方法是我们要掌握的，__第一个就是`exec()`它是用来捕获组的__，
```js
var pat = /suo/ig; // 全局模式
var str = "suo: I am Suoyue";
var matches = pat.exec(str);
// 第一次捕获
console.log(matches.index); // 0
console.log(matches.input); // suo: I am suo yue"
console.log(matches[0]); // suo
// 第二次捕获
matches = pat.exec(str);
console.log(matches.index); // 10
console.log(matches[0]); // Suo
```
正如上面所说，每一次捕获都要创建实例，都要再执行一遍捕获方法
__另一种方法是`test()`方法，他是一个判断是否匹配，正如它的名字，只是测试而已__
```js
var pat = /suo/i;
var str = "suo: I am Suoyue";
console.log(pat.test(str)); // true
```
__假设我现在捕获到了一个组，我们怎么取得我们想要的东西呢？__
答案是使用RegExp的构造函数属性
```js
var pat = /suo/ig;
var str = "ss suo: I am Suoyue";
if (pat.test(str)) {
    console.log(RegExp.$_);    // 最近一次要匹配的字符串 suo: I am Suoyue
    console.log(RegExp["$`"]); // 最近匹配项前面的字符串 ss
    console.log(RegExp["$'"]); // 最近匹配项后面的字符串 : I am Suoyue
    console.log(RegExp["$&"]); // 最近一次匹配项 suo
    console.log(RegExp["$+"]); // 最近匹配的捕获组 [空]
    console.log(RegExp["$*"]); // 是否使用多行模式 (未实现)
}
```
除了上面 6 个以外还有很多，不一一累述了
虽 ES 正则表达式功能还是比较完备的，但是对于 PHP 和 Java  还是缺少很多高级特性，作为一个前端的脚本语言，这些已经足够了

#### Function类型
前面我们一直提到函数，他是对象，它可以赋值给变量，它是 ES 中最小的作用域。但是一直我都没有具体研究它，现在我读了函数这章，很多之前的稍有疑问的地方，现在都豁然开朗了。

之前也说过，__函数有两种表达方式，一种是使用函数声明，一种是使用函数表达式__。我们稍微回顾一些这两种形式。
```js
// 函数声明式
function foo() {
    //code...
}
// 函数表达式
var foo = function() {
    //code...
}; 
```
注意的是函数声明是会有函数声明提升的，之前也讲过，这里不在累述了。
由上面可以看出，函数包含函数名，函数本体，也就是`{}`里面的内容

函数实际是对象，__那么函数名实际上是一个指向函数对象的指针，不会与某个函数绑定__
这句话怎么理解呢？下面给出一个例子来说明这个问题
```js
var foo = function() {
    console.log("suo");
}
var boo = foo;
foo = null;
// 经测试foo解引用后，运行会出错
boo(); // suo
```
由上面的代码可以看出，`foo`只是一个指向函数对象的指针，当使用解引用后，断绝了这个关系了。而`boo`又指向了函数对象，所以可以运行。

下面我们来探讨另一个问题，既然我们已经得出结论，函数名只是一个指向函数对象的指针。那函数就不应该有重载这个特性。因为同一个函数名是指向同一个函数对象的。就不会指向其他对象的说法
下面我们也来例证一下
```js
var name = "suo";
var gender = "male";
var foo = function(name) {
    console.log(name);
}
var foo = function(name, gender) {
    console.log(name+','+gender);
}
foo(name); // suo,undefined
foo(name, gender); // suo,male
```
由上可知，如果函数有重载的话，第一个`foo`执行的结果应该是`suo`，但是实际的结果确实`suo`，`undefined`。很显然第一个函数执行的也是第二个函数表达式。第二个表达式的函数覆盖了之前的。
得出结论，__函数重名会覆盖，不管参数是怎样的__

上面也提到了函数是对象，那么函数就可以给其他变量赋值。不仅如此函数还可以作为返回值用。
下面看一个例子
```js
// 比较人的身高
var suo = {
    height : 180
};
var yue = {
    height : 170
};
function campareHeight(height) {
    return (function(suo, yue) {
        if (suo.height > yue.height) {
            return 1;
        } else if (suo.height < yue.height) {
            return -1;
        } else {
            return 0;
        }
    })(suo, yue);
}
console.log(campareHeight("height")); // 1
```
这里要提醒下，函数声明是不可以直接做返回值的，只有执行后的函数才能做返回值.一般这种情况我，使用立即执行函数一步来搞定，下面写一个立即执行函数的简单例子
```js
// 声明与执行分步写
function foo() {
    return 1;
}
console.log(foo()); // 1
// 使用立即执行函数一步写
console.log( (function(){
    return 1;
    })() ); // 1
```
__函数内部有两个特殊的对象：arguments 和 this__
arguments 是一个类数组对象，包含传入函数的所以参数主要用来保存函数参数的，这个对象有一个叫 callee 的属性，它是一个指针，指向拥有这个 arguments 对象的函数。
下面一个例子告诉我们caller的用法
```js
function foo(num) {
    if (num <= 1) {
        return 1;
    } else {
        return num * arguments.callee(num-1);
    }
}
foo(10); // 3628800
```
其实这个就是 10 的阶乘。关键在于这行代码`arguments.callee(num-1)`，我们再仔细揣摩这句话，__callee 的属性，它是一个指针，指向拥有这个 arguments 对象的函数__，现在明白了`arguments.callee(num-1)`就等同于`foo(num-1)`，这样就清楚了。
该说说 this 了，我看了半天书上的叙述，总结了一句话，__当在哪个作用域调用函数时，该函数中的 this 就是哪个作用域对象__
下面验证下这句话的正确性
```js
// 全局调用
window.color = "red";
var o = {
    color: "blue"
};
function getColor() {
    console.log(this.color);
}
getColor(); // red
o.getColor = getColor;
o.getColor(); // blue
```
另一个是`caller`，表示调用当前函数的函数的引用，全局作用域为`null`，验证一下
```js
function foo() {
    boo();
}
function boo() {
    console.log(boo.caller); 
}
console.log(foo.caller); // null
foo(); // ƒ foo() { boo();}
```
这里可以看到显示了 foo 的源码，下面用另一种方式实现这个效果(根据 callee 的用法好理解)
```js
function foo() {
    boo();
}
function boo() {
    console.log(arguments.callee.caller); 
}
console.log(foo.caller); // null
foo(); // ƒ foo() { boo();}
```
这种写法虽然现在可以，但是出于安全性考虑，严格模式下已经不许这样做了，另外严格模式下函数的 caller 属性是不能赋值的

都说对象是有属性方法的，函数也不例外，下面谈谈函数的属性和方法吧
函数有两个属性一个是 leghth，一个是 prototype ，前面一个是指希望接受参数个数，没什么好说的。关键在于这个 prototype 属性,这个属性是ES搞面向对象专门搞得一个属性，这里不谈太多了，下一篇博客研究对象和继承时。好好探究这个属性。这里就简单提几点，ES5 中，prototype 属性太多，是无法使用 for-in 枚举的。

下面我们聊聊,__函数的两个独有的方法__，说独有是因为它不是其他对象有的，因为它们关系到作用域。我们知道 ES 就只有函数作用域，所以这一切搜说得通了。
这两个方法，一个是`apply()`,一个是`call()`,这两个函数有很多相似之处。
```js
function foo(a, b) {
    return a>b;
}
function callFoo(c, d) {
    return foo.apply(this, arguments);
}
callFoo(1, 2); // false
```
这里我的理解是，apply这个函数，是将 callFoo 的 this 对象和 arguments 传给了 foo，并执行结果返回给 callFoo，也可以不用自己的 arguments 对象，随意传一个数组也可以。因为 arguments 本身也是一个数组。如下
```js
function foo(a, b) {
    return a>b;
}
function callFoo(c, d) {
    return foo.apply(this, [3, 2]); // 替换成自己的
}
// 不写参数看看
callFoo(); // true
```
果然是这样，证明我的理解是正确的。
理解了 apply，call 也就好理解了，它们区别就在于参数，不同于 apply，一个作用域和一个参数数组。__call 的参数要全部单独写出来__。改变上面的代码，看看
```js
function foo(a, b) {
    return a>b;
}
function callFoo(c, d) {
    return foo.call(this, 3, 2); // 参数数组变成单个参数
}
callFoo(); // true
```
一切都在意料之中，果然是这样。
说了这么这两个方法，它们有啥用呢？__它们的真正用处在于扩充函数的作用域__
下面看一个例子你就明白了
```js
window.color = "red";
var o = {
    color: "blue"
};
function getColor() {
    console.log(this.color);
}
getColor();            // red
getColor.call(this);   // red
getColor.call(window); // red
getColor.call(o);      // blue
```
这里你是否明白了呢，`getColor`本来是全局作用域，本应该是输出`red`，绑定`o`后，竟然可以输出对象的变量。这样做的最大好处是函数既访问到了对象的变量，而且和对象没有形成耦合关系。
最后说一个 ES 方法`bind()`。下面看一下代码
```js
window.color = "red";
var o = {
    color: "blue"
};
function getColor() {
    console.log(this.color);
}
var oGetColor = getColor.bind(o);
oGetColor(); // blue
```
这个方法比`call()`更直接，直接实现函数对象绑定的值，并给一个新的函数。这个就牛逼了，具有`call()`的好处,并且更加直接，好理解。