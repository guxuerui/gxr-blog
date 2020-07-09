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
全局环境中，this指向window。

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

  // 将一个对象作为call和apply的第一个参数，this会被绑定到这个对象。
  var obj = {a: 'Custom'};

  // 这个属性是在global对象定义的。
  var a = 'Global';

  function whatsThis(arg) {
    return this.a;  // this的值取决于函数的调用方式
  }

  whatsThis();          // 'Global'
  whatsThis.call(obj);  // 'Custom'
  whatsThis.apply(obj); // 'Custom'
```
分析:
1. 对于直接调用foo函数来说，不管foo函数被放在了什么地方，this一定是指向window的。
2. 对于obj.foo()来说，只需要记住，谁调用了函数，this就指向谁，所以这时foo函数中的this就是指向obj对象。
3. 对于new的方式来说，this被永远绑定在了 c 上面，也就是new出来的对象，且不会再被任何方式改变this的指向。
4. 将一个对象作为call和apply的第一个参数，this会被绑定到这个对象。

## 箭头函数中的this
```JavaScript
  function a () {
    return () => {
      return () => {
        console.log(this);
      }
    }
  }
  console.log(a()());
```
分析:
首先箭头函数其实是没有this的，箭头函数中的this只取决于包裹箭头函数的第一个普通函数的this。在上栗中，因为包裹箭头函数的第一个普通函数是a，所以this的指向就是window，并且对箭头函数使用bind这类函数是无效的。

## 使用bind函数时的this

this取决于bind的第一个参数，如果第一个参数为空，this就指向window。
那么如果对一个函数多次使用bind，结果会是什么呢？
```JavaScript
  let a = {};
  let fn = function () {
    console.log(this);
  }
  fn.bind().bind(a);
```
如果你认为输出的是a，那么就错了。可以将上面代码转换为另一种形式:

```JavaScript
  // fn.bind().bind(a) 相当于
  const fn2 = function fn1 () {
    return function () {
      retur fn.apply();
    }.apply(a)
  }
  fn2();
```

可以发现，不管给函数bind几次，fn中dethis永远由第一次bind决定，所以结果永远是window。

```JavaScript
  const a = { name: 'gxr' };
  function foo () {
    console.log(this.name);
  }
  foo.bind(a); // gxr
```

## 总结
new的方式优先级最高，接下来就是bind这些函数，然后是obj.foo()这种方式，最后是直接调用的方式。同时，箭头函数中的this一旦被绑定，就不能再改变。
有可能会出现多种规则同时存在的情况，这时就会根据不同的规则优先级来决定this的指向。

## 活学活用
两道关于this的笔试题。

### 第一个
```JavaScript
  var length = 10;
  function fn() {
      console.log(this.length);
  }
  
  var obj = {
    length: 5,
    method: function(fn) {
      fn();
      arguments[0]();
    }
  };
  obj.method(fn, 1);
  // 结果是: 10 2
```
分析:
1. 在执行obj.method()方法时，如果函数内部有this，那么this确实是指向obj，但是method()内部执行的是fn()函数，而fn()函数绑定的对象是window，即window.fn()，所以会输出10；
2. 全局函数fn同时也属于arguments数组中的一员，即当作为arguments成员之一调用的时候，其作用域其实就绑定到了arguments上，this也就是指向了arguments对象，所以arguments[0]()这段代码调用了身为成员的fn()函数，this.length就等于是arguments.length，又因为method传入的参数为2个，所以最后又输出2。

### 第二个
```JavaScript
  
```