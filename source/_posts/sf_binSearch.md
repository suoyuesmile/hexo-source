---
title: JavaScript 写常见算法之二分查找
date: 2016-06-23 20:33:23
categories: 算法
tags: [JavaScript, 算法]
---
学习数据结构时，写过很多二分查找，具体细节已经忘的差不多了。现在重新用 JavaScript 捡起来，这其实和我之前写的快排有异曲同工之妙。

### 二分查找原理
1.思想：__减而治之__
__同样先选择一个轴点，左边比轴点小，右边比轴点大。目标大于左边，则缩减整体规模为左边，反之缩减至右边规模。__

2.方法：__迭代__
简单分为 2 步
- 确定中间轴点
- 迭代深入，轴点替换换上下界

### JavaScript 代码
代码如下
```js
// 原始数组
var originArr = [1, 3, 4, 5, 7, 8, 10, 11, 13, 14];

function binSearch(arr, target) {
    var mi, // 轴点下标
        lo = 0, // 下界下标
        hi = arr.length; // 上界下标

    while (hi - lo >= 1) {
        // 取整处理
        mi = Math.floor((hi + lo) / 2);
        if (target < arr[mi]) {
            hi = mi;
        } else if (arr[mi] < target){
            lo = mi;
        } else {
            return mi;
        }
    }
    return -1;
}

// 测试
console.log(binSearch(originArr, 4));
// 2
```

