---
title: 《JavaScript高级程序设计》读后记<十一>：跨域
date: 2015-12-15 22:31:11
categories: JavaScript
tags: JavaScript
---
终于到了收官之作了，学了这么长时间的 JavaScript ，对这门语言也有一个全面的认识。虽然你没有 C 系列经典健全，没有 Java 系列流行应用广泛，也许也没有 PHP 广受追捧。甚至天生的不完美给开发者带来不少麻烦，各个浏览器不同的支持兼容性也很让人头疼。但是随着前端的发展，作为一门脚本语言，你变化越来越大也越来越完善，同时好的工具也层出不穷，各种开源技术遍地开花。因此我始终相信你会成为一门理想的编程语言，让前端这个领域始终朝气蓬勃。

### 同源策略
什么是跨域？为什么跨域？这一切都要从它谈起：__同源策略__。
同源策略：同协议，同域，同端口
如果三个条件满足就是同源，两个不同源的站点是无法获取数据的。这是浏览器的一种策略。目的是为了保护用户信息安全，防止窃取数据。
具体是以下行为被禁止
- Cookie 和 LocalStroage ，indexDB 无法读取
- Dom 无法获得
- Ajax 请求无法获得
下面我们具体了解下 Ajax

### AJAX
全称 Asynchronous JavaScript + XML，这个技术能够向服务器请求额外的数据而无需卸载页面，简单的说就是无刷新技术，不需要刷新页面就可以获得新数据并显示在页面上。
这个技术的核心是`XMLHttpRequest`对象
#### XMLHttpRequest
直接上代码吧
1.创建 XHR 对象
```js
// 创建XHR对象
function createXHR() {
    if (typeof XMLHttpRequest != "undefined") {
        // IE7 以上版本
        return new XMLHttpRequest();
    } else if (typeof ActiveXObject != "undefined") {
        // IE7 之前的版本
        if (typeof arguments.callee.activeXString != "string") {
        var versions = ["MSXML2.XMLHttp6.0", "MSXML2.XMLHTTP.3.0", "MSXML2.XMLHttp"];
        var 1, len;
        for (i = 0, len = versions.length; i < len; i++) {
            try {
                new ActiveXObject(versions[i]);
                arguments.callee.activeXString = versions[i];
                break;
            } catch (ex) {
                // 跳过
            }
        }
    }
    return new ActivaXObject(arguments.callee.activeXString);
    } else {
        throw new Error("no xhr object avaiable");
    }
}
```

2.使用 XHR 方法和属性
发送同步请求
```js
// 创建XHR对象
var xhr = createXHR();

// 启动请求(同步)
xhr.open("get", "login.php", false);

// 发送请求
xhr.send(null);

// 服务器收到请求后自动填充xhr对象的属性
if (xhr.status >= 200 && xhr status < 300 || xhr.status == 304) {
    console.log(xhr.responseText);
} else {
    console.log("request fail!" + xhr.status);
}
```
发送异步请求
```js
var xhr = createXHR(); // 0状态

xhr.onreadystatechange = function() {
    // 检测请求状态
    if (xhr.readyState == 4) {
        // 检测状态码，确认响应成功返回
        if (xhr.status >= 200 && xhr.status < 300 || xhr.status == 304) {
            console.log("xhr.responseText");
        } else {
            console.log("fail" + xhr.status);
        }
    }
}

// 1状态
xhr.open("get", "login.php", "true");

// 2状态
xhr.send(null)

// 使用xhr.about()可以取消异步请求
```
说明：
- responseText: 响应主体被返回的文本
- responseXML: 响应的XML DOM 文档
- status: 响应的HTTP状态，200成功，304资源未修改
- statusText: HTTP状态说明
- readyState: 0未初始-1启动-2发送-3接收-4完成

3.HTTP 头部信息
(1)发送 XHR 时还会发送相应的头部信息
- Accept: 浏览器能处理的内容
- Accept-Charset: 能处理的字符集
- Accept-Encoding: 压缩编码
- Accept-Language: 语言
- Connection: 连接类型
- Cookie: 设置的 Cookie
- Host: 所在的域
- Referer: 请求的 URL (Referrer拼写正确的单词)
- User-Agent: 用户代理字符串

(2)设置获取头部信息
```js
// send方法之前
xhr.setRequestHeader("User-Agent", "suo"); // 设置

xhr.getRequestHeader("User-Agent"); // 获取

xhr.getRequestHeader(); // 获取所有
```

(3)Get 请求
关键在于处理字符格式
```js
// 处理字符函数
function addURLParam(url, name, value) {
    url += (url.indexOf("?") == -1 ? "?" : "&");
    url += encodeURLComponent(name) + "=" + encodeURLComponent(value);
    return url;
}
// 处理
var url = "index.php";
url = addURLParam(url, "name", "suo");
url = addURLParam(url, "friend", "yue");
// 请求
xhr.open();
```

(4)Post 请求
模仿表单提交
```js
xhr.open("post", "login", true);
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
var form = document.getElementById("user-info");
xhr.send(serialize(form));
```

#### XMLHttpRequest2
1.FromData
2.超时设定
3.进度事件
4.progress 事件

#### 进度事件

### CORS
全称为Cross-Origin Resource Sharing, 跨域资源共享，W3C新出来的一个规范的跨域方法
在发送请求的时候添加一个 origin 头部，服务器根据头部信息决定是否响应
Origin: http://suosmile.cn
服务器判断是否要响应，可以接受就回发一个
Access-Control-Allow-Origin: http://suosmile.cn

#### IE 实现 CORS
IE8 中引入了 XDR 类型
- cookie 不会随请求发出，也不会响应返回
- 只能设置请求头部信息中的 Content-Type 字段
- 不能访问响应头部信息
- 只支持 GET 和 POST
- 默认异步

```js
// 创建XDR
var xdr = new XDomainRequest();
// 成功触发
xdr.onload = function() {
    console.log(xdr.responseText);
}
// 错误返回
xdr.onerror = function() {
    console.log("error");
}
// 超时
xdr.timeout = 1000;
xdr.ontimeout = function() {
    console.log("timeout");
}
// 启动
xdr.open("post", "http://suosmile.cn");
// 设置post格式
xdr.contentType = "application/x-www-form-urlencoded";
// 发送
xdr.send(null);
```

#### 其他浏览器实现
只需要把`XMLHttpRequest`对象中的`open`方法的`url`改写成绝对地址就可以了，但是会有一些限制
- 不能使用`setRequestHeader()`和`getAllResponseHeaders()`方法
- 不能发送和接收`cookie`

#### 跨浏览器实现

### 图片 Ping
一个网页可以从任何网页加载图片
```js
var img = new Image();
img.onload = img.onerror = function() {
    console.log("ok");
};
img.src = "http://suosmile.cn?name=suo"; // ok
```

### JSONP
全称Json with padding的简写，是应用JSON的一种新方法
```js
callback({"name":"suo"});
```
有两个部分组成，一个是回调函数和数据
通过动态操作脚本实现
```js
function handleResponse(response) {
    console.log(response.ip + "," + response.city + ',' + response.region_name);
}
var script = document.createElement("script");
script.src = "http://suosmile.cn?callback=handleResponse";
document.body.insertBefore(script, document.body.firstChild);
```

### Comet
一种高级的`Comet`，高级的`AJAX`

### 服务器发送
SSE 
### Web Sockets

### SSE

### XSS

