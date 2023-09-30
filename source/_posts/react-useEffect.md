---
title: react-useEffect的使用
date: 2023-09-20 10:17:58
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
import React, { useState, useRef } from "react";

export default function UseEffectPage() {
  const [isPlaying, setIsPlaying] = useState(false);
  const [src] = useState(
    "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
  );
  return (
    <div>
      <div>
        <button onClick={() => setIsPlaying(!isPlaying)}>
          {isPlaying ? "暂停" : "播放"}
        </button>
      </div>
      <VideoPalyer src={src} isPlaying={isPlaying} />
    </div>
  );
}

function VideoPalyer({ src, isPlaying }) {
  const videoRef = useRef(null);
  // 渲染期间不能调用play()和pause()方法
  isPlaying ? videoRef.current.play() : videoRef.current.pause();
  return <video width={100} ref={videoRef} src={src}></video>;
}

```

上面代码运行起来会报错，因为它试图在渲染期间对`DOM`节点进行操作。在`React`中，JSX 的渲染操作必须是纯粹的，不能包括任何像渲染`DOM`的副作用。

当我们去调用`paly()`方法的时候，对应的`DOM`节点还不存在，如果`DOM`节点都不存在的话，我们是没有办法调用`play()`和`pause()`方法的。

解决办法就是使用`useEffect`这个`hook`

```JavaScript
import React, { useState, useEffect, useRef } from "react";

export default function UseEffectPage() {
  const [isPlaying, setIsPlaying] = useState(false);
  const [src] = useState(
    "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
  );
  return (
    <div>
      <div>
        <button onClick={() => setIsPlaying(!isPlaying)}>
          {isPlaying ? "暂停" : "播放"}
        </button>
      </div>

      <VideoPalyer src={src} isPlaying={isPlaying} />
    </div>
  );
}

function VideoPalyer({ src, isPlaying }) {
  const videoRef = useRef(null);
  useEffect(() => {
    isPlaying ? videoRef.current.play() : videoRef.current.pause();
  });
  return <video width={100} ref={videoRef} src={src}></video>;
}

```

当`VideoPalyer`组件渲染的时候（无论是否是初次渲染），`react`都会刷新屏幕，确保`video`元素已经被渲染到`DOM`中，然后才会执行`useEffect`中的代码，根据`playing`的状态来确定是调用`paly()`还是`pause()`方法

## 指定 useEffect 的依赖

一般来说，`useEffect`方法会在每次渲染的时候去执行，但是有的时候我们并不希望每次渲染的时候执行`useEffect`。

为了演示这个问题，我们在伏组件中添加一个`input`输入框，并且在`play()`和`pause()`方法中添加`console.log()`方法

```JavaScript
import React, { useState, useEffect, useRef } from "react";

export default function UseEffectPage() {
  const [isPlaying, setIsPlaying] = useState(false);
  const [val, setVal] = useState("");
  const [src] = useState(
    "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
  );
  return (
    <div>
      <div>
        <input
          type="text"
          value={val}
          onInput={(e) => setVal(e.target.value)}
        />
      </div>
      <div>
        <button onClick={() => setIsPlaying(!isPlaying)}>
          {isPlaying ? "暂停" : "播放"}
        </button>
      </div>

      <VideoPalyer src={src} isPlaying={isPlaying} />
    </div>
  );
}

function VideoPalyer({ src, isPlaying }) {
  const videoRef = useRef(null);
  useEffect(() => {
    if (isPlaying) {
      videoRef.current.play();
      console.log('调用了play()');
    } else {
      videoRef.current.pause();
      console.log('调用了pause()');
    }
  });
  return <video width={100} ref={videoRef} src={src}></video>;
}

```

在我们输入框中输入内容的时候，他会重新渲染父组件。因为`useEffect`中没有指定依赖项，所以它会一直调用`useEffect`中的代码。

如果我们给`useEffect`指定一个`[]`一个空的依赖项，拿它会给我们抛出一个错误，`React Hook useEffect has a missing dependency: 'isPlaying'`,这是因为我们在`useEffect`中依赖了`isPlaying`这`prop`属性，在依赖中却没有指定这个依赖。

```JavaScript
function VideoPalyer({ src, isPlaying }) {
  const videoRef = useRef(null);
  useEffect(() => {
    if (isPlaying) {
      videoRef.current.play();
      console.log('调用了play()');
    } else {
      videoRef.current.pause();
      console.log('调用了pause()');
    }
  }, [isPlaying]);
  return <video width={100} ref={videoRef} src={src}></video>;
}
```

当我们指定了`isPlaying`为依赖项之后，如果`isPlaying`与上次渲染的时候想等，就应该跳过`useEffect`的运行。那么在我们输入框输入内容的时候就不会导致`useEffect`重新运行，但是播放、暂停按钮还是会触发`useEffect`的运行。

依赖项数组中可以包含多个依赖项，当指定的所有依赖项和上次的渲染期间的依赖项完全相同的时候，`React`才不会重新`useEffect`。

依赖项不能随意的去指定，如果指定的依赖项和`useEffect`代码所期望的相匹配时，`lint`将会报错

### 没有指定任何依赖，会在每次重新渲染的时候执行

```JavaScript
useEffect(()=>{
})
```

### 指定一个空数组会在初次渲染的时候执行

```JavaScript
useEffect(()=>{
}, [])
```

### 只会在` a``b `的值与上次的值不同的时候才会执行

```JavaScript
useEffect(()=>{
}, [])
```

## 按需添加清理函数

```JavaScript
function ChatRoom() {
  useEffect(() => {
    const service = createService();
    service.connect()
  }, []);
  return <h1>欢迎来到聊天室</h1>;
}
```

想象`ChatRoom`是一个大型项目的一部分，当用户在切换到包含`ChatRoom`组件上就会连接服务器一次，当用户切换页面之后，在返回到包含`ChatRoom`组件当中，就会再次执行一次连接服务器一次。但是第一次创建的连接并未被销毁。当用户不断的切换页面，与服务器的连接就回不断堆积。

为了解决这个问题，我们可以在`useEffect`中返回一个清理函数。

```JavaScript
function ChatRoom() {
  useEffect(() => {
    const service = createService();
    service.connect()
    return () => {
        service.disConnect()
    }
  }, []);
  return <h1>欢迎来到聊天室</h1>;
}
```

加上清理函数之后，在我们每次重新执行`useEffect`之前，`React`都会执行清理函数；组件被卸载时，也会执行清理函数。

## 总结
1. 与事件不同，`useEffect`是由渲染本身，而非特定的交互引起的。
2. 默认情况下，`useEffect`在每次渲染（包括初始渲染）后运行。
3. 如果`React`的依赖项与上次渲染时的值相同，则跳过本次`useEffect`。
4. 如果`useEffect`因为重新挂载而中断，那么需要实现一个清理函数。
5. `React`将在下次`useEffect`运行之前以及卸载期间这两个时候调用清理函数。


