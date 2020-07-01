---
title: JS中==和===的区别
date: 2020-07-01 16:37:14
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
JS中==用于一般比较(不严格)，===用于严格比较，==在比较的时候若需要会先转换数据类型；而===是严格比较，只要类型不匹配就返回flase。

## 对于==

对于==来说，如果对比双方类型一样，就直接比较值是否相等；如果对比双方的类型不一样，就会先进行类型转换。 比如若我们需要对比x和y是否相同，使用==就会进入如下判断流程:
1. 若x和y的类型相同，则JS会转换成===进行比较。
2. 若一方为null，且另一方为undefined，则返回true。
3. 若x是number类型，y是string类型，则比较 x == toNumber(y)，反之亦然。
4. 若x为boolean类型，则比较 toNumber(x) == y，反之亦然。
5. 若x是string、symbol或number类型，而y是Object类型，则比较 x == toPrimitive(y)，x和y类型互换也是如此。
6. 除以上情形外, 会返回false。

特别的是: 当对象执行toPrimitive时，会首先调用对象的valueOf()方法，再调用对象的toString()方法。

举个栗子:
```JavaScript
  5 == 5 // true
  1 == '1' // true
  null == undefined // true
  0 == false // true
  1 == true // true
  '1,2' == [1,2] // true
  '[object Object]' == {} // true, {}.toString()会返回'[object Object]'
```

## 对于===
对于===来说, 就是判断两者类型和值是否完全相同, 若类型不同就会直接返回false; 类型相同再继续比较值是否相等。
举个栗子:
```JavaScript
  5 === 5 // true
  1 === '1' // false
  null === undefined // false
  0 === false // false
  1 === true // false
  '1,2' === [1,2] // false
  '[object Object]' === {} // false
```

## 活学活用
记大佬的提问: 构造一个a，使得a同时满足判断条件a == 1 && a == 2 && a == 3
```JavaScript
  let a = {
    i: 1,
    toString: function () {
      return a.i++;
    }
  }
  // 利用对象进行==判断时先调用toString()方法进行类型转换
  if (a == 1 && a == 2 && a == 3) {
    console.log(1); // 打印 1
  } else {
    console.log(2);
  }
```