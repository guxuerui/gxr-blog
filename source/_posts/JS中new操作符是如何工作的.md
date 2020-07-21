---
title: JS中new操作符是如何工作的
date: 2020-07-21 10:29:34
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
关于JS中的new操作符，首先来看两个例子来了解下。

## new的作用
```JavaScript
  function Test(name) {
    this.name = name;
  }
  Test.prototype.sayName = function () {
    console.log(this.name);
  }
  const t = new Test('lufei');
  console.log(t.name); // lufei
  t.sayName(); // lufei
```
可以看出：
1. new通过构造函数Test创建出来的实例可以访问到构造函数中属性。
2. new通过构造函数Test创建出来的实例可以访问到构造函数原型链中的属性，也就是说通过new操作符，实例与构造函数通过原型链连接了起来。

但是当下的构造函数Test并没有显示的return任何值(默认返回undefined)。

1. 如果设置一个返回值会怎么样呢？
```JavaScript
  function Test (name) {
    this.name = name;
    return 1;
  }
  const t = new Test('lufei);
  console.log(t.name); // lufei
```
虽然构造函数Test中return了值1，但是这个返回值并没有任何的用处。此时可以得出结论：
**构造函数如果返回原始值，那么这个返回值毫无意义。**

2. 如果返回了对象
```JavaScript
  function Test(name){
    this.name = name
    console.log(this)//Test {name: 'lufei'}
    return {age: 26}
  }
  const t = new Test('lufei')
  console.log(t)//{age: 26}
  console.log(t.name)//undefined
```
可以发现，构造函数内部的this虽然工作正常，但是当返回值为对象时，这个返回值就会被正常的返回出去。此时可以得出结论：
**构造函数如果返回值为对象，那么这个返回值就会被正常使用。**

**PS: 通过这两个例子说明，在构造函数中尽量不要有返回值，因为返回原始值没有作用，返回对象则会导致new操作符失效。**

## 总结
new操作符的作用：
* new 操作符会返回一个对象
* 这个对象，也就是构造函数中的 this，可以访问到挂载在 this 上的任意属性
* 这个对象可以访问到构造函数原型上的属性
* 返回原始值会忽略，返回对象会正常处理

## 手动实现一个new
```JavaScript
  function create() {
    // 创建一个空的对象
    let obj = new Object()
    // 获得构造函数
    let Con = [].shift.call(arguments)
    // 链接到原型
    obj.__proto__ = Con.prototype
    // 绑定 this，执行构造函数
    let result = Con.apply(obj, arguments)
    // 确保 new 出来的是个对象
    return typeof result === 'object' ? result : obj
  }
```