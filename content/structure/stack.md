+++
title = '一文读懂栈'
tags = ['数据结构-栈']
series = ["重学数据结构"]
categories = ['编程开发']
series_order = 2
date = 2024-09-11T23:06:12+08:00
draft = false
showComments = false
+++
# 栈
## 定义
栈是一种[特殊的线性表](),只可以在[线性表的底部]()进行插入和删除操作.

线性表的头部被称为[栈底](),底部被成为[栈顶](),也就是说栈只可以在[栈顶]()进行插入或者删除操作

插入操作被称为[压栈(push)]()
![push](structure/stack_push.png)

删除操作被叫做[弹栈(pop)]().
![pop](structure/stack_pop.png)


## 抽象数据类型
### 数据
[AbstractStack](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/stack/ArrayStack.java)

### 行为
[IStack](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/stack/IStack.java)

[IShardedStack.java](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/stack/IShardedStack.java)

## 栈的种类

### 数组栈和链表栈
因为栈是特殊的线性表，所以很自然的就想到存在数组和链表两种实现方式。
个人觉得数组这种方式特别合适，因为我们限定了所有操作都要在栈顶，而链表方式会多一些额外开销。

### 共享栈
共享栈就是两个栈分享一个数组/节点链表，栈顶由单个栈的时候的数组最大值，变成了两个栈顶"发生碰撞"
![img.png](structure/stack_sharedStack.png)

## 栈的应用

### 递归
递归的典型运用场景就是[斐波那契数列](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/stack/Fibonaci.java)

斐波那契数列: 前两项数的和 f(0) = 0; f(1) = 1; f(n) = f(n-1) + f(n-2);

普通的执行方法如果我们要算f(20),那么要从 f(0) f(1) f(2)一步步计算到f(20);

使用递归的化我们是从f(20)开始分解公式,f(20) = f(19) + f(18);再去调用f(18) 和 f(19) 依次往下自己调用自己。

在这个依次向下的调用过程我们就需要 "先将上层的公式存起来" 先存起来的公式后算,这和栈结构不谋而合，而在java中执行方法的压栈和出栈也确实是这样的。

### 四则运算法则

我们常见的表达式 9+（3－1）×3+10÷2 叫做[中缀表达式]()，我们可以很轻松的依据优先级 “ 括号 大于 乘除 大于 加减” 来算出结果

但是计算机在处理中缀表达式的时候就很难办,于是 波兰的一位逻辑学家 想出来了一个[后缀(逆波兰)表达式](),
而上述中缀表达式转换后未 9 3 1 - 3 * + 10 2 / +

#### 中缀表达式转后缀表达式
中缀表达式提取后缀表达的基本步骤:
- 依次读取表达式中的每一项 符号和数字 ，读取到数字则放到后缀表达式中，读取到符号则放入计算符号栈中
- 如果栈顶 无符号 或者 优先级小于 当前要放入的符号,则直接放入
- 如果放入的符号 优先级大于当前符号则取出栈中的所有符号加入表达式 再放入符号
- 如果放入的符号是) 则取出符号直到(为止 加入表达式 然后再放入符号

#### 后缀表达式的计算逻辑
- 依次将每个表达式的元素放入到计算栈中
- 如果获取到计算符号则从栈中取出两个数进行计算，之后再放入到栈中
- 最后计算结束取出栈中数据即可
