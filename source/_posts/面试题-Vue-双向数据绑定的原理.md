---
title: 面试题-Vue-双向数据绑定的原理
date: 2023-09-13 13:20:06
tags: [面试题, Vue]
categoryies: Vue
cover: https://pic.imgdb.cn/item/6518058bc458853aef57cd94.png
---

## Vue 的基本原理

当一个`Vue`实例创建时，`Vue`会遍历`data`中的属性，用`Object,defineProperty`将它们转为`getter/setter`,并且在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都有相应的`watcher`程序实例，它会在组件渲染的过程中把属性记为依赖，之后当依赖项的`setter`被调用时，会通知`watcher`重新计算，从而致使它关联的组件得以更新。

## 数据双向绑定的原理

`Vue`采用数据劫持结合发布者-订阅者模式的方式，通过`Object.defineProperty()`来劫持各个属性的`setter`、`getter`,在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下步骤：

1. 需要`observe`的数据对象进行递归遍历，包括子属性对象的属性,都加上`setter`和`gettter`这样的话，给这个对象的某个值赋值就会触发`setter`,那么就能监听到了数据的变化。
2. `compile`解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动收到通知更新视图。
3. `MVVM`作为数据绑定的入口，整合`Observer`、`Compile`、`Watcher`三者，通过`Observer`来监听自己的`model`数据变化，通过`Compile`来解析编译模板指令，最终利用`Watcher`搭起`Observer`和`Compile`之间的通信桥梁，达到数据变化 -> 视图更新;视图交互变化（input）-> 数据`model`变更的双向绑定效果。

## 使用 Object.defineProperty() 来劫持数据有什么缺点

在对一些属性进行操作时，使用这种方法无法拦截，比如通过下标方式修改数组数据或者给对象新增属性，这都不能触发组件的重新渲染。因为`Object.defineProperty`不能拦截到这些操作。更精确的来说，对于数组而言，大部分操作都是拦截不到，只是`Vue`内部通过重写函数的方式解决了这个问题。

## MVVM、MVC 的区别

### MVC

`MVC`通过分离`model`、`View`和`Controller`的方式来组织代码结构。`View`负责页面的显示逻辑，`Model`负责存储页面的业务数据，以及对相应的数据的操作。`View`和`Model`应用了观察者模式，当`Model`层发生改变的时候会通知`View`层更新页面。`Controller`层是`View`层和`Model`层的纽带，它主要负责与应用的相应操作，当用户与页面产生交互的时候，`Controller`中的事件触发器就开始工作了，通过调用`Model`层，来完成对`Model`的修改，然后`Model`层再去通知`View`层更新。

### MVVM

- `Model`代表数据模型，数据和业务逻辑都在`Model`层中定义。
- `View`代表视图，负责数据的展示。
- `ViewModel`负责监听`Model`中数据的改变并且控制视图的更新，处理用户的交互操作。
