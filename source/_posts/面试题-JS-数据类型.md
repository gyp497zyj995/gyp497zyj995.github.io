---
title: 面试题-JS-数据类型
date: 2023-09-12 22:07:41
tags: [数据类型, 面试题]
categories: 面试题
cover: https://pic.imgdb.cn/item/65007167661c6c8e5410336c.png
---

`JavaScript`分为`基本数据类型`和`引用数据类型`

- 基本数据类型：Number、String、Boolean、Null、Undefined、BigInt、Symbol
- 引用数据类型：Object、Function、Array

两种类型的存储位置不同

- 基本数据类型存储在栈内存中，栈内存的占据空间小，大小固定，属于被频繁使用的数据，所以放入栈中存储。

- 引用数据类型存储在堆内存中，占据空间大，大小不固定。如果存储在栈中，会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。




