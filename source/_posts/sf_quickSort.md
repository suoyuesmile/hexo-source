---
title: JavaScript 写常见算法之快速排序
date: 2016-06-22 20:33:23
categories: 算法
tags: [JavaScript, 算法]
---
之前学习数据结构的时候，用 C++ 的模板写过快速排序，而且还写了不少的变种。现在将用 JavaScipt 再重新复习一遍吧

### 快速排序原理
1.思想： __分而治之__
__选择一个数作为轴点，将整个序列分为左右两侧，左边不比轴点大，右边不比轴点小，对左右两边的子集迭代或递归直至缩减为最小规模。__

2.方法： __递归__
简单分为4 步
- 求平凡解
- 选取轴点
- 左右分组
- 递归连接

### JavaScipt 代码
代码如下
```js
// 原始序列
var originArr = [2, 4, 5, 7, 3, 8, 1, 10, 9, 6];

function quickSort(arr) {

    // 1.最小规模
    if (arr.length <= 1) return arr;

    // 2.确定轴点
    var povitIndex = Math.floor(arr.length / 2);
    var pivot = arr.splice(povitIndex, 1)[0];
    
    // 3.遍历集合分类
    var leftArr = [], rightArr = [];
    for (var i = 0; i < arr.length; i++) {
        if(arr[i] < pivot) {
            leftArr.push(arr[i]);
        } else {
            rightArr.push(arr[i]);
        }
    }

    // 4.递归子集并连接
    return quickSort(leftArr).concat(pivot, quickSort(rightArr));
}

// 测试
console.log(quickSort(originArr));
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

