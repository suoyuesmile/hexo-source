---
title: 前端工程化之路：初探 Gulp流式构建工具
date: 2016-06-10 20:33:23
categories: 前端工具
tags: gulp
---
用了一段事件的 webpack，感觉挺好用的，现在也试一下 gulp，这个工具上手起来其实也很快的。虽然它和 webpack 的原理相差很大，但是都能出色的完成任务。所以写一篇入门博客供以后参考。

### 什么是 gulp
流式自动化构建工具，简单的说就是，把输入的某些东西，经过某个管道处理后，输出需要的形式。

### 为什么要用 gulp
简单来说，就是减少我们一个一个构建的时间，使用这个工具自动化构建，如：scss 转化成 css, ES6 转化成 ES5, 等等，只要装了插件都能完成，最后就是把这些功能都写在一个函数里面，一起处理。

#### gulp 安装
和其他 npm 一样，直接输入命令
全局安装
```sh
npm install --global gulp
```
工程安装
```js
npm install --save-dev gulp
```

#### gulp 使用

__1.在工程目录里新建文件 gulpfile, 然后引入gulp__
```js
var gulp = require('gulp');
```

__2.gulp 四个核心方法__
- task() : 执行的任务
- src()  : 输入的文件
- pipe() : 执行的管道方法，接在源后面或者其他管道后面
- dest() : 输出的位置

```js
var gulp = require('gulp');

// 第一个参数为任务名（默认为default)， 第一个是任务内容
gulp.task('default', function() {
    gulp.src('../source/*.js')
    .pipe(gulp.dest('../dest'));
});
```

__3.gulp 插件使用__

安装压缩插件
```sh
npm install --global gulp-uglify
```

使用插件
```js
var gulp = require('gulp');
var uglify = require('gulp-uglify');

gulp.task('compess', function() {
    gulp.src('../source/js/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('../dest/js'))
});
```

执行任务
```sh
gulp compress
```

#### gulp 实践
根据需要搜索想要的插件并安装[gulp文档](http://www.gulpjs.com.cn/docs/)与[gulp插件](https://gulpjs.com/plugins/)
任务列表：

| 任务 | 插件 |
|---|---|
|检测js| gulp-jshint|
|scss => css| gulp-sass |
|jsx => js | gulp-react |
|es6 => es5| gulp-babel |
|文件拷贝|  gulp-copy |
|文件合并| gulp-concat |
|压缩js| gulp-uglify |
|压缩css| gulp-cssmin |
|压缩html| gulp-htmlmin|
|压缩img| gulp-imagemin|

安装各种插件
```sh
npm insatll --global gulp-sass 
...
```

写构建任务
```js
var gulp = require('gulp');
var sass = require('gulp-sass');
var jsinit = require('gulp-jsinit');
// ...
gulp.task('all', function() {
    gulp.src('../source/js')
        .pipe(react())
        .pipe(babel())
        .pipe(concat())
        .pipe(uglify())
        .pipe(gulp('../dest/js'));
    gulp.src('../source/css')
        .pipe(sass())
        .pipe(concat())
        .pipe(cssmin())
        .pipe('../dest/css');
    //...
});
```

执行任务
```sh
gulp all
```




