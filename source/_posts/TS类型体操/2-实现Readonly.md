---
title: 2.实现Readonly
date: 2023-12-12 09:19:10
tags: [TS类型体操, TS]
categories: TS类型体操
cover: https://gitee.com/zyjgyp/imgs/raw/master/imgs/TS类型体操-实现Readonly.webp
---

### 概念

`Readonly<T>`是`TS`一个内置的泛型工具，它可以用于将给定类型`T`的所有属性变为只读。这可以用于创建新的类型，其中对象的属性不能被修改。`Readonly<T>`是`TS`提供的一种方便的方式，无需手动为每个属性添加`readonly`修饰符。

```JavaScript
interface IPerson {
    name: string;
    age: number;
}

const person: Readonly<IPerson> = {
    name: "John",
    age: 25
}

// 下面两行代码会导致编译错误，因为 person 是只读的
// person.name = "Tom"
// person.age = 18
```

在上面这个例子中，`Readonly<IPerson>`将`IPerson`接口的所有属性变为只读，创建了一个新的只读类型。这确保了`person`对象的属性在创建后不可被修改。

### 实现 Readonly

```JavaScript
type myReadonly<T> = {
    readonly [P in keyof T]: T[P]
}

interface IT {
    name: string;
    age: number;
    gender: boolean;
}

type GT = myReadonly<IT>
```
