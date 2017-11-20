---
title: 前端工程化之路：初探 Webpack 打包工具
date: 2016-04-01 20:33:23
categories: 前端工具
tags: webpack
---
最近一段时间前端的工具越来越多，让人不禁眼花缭绕。其中也不乏好的工具，比如 Webpack 和 Gulp 两种不同风格的前端工具。感觉再不学都跟不上技术的节奏了。所以最近尝试的去学习了一些。收获也是颇丰。现在记下笔记，以便日后，查看。

### Webpack 作用
Webpack 被称为前端打包工具。将项目当作一个整体，通过给定主文件，使用 loaders 处理后生成浏览器可识别的 JavaScript 文件

### Webpack 原理
- 模块化，处理依赖关系
- CommonJs 标准
- 加载工具，集成在一起

### Webpack 安装
```sh
# 全局安装
npm install -g webpack
# 安装到项目目录
npm install --save-dev webpack
```

### Webpack 使用
1.建立npn环境
```sh
npm init
```

2.使用webpack工具
```sh
webpack {entry file} {destination file}
```

3.添加配置文件
```js
// webpack.config.js
module.exports = {
    entry: _dirname + "/app/main.js",
    output: {
        path: _dirname + "/public",
        filename: "bundle.js"
    }
}
```

4.添加调试工具
(1)安装
```sh
npm install --save-dev webpack-dev-server
```
(2)配置
```js
// webpack.config.js
module.exports = {
    devtools: "eval-source-map",
    entry: _dirname + "/app/main.js",
    output: {
        path: _dirname + "/public",
        filename: "bundle.js"
    }
    devServer: {
        contentBase: "./public",
        historyApiFallback: true,
        inline: true
    }
}
```
(3)开启服务器
```json
// package.json
"script": {
    "server": "webpack-dev-server --open"
}
```
(4)运行
```sh
npm run server
```

5.使用loaders



