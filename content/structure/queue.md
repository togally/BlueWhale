+++
title = '队列简介'
tags = ['队列']
series = ["数据结构"]
categories = ['编程开发']
series_order = 2.1
date = 2024-09-11T23:06:12+08:00
draft = false
showComments = false
+++
# 队列
## 定义
队列是一种[特殊的线性表](),他的特殊性在于将线性表的操作限定为[表尾插入，表头删除]()

通过这个特殊的限定我们可以达到[FIFO(first in first out)]()的目的


## 抽象数据类型
[Queue.java](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/queue/Queue.java)

## 队列的数组实现（循环队列）
[CircleQueue.java](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/queue/CircleQueue.java)

### 队列数据存储的问题
虽然我们采用数组来作为数据存储的基本结构，但是由于队列的使用过程中头尾指针是动态的。

所以我们很容一造成头指针到index = 0 之间的空间浪费
![pop](structure/circleQueue_01.png)

因此我们采用 尾指针 = (尾指针 + 1) % data.length的方式来移动尾指针，让尾指针可以循环起来

这样就解决了存储空间浪费的问题

### 尾指针移动的几个问题，
[CircleQueue.java#rearMove](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/queue/CircleQueue.java)

按照正常情况下，队列临界的问题判断应该如下
```
# 队列满
this.font == this.rear

# 队列空
this.font == this.rear
```
我们无法区分队列满 和 队列空，需要引入一个新的变量flag来判断。

我们这里可以使用空出最后一个存储空间的方式来达到该目的，而不用引入新的状态
```
this.rear = ++this.rear % data.length;
if (this.rear == this.front){
    throw new RuntimeException("rear is out of size");
}
```

## 队列的链表实现实现
[LinkedQueue.java](https://github.com/togally/bookLearning/blob/master/src/main/java/com/togally/structure/queue/LinkedQueue.java)