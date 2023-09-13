---
title: 面试题-Vue-双向数据绑定的原理
date: 2023-09-13 13:20:06
tags: [面试题, Vue]
categoryies: Vue
cover: 
---

## Vue 的基本原理
当一个`Vue`实例创建时，`Vue`会遍历`data`中的属性，用`Object,defineProperty`将它们转为`getter/setter`,并且在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都有相应的`watcher`程序实例，它会在组件渲染的过程中把属性记为依赖，之后当依赖项的`setter`被调用时，会通知`watcher`重新计算，从而致使它关联的组件得以更新。

