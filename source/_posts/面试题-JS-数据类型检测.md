---
title: JS-数据类型检测有哪些
date: 2023-09-13 12:51:28
tags: [面试题, 数据类型]
categories: 面试题
cover: https://pic.imgdb.cn/item/65180673c458853aef57e67e.png
---

## typeof

```JavaScript
console.log(typeof 2);               // number
console.log(typeof true);            // boolean
console.log(typeof 'str');           // string
console.log(typeof []);              // object
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object
```

> typeof 会将`Array`、`Object`、`null` 都会判断为`object`,其他的判断结果都是正确的

## instanceof

````JavaScript
// 判断基本数据类型
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false
console.log('str' instanceof String);                // false

// 判断引用数据类型
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```
> `instanceof`只能正确的判断引用数据类型，而不能判断基本数据类型。其内部运行机制是判断一个对象在其原型链中是否存在一个构造函数的`prototype`属性。
````

## constructor

```JavaScript
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true
```

> `constructor`有两个作用，一是判断数据的类型，二是对象实例通过`constructor`对象访问它的构造函数。

## Object.prototype.toString.call()

```JavaScript
var a = Object.prototype.toString;

console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
```

> `Object.prototype.toString().call()`使用`Object`对象的原型方法`toString`来判断数据类型
> 注意：同样是检测对象`obj`调用`toString`方法，`obj.toString()`的结果和`Object.prototype.toString.call(obj)`的结果不一样，这是为什么？这是因为不同对象类型调用`toString`方法时，根据原型链的知识，调用的是对应的重写之后的`toString`方法。（`function`类型返回内容为函数体的字符串，`Array`类型返回元素组成的字符串），而不会去调用`Object`上原型`toString`方法（返回对象的具体类型）。