---
title: undefined和null到底有什么区别呢
date: 2020-06-29 10:43:54
tags: 
  - JavaScript
  - 前端
categories:
  - Web前端
---
undefined和null都是在前端中经常会遇到的，有时候却不能立刻说上来它们的区别，了解过后也总是容易忘。那么，它们之间到底有什么区别呢？

## 基本数据类型
首先，在JavaScript中有7中基本数据类型(还有一种对象类型: Object)，分别是
1. string
2. number
3. null
4. undefined
5. boolean
6. symbol
7. bigint

它们是属于虚值，可以使用Boolean(value)或!!value将其转换为布尔值，值为false。例如：
```JavaScript
  console.log(!!null); // false
  console.log(!!undefined); // false
  console.log(Boolean(null)); // false
  console.log(Boolean(undefined)); // false  
```
## undefined
undefined表示”缺少值“，就是此处应该有一个值，但是还没有定义。例如：
1. 变量声明了，但是还没有赋值，此时就是undefined
2. 调用函数时，应该传的参数没有传，该参数就是undefined
3. 对象没有赋值的属性，该属性的值就是undefined
4. 函数没有返回值时，默认返回undefined
```JavaScript
  let foo;
  foo; // undefined

  function f(o) {console.log(o)};
  f(); // undefined

  let foo = new Object();
  foo.g; // undefined

  let foo = f();
  foo; // undefined
```

## null
null表示”没有对象“，即一个值被定义了，但是被定义为了空值。例如；
1. 作为函数的参数，表示该参数不是对象
2. 作为对象原型链的终点
```JavaScript
  Object.getPrototype(Object.prototype); // null
```

## 判断相等
在比较undefined和null是否相等时，使用==得到true，使用===得到false。
```JavaScript
  console.log(null == undefined); // true
  console.log(null === undefined); // false
```

## 总结
1. undefined表示”缺少值“，就是此处应该有一个值，但是还没有定义
2. null表示”没有对象“，即一个值被定义了，但是被定义为了空值
