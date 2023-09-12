---
title: 初识 react-useState
date: 2023-09-08 16:38:33
tags: React-Hook
categories: React
cover: https://pic.imgdb.cn/item/64fae909661c6c8e54ae2279.png
description: useState的基本使用
keywords: useState的基本使用
comments: true
---

## useState 记忆组件的状态

组件通常需要根据交互更改屏幕显示的内容，组件输入表单应该更新输入字段。组件需要`记住`当前表单输入的值，再`React`中，这种组件特有记忆被称为`state`。

## 普通变量无法记忆

- 局部变量无法再多次渲染中持久保存。当`React`再次渲染这个组件时，不会考虑之前对局部变量的任何更改。
- 更改局部变量不会触发渲染。`React`没有意识到它需要使用新数据再次渲染组件。

## 如何使用新数据更新组件

1. 保留渲染的数据。
2. 触发`React`的重新渲染`render()`。

## 使用 useState

```JavaScript
import React, { useState } from "react";

export default function HooksState() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>点击+1</button>
      <h1>{count}</h1>
    </div>
  );
}
```

useState 是实际发生的情况如下：

1. 组件进行第一次渲染。因为`0`作为`count`的初始值传递给`useState`,返回`[0, setCount]`。`React`记住了`0`是最新的`state`值。
2. 更新`state`。当点击按钮时，触发了`setCount(count + 1)`。`count`是`0`,所以它是`setCount(1)`。这告诉`React`现在记住`count`是`1`并且触发渲染。
3. 组件进行第二次渲染。`React`仍然看到`useState(0)`,但是因为`React`记住了你将`index`设置为了`1`,它将返回`[1, setCount]`。
4. 以此类推。

`useState`的参数就是变量的初始值。在这个例子中，`count`的初始值就是`1`。在每次渲染组件的时候，`useState`都会给你返回一个包含两个值的数组

1. 第一个参数`state(count)`会保存上一次渲染的值。
2. 第二个参数`setState(setCount)`可以更新`state(count)`并且触发`React`重新渲染组件。

> `Hooks`以`use`开头的函数只能在组件的最顶层调用，不能在条件语句、循环语句或其他嵌套函数内调用`Hook`。
> `const [count, setCount]` 最好使用驼峰命名，因为按照约定命名更加容易理解。

## 传递函数作为 useState 的初始值

如果`useState`的初始值为函数，则这个函数将被视为初始化函数。它应该是纯函数，不应该接受任何参数。并且该函数必须返回一个任何类型的值。当初始化组件时，`React`将调用你的初始化函数，并将其返回值存储为初始状态。

```JavaScript
import React, { useState } from "react";

function createList() {
  let list = [];
  for (let i = 0; i <= 50; i++) {
    list.push("item" + i);
  }
  return list;
}

export default function HooksState() {
  const [list, setList] = useState(createList);
  return (
    <ul>
      {list.map((item) => (
        <li>{item}</li>
      ))}
    </ul>
  );
}

```

尽管`React`只在初始渲染时保存初始状态，后续渲染时将其忽略。但是在上边这个例子中，`createList()`的结果仅用于初始渲染，但你仍然在每次渲染时调用此函数。如果它创建大数组或者执行昂贵的计算，这可能会浪费资源。

为了避免出现每次渲染都去调用此函数，我们可以将`createList`作为初始化函数传递给`useState`

```JavaScript
function HooksState() {
  const [list, setList] = useState(createList);
  // ...
}
```
注意，现在我们传递的是`createList`函数本身，而不是`createList()`调用该函数的结果。如果将函数传递给`useState`,`React`仅在初始化期间调用它。

## state 是组件私有的状态

`state`是组件实例内部的状态。如果渲染同一个组件两次，每个组件实例都会完全隔离`state`。改变其中一个组件实例的状态，并不会影响到另一个组件实例中的状态。`state`完全私有于声明它的组件，父组件无法更改它的`state`。

## 本文总结

1. `state`变量是通过`useState`Hook 来声明的。
2. 调用`Hook`时，包括`useSate`,只能在组件的顶层去调用。
3. `useState`Hook 返回一对值：[当前值，更新当前值的函数]。
4. `state` 是组件私有的。如果在两个地方渲染它，每个组件实例都已自己一份独有的`state`,不会互相影响`state`。
5. `state`完全私有于声明它的组件，父组件无法更改它的`state`。
6. 组件实例中可以存在多个`state`。
