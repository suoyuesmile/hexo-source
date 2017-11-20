---
title: 《数据结构》笔记：栈
date: 2015-08-20 12:22:23
categories: 编程基础
tags: 数据结构
---
> 一组元素的线性序列，通常只能访问一个元素  
> 开放的端top,不开放的bottom  
> push,pop只能操纵当前的顶部的元素  
### 接口与实现

- 操纵

操纵 | 功能
---|---
Stack() | 初始化栈
empty() | 判断栈是否为空
push(e) | 进栈
pop(e)  | 出栈
size()  | 返回元素数量

- 特性：后进先出(LIFO)
 
- 实现：通过向量或列表派生

```c
//向量实现
template <typename T> class Stack :public Vector<T> {
    public: //size() empty()直接沿用
        void push(T const & e) { insert(size(), e); } //入栈
        T pop() { return remove( size() -1 ); }       //出栈
        T & top() { return (*this)[ size() - 1]; }    //取顶 O(1)
};
//列表实现
```

### 进制转换(逆序输出)

```c
#include <stdio.h>
#include <iostream>
#include <stack>
using namespace std;
void convert( Stack<char> & S, __int64 n, int base) {
    static char digit[] = {'0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'};
    while (n > 0) {
        s.push( digit[ n % base ]);
        n /= base;
    }
}
int main(void) {
    Stack<char> S;
    cin >> n >> base;
    convert(S, n, base);
    while ( !S.empty() )
        cout << S.pop();
    return 0;
}
```

### 括号匹配（递归嵌套）

```c
#include <stdio.h>
#include <iostream>
#include <stack>
bool paren(const char exp[], int lo, int hi) {
    Stack<char> S;
    for (int i = lo; i < hi; i++)
        if( exp[i] == '(' )
            S.push(exp[i]);
        else if( !S.empty() )
            S.pop();
        else
            return false;
    return S.empty();
}
int main(void) {
    char exp[20] = { '(' };
    paren(exp, 0, 20);
    return 0;
}
//通过计数器也可以实现
//但是通过栈可以拓展到多个括号并存的实例
```

### 栈混洗
- 计数：cabula数 （2n!）/ (n!*(n+1)!)
- 甄别：n = 3 时 3 1 2 不是栈混洗，与元素无关，这是一个禁型，是充要条件

```
//高效算法，引入3个栈，模拟混洗过程，使用贪心算法
//练习
```
- 与括号匹配的联系

### 中缀表达式求值（延迟缓冲）

```c
float evaluate(char * s, char * & RPN) {
    Stack<float> opnd; 
    Stack<char> optr;
    optr.push('\0');
    while (!optr.empty()) {
        if (isdigit(*S))
            readNumber(S, opnd);
        else {
            switch( orderBetween(optr.top(), *S) ) {
                case '<':
                    optr.push(*S);
                    S++;
                    break;
                case '=':
                    optr.pop();
                    S++;
                    break;
                case '>': {
                    char op = optr.pop();
                    if ('!' == op)   //一元运算符
                        opnd.push( calcu(op, opnd.pop()) );
                    else {           //二元运算符
                        float pOpnd2 = opnd.pop(); //操作数
                        float pOpnd1 = opnd.pop();
                        opnd.push( calcu(pOpnd1, op, pOpnd2) ); //运算并回收
                    }
                    break;
                }
            }
        }
    }
    return opnd.pop();
}
```

### 逆波兰表达式（RPN）:不需要判断优先级，只要遇到操作符就计算，后缀表达式
- 变形：若欲取之，必先予之

```
1. 括号显示表示优先级
2. 将运算符移动对应的右括号之后
3. 抹去所有运算符
4. 稍加整理
```
- 实现：

```
//练习
```