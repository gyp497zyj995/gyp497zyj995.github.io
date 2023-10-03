---
title: js-数组-reduce
date: 2023-10-02 16:50:47
tags: 数组方法
cover: https://pic.imgdb.cn/item/651aa979c458853aef10d72d.png
---

## 常见场景

### 1. 求对象数组中的和

```JavaScript
const objects = [{ x: 1 }, { x: 2 }, { x: 3 }];
const newArr = objects.reduce((acc, cur) => acc + cur.x, 0);
console.log(newArr); // 6
```

### 2. 将二维数组扁平化

```JavaScript
const flattend = [
  [1, 2],
  [3, 4],
  [5, 6],
];
let result = flattend.reduce((acc, cur) => acc.concat(cur), []);
console.log(result); // [1, 2, 3, 4, 5, 6];
```

### 3. 统计对象中值的出现次数

```JavaScript
const names = ["Alice", "Bob", "Tiff", "Bruce", "Alice", "Bruce"];
const allNames = names.reduce((acc, cur) => {
  if (acc[cur]) {
    acc[cur]++;
  } else {
    acc[cur] = 1;
  }
  return acc;
}, {});
console.log(allNames); // { Alice: 2, Bob: 1, Tiff: 1, Bruce: 2 }
```

也可以使用空值合并运算符`??`

```JavaScript
const names = ["Alice", "Bob", "Tiff", "Bruce", "Alice", "Bruce"];
const allNames = names.reduce((acc, cur) => {
  const count = acc[cur] ?? 0;
  return {
    ...acc,
    [cur]: count + 1,
  };
}, {});
console.log(allNames); // { Alice: 2, Bob: 1, Tiff: 1, Bruce: 2 }
```

### 4. 使用按属性对对象进行分组

```JavaScript
const people = [
  { name: "Alice", age: 21 },
  { name: "Max", age: 20 },
  { name: "Jane", age: 20 },
];

function groupBy(objectArray, property) {
  return objectArray.reduce((acc, cur) => {
    const key = cur[property];
    if (acc[key]) {
      acc[key].push(cur);
    } else {
      acc[key] = [cur];
    }
    return acc;
  }, {});
}
const groupedPeople = groupBy(people, "age");
console.log(groupedPeople);
// {
//   '20': [ { name: 'Max', age: 20 }, { name: 'Jane', age: 20 } ],
//   '21': [ { name: 'Alice', age: 21 } ]
// }
```

可以将里边的判断反着写

```JavaScript
const people = [
  { name: "Alice", age: 21 },
  { name: "Max", age: 20 },
  { name: "Jane", age: 20 },
];

function groupBy(objectArray, property) {
  return objectArray.reduce((acc, cur) => {
    const key = cur[property];
    if (!acc[key]) {
      acc[key] = [];
    }
    acc[key].push(cur);
    return acc;
  }, {});
}
const groupedPeople = groupBy(people, "age");
console.log(groupedPeople);
// {
//   '20': [ { name: 'Max', age: 20 }, { name: 'Jane', age: 20 } ],
//   '21': [ { name: 'Alice', age: 21 } ]
// }
```

使用空值合并运算符`??`

```JavaScript
const people = [
  { name: "Alice", age: 21 },
  { name: "Max", age: 20 },
  { name: "Jane", age: 20 },
];

function groupBy(objectArray, property) {
  return objectArray.reduce((acc, cur) => {
    const key = cur[property];
    const curGroup = acc[key] ?? [];

    return { ...acc, [key]: [...curGroup, cur] };
  }, {});
}
const groupedPeople = groupBy(people, "age");
console.log(groupedPeople);
// {
//   '20': [ { name: 'Max', age: 20 }, { name: 'Jane', age: 20 } ],
//   '21': [ { name: 'Alice', age: 21 } ]
// }
```

### 5. 数组去重

```JavaScript
const myArray = ["a", "b", "a", "b", "c", "e", "e", "c", "d", "d", "d", "d"];
const result = myArray.reduce((acc, cur) => {
    if(!acc.include(cur)){
        acc.push(cur)
    }
}, [])
```
