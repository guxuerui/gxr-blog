---
title: JS中扩展运算符(...)的应用
date: 2020-07-22 15:42:11
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---

## 含义
扩展运算符(...)好比rest参数的逆运算，将一个数组转化为用空格分隔的参数序列。
```JavaScript
  console.log(...[1,2,3]) // 1 2 3
  console.log(1,...[2,3,4,5],6) // 1 2 3 4 5 6
```
1. 该运算符主要用于函数调用
```JavaScript
  function push (array, ...items) {
    array.push(...items)
  }
  function add (x,y) {
    return x + y
  }
  const numbers= [1, 38]
  add(...numbers) // 39
```
上面代码中，array.push(…items)和add(…numbers)这两行，都是函数的调用，它们的都使用了扩展运算符。该运算符将一个数组，变为参数序列。

2. 扩展运算符可以和正常的函数参数结合使用，非常灵活
```JavaScript
  function f (v, w, x, y, z) {}
  const args = [0, 1]
  f(-1, ...args, 2, ...[3])
```

3. 扩展运算符后面还可以放置表达式
```JavaScript
    const arr = [
      ...(x > 0 ? ['a'] : []),
      'b',
    ];
```

4. 如果扩展运算符后面是一个空数组，则不产生任何效果
```JavaScript
   [...[],1] // [1]
```

**PS: 只有当函数调用时，扩展运算符才可以放在圆括号中，否则就会报错**
```JavaScript
  (...[1, 2])
  // 直接这样运行会报错，Uncaught SyntaxError: Unexpected number
  
  console.log((...[1, 2]))
  // Uncaught SyntaxError: Unexpected number
  
  console.log(...[1, 2])
  // 1 2
```

## 替代函数的apply方法

1. 由于扩展运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了。
```JavaScript
   // ES5 的写法
  function f(x, y, z) {
    // ...
  }
  var args = [0, 1, 2];
  f.apply(null, args);
  
  // ES6的写法
  function f(x, y, z) {
    // ...
  }
  let args = [0, 1, 2];
  f(...args);
```
