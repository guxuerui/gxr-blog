---
title: 关于JS中的this
date: 2020-07-09 14:01:01
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
在日常开发中，我们总是会用到this。那么this到底是什么东东，如何正确判断this呢？

## 非箭头函数中的this

举个栗子:

```JavaScript
  function foo () {
    console.log(this.a);
  }
  var a = 1;
  foo();

  const obj = {
    a: 2,
    foo: foo
  }
  obj.foo();

  const c = new foo();
```
分析:
1. 对于直接调用foo函数来说，不管foo函数被放在了什么地方，this一定是指向window的。
2. 对于obj.foo()来说，只需要记住，谁调用了函数，this就指向谁，所以这时foo函数中的this就是指向obj对象。
3. 对于new的方式来说，this被永远绑定在了 c 上面，也就是new出来的对象，且不会再被任何方式改变this的指向。

