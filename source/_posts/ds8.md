---
title: 《数据结构》笔记：词典
date: 2015-09-15 10:52:04
categories: 编程基础
tags: 数据结构
---
### 散列(Hash)
> 新的访问方式   

```
寻秩访问：向量
寻位置：列表
寻关键码：BST
```
- 访问方式：寻值访问
- 电话

### 原理
- 桶Bucket：直接存放或者间接指向一个词条
- 桶数组Bucket array/散列表 Hash Table 容量为M

```
N < M << R 空间 = O(N + M) = O(n)
```
- 定址/杂凑/散列：根据Key直接确定散列表入口
- 散列函数：hash() : key -> & entry

```
hash(key) = key % M

25K / 90K > 25% 节约了较大空间
装填因子 N / M 

6278 5001                54304          39514
6277 0211    % 90001     39514          51304
5154 1876                51304          54304
 
```
- 冲突：不同关键码，被映射到同一实体上

```
5153 1976    % 90001     51304
6278 2001
理论上无法彻底避免，只能根据策略减少冲突
hash 不可能是单射
```

- 散列策略：近似的单射，往往可行
- - 精心设计散列表和散列函数，尽可能降低冲突的概率
- - 制定可行的预案，以便排解

### 散列函数
- 评价标准：什么样hash()更好
- - 确定determinism：同一关键码总是被映射至同一地址
- - 快速efficency：expected-O(1)
- - 满射surjection:尽可能充分的覆盖整个散列空间
- - 均匀uniformity：关键码映射到散列表各位置的概率尽量接近可有效避免聚集clustering现象
- 除余法

```
hash(key) = key % M
```
- - 表长为素数时，分布更均匀，更全面
- - gcd(S, M) = G ，M取素数
- - 缺陷：不动点hash(0) = 0，零阶均匀，相邻关键码散列地址必相邻
- MAD法：

```
M为素数，a > 0, b > 0, a % M !=0
hash( key ) = ( a * key + b ) % M
```
- 数字分析法
- 平方取中法：平方后取中间数

```
hash( key ) = key^2 取中间位数
```
- 折叠法：将key分隔为等宽的若干端，取其总和作为地址
- 位异或法。。。。
- 总结：越是随机越是没有规律越好
- 伪随机数法：
```
随机数发生器
循环 rand( x + 1 ) = a * rand( x ) % M
x = time()
伪随机数法
hash( key ) = rand( key ) = [ rand(0) * a^key ] % M
种子 rand(0)
```
- 多项式法：

```
hash( s = X0X1X2...Xn-1) = X0a^n-1 + X1a^n-2+... Xn-1
```
### 排解冲突