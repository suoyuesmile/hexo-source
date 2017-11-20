---
title: 《JavaScript高级程序设计》读后记<九>：DOM
date: 2015-12-10 22:31:11
categories: JavaScript
tags: JavaScript
---
DOM 为 __Document Object Model__ 的缩写，也是 HTML 和 XML 文档的一个 API 接口。描绘的是一个层次化节点树，允许开发人人添加移除和修改页面的某一部分。

#### 节点层次
```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <p>hello world</p>
</body>
</html>
```
用一个图片描述一下

1.Node 类型

__属性__
(1)nodeType
JavaScipt中的所有节点类型都继承与Node类型。因此所有节点都共享着相同的基本属性和方法
每个节点都有个NodeType属性，用于表明节点的类型。节点类型由在Node类型中定义的下列12个数值常量来表示，任何节点类型必居其一
- Node.ElEMNRT_NODE
- Node.ATTRIBUTE_NODE
- Node.TEXT_NODE
- Node.CDATASELECTION_NODE
- Node.ENTITY_REFERENCE_NODE
- Node.ENYIYY_NODE
- Node.PROCESSING_INSTUCATION_NODE
- Node.COMMENT_NODE
- Node.DOCUMNENT_NODE
- Node.DOCUMENT_TYPE_NODE
- Node.DOCUMENT_FRAGMENT_NODE
- Node.NOTATION_NODE

通过比较上面这些常量很容易确定节点的类型
```js
if (someNode.nodeTyep == Node.ELEMENT) {
    alert("node is element type"); // ie无效
}
// 也可以直接与数字比较
```
(2)nodeName：保存标签名
(3)nodeValue：null
(4)childNodes:保存着NodeList对象，一组有序的序列
```js
var firstChild = someNode.childNodes[0];
var length = someNode.childNodes.length;
```
(5)parentNode:指向文档父节点
(6)nextSibling:下一个节点
(7)previouSibling:上一个节点
(8)ownerDocument:整个文档的文档节点

__方法__
(1)appendChild方法
在childNodes列表的末尾添加一个节点
```js
var returnedNode = someNode.appendChild(newNode);
```
(2)insertBefore方法
插入特定的位置
```js
// 插入到第一个
var aNode1 = someNode.appendChild(newNode, someNode.firstChild);
// 插入到倒数第二个
var aNode2 = someNode.appendChild(newNode, someNode.lastChild);
```
(3)replaceChild方法
```js
// 替换第一个
var aNode1 = someNode.replaceChild(newNode, someNode.firstChild); 
// 替换最后一个
var aNode2 = someNode.replaceChild(newNode, someNode.lastChild); 
```
(4)removeChild方法
```js
// 移除第一个
var aNode1 = someNode.removeChild(newNide, someNode.firstChild);
// 移除最后一个
var aNode2 = someNode.removeChild(newNide, someNode.laststChild);
```
2. Document 类型
表示文档，在浏览器中document对象是HTMLDocument的一个实例，表示整个HTML页面。document对象是window对象的一个属性，因此document作为全局对象来访问
- nodeType:9
- NodeName:#document
- nodeValue:null
- parentNode:null
- ownerDocument:null

(1)文档子节点
2个快捷访问方式，一个是documentElement属性，一个是childNodes访问文档元素
- `documentElement`属性，指向`<html>`
```js
var html = document.documentElement;
console.log(html === document.childNodes[0]);
console.log(html === document.firstChild);
```
- `body`属性，直接指向`<body>`
```js
var body = document.body;
```
所有浏览器都支持document.documentElement和document.body
- `doctype`属性，指向`<!DOCTYPE>`
```js
var docType = document.doctype;
```

(2)文档信息
- `title`属性，指向`<title>`
- `URL`属性
- `domain`属性
- `referrer`属性
```js
var aTitle = document.title;
// 完整URL
var aUrl = document.url;
// 域
var aDomain = document.domain;
// 来源URL
var aReferrer = document.referrer
```

(3)查找元素
- `getElementById`方法:取ID元素
```js
var oDiv = document.getElementById("mydiv"); // 早期版本不分大小写
```
- `getElementsByTagName`方法:取元素
```js
var lis = document.getElementsByTagName("li");
console.log(aLi[0].innerHTML);

// 全部
var all = document.getElementsByTagName("*");
```
- `getElementsByName`方法：取name所有元素
```js
var radio = document.getElementsByName("color");
```

(4)特殊集合
- document.anchors: 所有`<a>`
- document.applets: 所有带name的`<a>`
- document.forms: 所有`<form>`元素
- document.images: 所有`<img>`元素
- document.links: 所有带src的`<a>`元素

(5)DOM 一致性检测
DOM 有多个级别。实现DOM检测就有很大必要了,implementation,规定了一个方法hasFeature
```js
var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
```

(6)文档写入
- write():原样写入
- writeln():写入换行
- open()
- close()

3. Element 类型
- nodeType:1
- nodeName:标签名
- nodeValue:null
- parentNode:可是Document或者Element
- childNodes:都可能

(1)HTML元素
- id: 元素在文档唯一标识符
- title: 附加说明
- lang: 语言代码
- dir: 语言的方向
- className: 与class对应，之所以不用class是因为class是ES保留字

这些值都可以用来修改
```html
<a href="" id="suo" name="" title="" class="" lang="" dir="">name</a>
<!-- ie8之前不能访问 -->
```

(2)特性
- getAttribute()
- setAttrubute()
- removeAttribute()

(3)创建元素
document.createElement方法
```js
var div = document.createElement("div");
div.id = "suo";
div.className = "yue";
document.body.appendChild("div");

// 完整插入
var div = document.createElement("<div id=\"suo\" class=\"yue\"> </div>");
```

(4)元素的子节点
不同浏览器解析不一样
- IE不解析间隔
- 其他浏览器连间隔也解析

4.Text 类型
- nodeType: 3
- nodeName: "#text"
- nodeValue: 节点文本
- parentNode是一个Element
- 没有子节点
- appendData(text): 插入节点末尾
- deleteDate(offset, count): 删除文本
- insertData(offset, text): 插入文本
- replaceData(offset, count, text): 替换文本
- splitText(offset): 分割文本
- substringData(offset, count): 提取字符串

```html
<!-- 没有内容 -->
<div></div> 
<!-- 有个空格 -->
<div> </div>
<!-- 有内容 -->
<div>hello world</div>
```

```js
// 取得文本
var textNode = div.firstChild;
// 或者
var textNode = div.childNodes[0];
// 修改值
textNode.nodeValue = "suoyue";
```

(1)创建文本节点
```js
var textNode = document.createTextNode("<p>hello world!</p>");

// 分开创建
var element = document.createElement("p");
element.className = "color";
var textNode = document.createTextNode("hello world!");
element.appendChild(textNode);
document.body.appendChild(element);
```

(2)规范文本节点
```js
element.normalize(); // 将多个文本节点合并成一个文本节点
```

5.Comment 类型
注释在DOM里面用Comment类型表示
- nodeType: 8
- nodeName: "#comment"
- nodeValue: "注释的内容"
- parentNode 可能是Document或者Element
- 不支持子节点

```js
// 获得
var div = document.getElementById("mydiv");
var comment = div.firstChild;
console.log(comment);
// 添加
var comment = document.createComment("good job");
```
6.CDATASection 类型
- nodeType: 4
- nodeName: "#cdata-section"
- nodeValue: CDATA区域内容
- parentNode 可能是Document或者Element
- 不支持子节点

```js
var cdata = document.createCDateSection();
```

7.DocumentType 类型
仅FireFox,Safari,Oprea支持
- nodeType: 10
- nodeName: doctype的名称
- nodeValue: null
- parentNode: Document
- 不支持子节点

8.DocumentFragment 类型
轻量级文档，文档片段
- nodeType: 11
- nodeName: "#document-fragment"
- nodeValue: null
- parentNode: null
- 子节点可以为各种

```js
var fragment = document.createDocumentFrament();
va ul = document.getElementById("list");
var li = null;
for (var i = 0; i < 3; i++) {
    li = document.createElement("li");
    li.appendChild(document.createTextNode("Item" + (i+1)));
    fragment.appendChild(li);
}
ul.appendChild(fragment);
```

9.Attr 类型
- nodeType: 2
- nodeName: 特性的名称
- nodeValue: 特性的值
- parentNode: null
- HTML没有子节点
- XML里面子节点可以是TEXT或者EntityReference

#### DOM 操作技术
DOM操作比较简明，原本不会很麻烦，但是由于浏览器有个隐性的陷阱和不兼容的问题，JavaScript操作还比较麻烦
1.动态脚本
动态加载外部文件
```js
var script = document.createElement("script");
script.type = "text/javascript";
script.src = "suo.js";
document.body.appendChild(script);

// 使用函数封装一下
function loadScript($url) {
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = $url;
    document.body.appendChild(script);
}
```
2.动态样式
与动态脚本一样
3.操作表格
4.使用NodeList

#### DOM 选择符API
1.querySelector 方法
```js
// 接受选择符，返回与该模式匹配的第一个元素
var body = document.querySelector("body");
var myid = document.querySelector("#myid");
var myclass = document.querySelector(".myclass");
var img = document.querySelector("img.myclass");
```
2.querySelectorAll 方法
```js
// 接受选择符，返回的是一个NodeList实例
var nodeList = document.querySelector(".myclass");
```
3.machesSelector 方法
```js
// 接受选择符，返回True或者false
var hasCLass = document.matchesSelector("body.myclass");
```

#### 元素遍历
新增的一组属性，为了让IE的行为一致
- childElementCount: 返回子元素个数
- firstElementChild: 指向第一个子元素
- lastAElementChild: 指向最后一个子元素
- previousElementSibling: 指向后一个同辈元素
- nextElementSibling: 指向前一个子元素

#### HTML5
1.与类相关的扩充
- getElementsByClassName()
- classList 属性

2.焦点管理
- document.activeElement

3.HTMLDocument的变化
- readyState 属性
- 兼容模式
- head 属性

4.字符集属性
`document.charset = "UTF-8`

5.自定义属性类型
`data-`

6.插入标记
- innerHTML 属性
- outerHTML 属性
- insertAdjacentHTML 方法
- 内存与性能
- scrollIntoView方法

#### 扩展（了解）
1.文档模式
- IE5: 混杂模式
- IE7: IE7标准模式
- IE8: IE8标准模式
- IE9: IE9标准模式，ES5，CSS3，H5大部分功能

2.children 属性
3.contains 方法
4.插入文本
5.滚动

<!-- #### DOM2和DOM3 -->
