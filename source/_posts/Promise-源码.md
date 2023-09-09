---
title: Promise-源码实现
date: 2023-09-09 15:33:45
tags: Promise
categories: Promise
cover: https://pic.imgdb.cn/item/64fc1fa5661c6c8e54ee0578.png
---

## Promise 一共有三个状态

Promise 有 `Pending`、`Fulfilled`、`Rejected`三种状态，其中初始状态值为`Pending`。

```JavaScript
const PENDING = "PENDING",
  FULFILLED = "FULFILLED",
  REJECTED = "REJECTED";
```

## Promise 初始化的时候都做了什么

当我们去实例化一个`Promise`的时候，需要去传入`executor`函数。这个函数接收两个参数`resolve`和`reject`。`resolve`函数的作用是将`Promise`的状态由`Pending`转为`Fulfilled`。相反，`reject`函数的作用是将`Promise`的状态由`Pending`转为`Rejected`。

```JavaScript
const PENDING = "PENDING",
  FULFILLED = "FULFILLED",
  REJECTED = "REJECTED";

class MyPromise {
  constructor(executor) {
    this.status = PENDING;
    this.value = undefined;
    this.reason = undefined;

    const resolve = (value) => {
      if (this.status === PENDING) {
        this.status = FULFILLED;
        this.value = value;
      }
    };

    const reject = (reason) => {
      if (this.status === PENDING) {
        this.status = REJECTED;
        this.reason = reason;
      }
    };

    try {
      executor(resolve, reject);
    } catch (e) {
      reject(e);
    }
  }
}
```


