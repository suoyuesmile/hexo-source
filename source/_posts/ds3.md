---
title: 《数据结构》笔记：列表
date: 2015-08-16 11:38:34
categories: 编程基础
tags: 数据结构
---
### 接口与实现
> 列表是采用**动态存储**策略的典型结构，其中的元素称作节点(node)
> 各节点通过指针或者引用彼此联结，在**逻辑**上构成一个线性序列         
> 属于双向链表结构

- 从静态到动态操作
- 1）静态：get O(1), search O(logn)
- 2）动态：insert O(n), remove O(n)
- 前驱后继：彼此相邻的节点，前驱或后继若存在则必然唯一
- 首末节点：没有前驱后后继的唯一节点
- 寻位置访问：called-by-position,通过节点的相互引用找到特定的节点
- 实现接口：列表节点作为基本单位  
操作 | 功能
---|---
pred() | 取节点前驱
succ() | 取节点后继
data() | 取节点数据对象
insertAsPred(e) | 插入前驱节点，存入e，返回新节点位置
insertAsSucc(e) | 插入后继节点，存入e，返回新节点位置

```c
#define Post(T) ListNode<T>*
template <typename T> struct ListNode {  //完全开放，不再过度封装
    T data;   //数值
    Post(T) pred;  //前驱
    Post(T) succ;  //后继
    ListNode() {}  //针对header和trailer的构造
    ListNode(T e, Posi(T) p = NULL, Post(T) s = NULL) : data(e), pred(p), succ(s) {} //默认构造
    Posi(T) insertAsPred(T const & e); //前插入
    Posi(T) insertAsSucc(T const & e); //后插入
}
```
- 操作接口
- 模板类

```c
#include "ListNode.h"
template <typename T> class List {
private:
    int _size;
    Posi(T) header;  //头元素（不可见）
    Posi(T) tailer;  //末元素（不可见）
protected:
    //内部函数
public:
    //构造函数
    //析构函数
    //只读接口
    //可写接口
    //遍历接口
};
//构造函数 规模为0
template <typename T> void List<T>::init() {
    header = new ListNode<T>;
    trailer = new ListNode<T>;
    header->succ = trailer;
    header->pred = NULL;
    tailer->pred = header;
    tailer->succ = NULL;
    _size = 0;
}
```
### 无序列表
- 秩到位置：模仿向量的循秩访问方式，重载下标符
- 查找

```c
template <typename T> Posi(T) List<T>::find(T const &e, int n, Posi(n) p) const {
    while (0 < n--) 
        if (e == ( p = p->pred )->data )
            return p;
    return NULL;
}
```
- 插入
 
```c
template <typename T> Posi(T) List<T>::insertBefore(Posi(T) p, T const & e) {
    _size++;
    return p->insertAsPred(e);  //e当作前驱插入
}
template <typename T> Posi(T) ListNode<T>::insertAsPred(T const & e) {
    Posi(T) x = new ListNode(e, pred, this); //创建
    pred->succ = x;                          //建立连接
    pred = x;
    return x;
}
```
- 基于复制的构造

```c
//基于复制的构造
template <typename T> void List<T>::copyNodes(Posi(T) p, int n) {  //O(n)
    init();  //创建头尾节点初始化
    while (n--) {  //自p的n下依次作为末节点插入
        insertAsLast(p->data);
        p = p->succ;
    }
}
```
- 删除节点

```c
//删除
template <typename T> T List<T>::remove(Posi(T) p) {  //O(1)
    T e = p->data;     //备份待删除的数值
    p->pred->succ = p->succ; //跳过p
    p->succ->pred = p->pred; //对称
    delete p;
    _size--;
    return e;          //返回数值
}
```
- 析构

```c
//析构
template <typename T> List<T>::~List() {
    clear();          //清空列表
    delete header;    //释放头
    delete tailer;    //释放末
}
template <typename T> int List<T>::clear() {
    int oldSize = _size;
    while(0 < _size)
        remove(header->succ); //反复删除首节点
    return oldSize;
}  //O(n)
```
- 唯一化

```c
//唯一化
template <typename T> int List<T>::deduplicate() {
    if (_size < 2)    //平凡列表自然无重复
        return 0;     
    int oldSize = _size;  //记录原规模
    Posi(T) p = first();
    Rank r = 1;           //p从首节点开始
    while ( trailer != ( p = p->succ ) ) {  //依次到末节点
        Posi(T) q = find(p->data, r, p);    //从前驱中查找雷同的
        q ? remove(q) : r++;                //若存在删除，否则秩递增
    }
    return oldSize - _size;                 //列表规模变化，等于删除的元素
}
```
### 有序列表
- 唯一化

```c
//有序向量唯一化
template <typename T> int List<T>::uniquify() { 
    if(_size < 2)
        return 0;
    int ordSize = _size;   //原始规模
    ListNodePosi(T) p = first(); 
    ListNodePosi(T) q;
    while ( trailer != ( q = p->succ ) ) //遍历
        if ( p->data != q->data )  //不同
            p = q;  //连接
        else
            remove(q); //雷同删除
    return oldSize - _size;
} //O(n)
```
- 查找

```c
//查找
template <typename T> Posi(T) List<T>::search(T const &e, int n, Posi(T) p) const {
    while ( 0 <= n-- )  //从右往左扫描，发现下于即命中
        if ( ( (  p = p->pred )->data ) <= e ) break;
    return p;
}
```
### 链表
- 单链表：节点只包含值域和指针域两个部分

```c
#define posi(T) sList<T>*
Template <typename T> struct sList {
    T data;
    posi(T) next;
};

//特点：特定元素之后插入，删除，复杂度为O(1)
//尾节点的指针为空 tail->next = NULL;
```
- - 
- 双向链表（列表）
- 循环单链表：尾指针指向了首元素

```c
tail->next = head;
//特点：特点元素前后插入，删除复杂度O(1)
```

- 循环双链表：兼具双向链表和循环链表优势

```c
tail->succ = head;
head->pred = tail;
//优势：在尾部插入删除元素的复杂度为O(1)
```


### 选择排序

```c
template <typename T> void List<T>::selectionSort(Posi(T) p, int n) {
    Posi(T) head = p->pred;   //待排区间
    Posi(T) tail = p;
    for (int i = 0; i < n; ++i)
    {
        tail = tail->succ;   
    }
    while (1 < n) {          //反复从待排区间找出最大的移至有序区间前端
        insertBefore( tail, remove( selectMax(head->succ), n) );
        tail = tail->pred;
        n--;
    }
}
//尽可能少的使用new delete
template <typename T> Posi(T) List<T>::selectMax(Posi(T) p, int n) {
    Posi(T) max = p;
    for (Posi(T) cur = p; 1 < n; n--) {
        if ( !lt ( (cur = cur->succ )->data, max->data) )
            max = cur;
    return max;   //返回最大节点位置
    }
}
//O(n^2)  但是移动操作远远少于起泡排序
//改进比较操作后可以在nlogn下完成
```

### 插入排序

### LightHouse

