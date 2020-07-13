---
title: JS中的类型转换
date: 2020-07-13 17:20:44
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
在JS中数据类型转换存在隐式转换和强制转换，并且基本数据类型转换只有三种情况，分别是:
1. 转换为字符串
2. 转换为数字
3. 转换为布尔值

## 隐式转换
  ```JavaScript
    5 + null    // 返回 5, null 转换为 0
    "5" + null  // 返回"5null", null 转换为 "null"
    "5" + 1     // 返回 "51", 1 转换为 "1"  
    "5" - 1     // 返回 4, "5" 转换为 5
  ```

## 强制转换
 ```JavaScript
  String(123); // '123'
  String(null); // 'null'
  String([]); // ''
  String({}); // '[object object]'

  Number('123'); // 123
  Number(null); // 0
  Number([]); // 0
  Number(undefined); // NaN
  Number({}); // NaN

  Boolean(123); // true
  Boolean('123'); // true
  Boolean([]); // true
  Boolean({}); // true
  Boolean(null); // false
  Boolean(undefined); // false
 ```

## 转换为Boolean
 在条件判断时，除了 undefined， null， false， NaN，''， 0， -0，其他所有值都转为 true，包括所有对象。

## 对象转原始类型
对象在转换类型的时候，会调用内置的 [[ToPrivitive]]函数，这个方法在调用时优先级最高，对于这个函数来说，算法逻辑一般如下：

* 如果已经是原始类型了，那就不需要再次转换
* 调用 x.valueOf()，如果转换为基础类型，就返回转换的值(先调用valueOf()方法)
* 如果需要转字符串类型就调用 x.toString()，转换为基础类型的话就返回转换的值。不是字符串类型的话就先调用 valueOf，结果不是基础类型的话再调用 toString
* 如果都没有返回原始类型，就会报错

## 进行四则运算时
加法运算符不同于其他几个运算符：
* 运算中其中一方为字符串，那么就会把另一方也转换为字符串
* 如果一方不是字符串或者数字，那么会将它转换为数字或者字符串

对于加法 ‘a’+ + ‘b’会返回 ‘aNaN’，因为 + ‘b’ 等于 NaN，所以结果为 “aNaN”，你可能也会在一些代码中看到过 +‘1’ 的形式来快速获取 number 类型。
对于除了加法的运算符来说，只要其中一方为数字，那么另一方就会被转化为数字。

  ```JavaScript
    4 * '3'; // 12
    4 * []; // 0
    4 * [1, 2]; // NaN
    4 - '3'; // 1
    4 / '2'; // 2
  ```

## 进行比较运算时
1. 如果是对象，就调用toPrimitive转化对象
2. 如果是字符串，就通过unicode字符索引来比较

  ```JavaScript
    1 > '34'; // false
    '23' > 21; // true
    '34' > '12'; // true

    let a = {
      valueOf() {
        return 0
      },
      toString() {
        return '1'
      }
    }
    a > -1 // true，此时a是 0
  ```
  这里因为 a 是对象，所以会先通过 valueOf() 转换为原始类型再比较值。

## 小技巧
1. 使用 + 号，转换为 number 类型
2. 使用 0 相减，转换为 number 类型
3. 使用 !! 号，转换为 Boolean 类型

  ```JavaScript
    +'10'; // 10
    +'a'; // NaN

    '10' - 0; // 10
    'a' - 0; // NaN

    !!'hello world'; // true
    !!null; // false
  ```