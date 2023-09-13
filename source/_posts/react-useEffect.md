---
title: react-useEffect的使用
date: 2023-09-13 10:17:58
tags: React-Hook
categories: React
cover: https://pic.imgdb.cn/item/65011c74661c6c8e544916d4.png
---

## 如何编写 useEffect

编写`useEffect`需要遵循三个原则

1. 声明`useEffect`。默认情况下，`useEffect`会在每次渲染后都会执行。
2. 指定`useEffect`依赖。大多数`useEffect`应该按需执行，而不是在每次渲染后都执行。
3. 必要时要添加清理函数。有时`useEffect`需要指定如何停止、撤销，或者清除它的效果。

### 声明 useEffect

首先引入`useEffect`这个 Hook,然后再组件顶部调用，并且传入需要在每次渲染时都需要执行的代码。

```JavaScript
import { useEffect } from "react";

export default function HooksEffect() {
  useEffect(() => {
    // 每次渲染之后都会打印 useEffect
    console.log('useEffect');
  });
}
```

当你的组件渲染时，`React`将更新屏幕，然后运行`useEffect`中的代码。`useEffect`会把这段`console.log('useEffect')`代码放到屏幕更新渲染后执行。

`useEffect`组件如何与外部系统同步。声明一个`<VideoPlayer>`组件。通过传递布尔值类型的`isPlaying`prop 来控制视频的播放暂停。

```JavaScript
function VideoPlayer({ src, isPlaying }){
  
}
```
