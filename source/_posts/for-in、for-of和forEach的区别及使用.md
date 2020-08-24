---
title: for in、for of和forEach的区别及使用
date: 2020-08-24 15:32:57
tags: 
  - JavaScript
  - 前端
categories:
  - Web前端
---
## 1. for in
```JavaScript
  /*
    for in用于遍历数组或对象的属性(对数组或对象的属性进行循环操作)。对于数组，迭代出来的是数组元素；对于对象，迭代出来的是对象的属性。
  */
  const arr = ['1a', '2b', '3c'];
  for (let k in arr) {
    console.log(k); // 0 1 2
    console.log(arr[k]); // 1a 2b 3c
  }

  const obj = {
    name: 'gxr',
    age: 25,
    height: 175
  };
  for (let k in obj) {
    console.log(k); // name age height
    console.log(obj[k]); // gxr 25 175
  }
```

## 2. for of
```JavaScript
  /*
    for of可在迭代对象(Array, Map, Set, String, TypedArray, arguments)上创建一个迭代循环。
  */
  // 遍历字符串 -- string
  let str = 'abcdef'
  for (let val of str) {
    console.log(val); // a b c d e f
  }

  // 遍历Set -- 对象允许你存储任何类型的唯一值，无论是原始值还是对象引用  Set方法可以用来做数组去重
  let set = new Set([1,2,2,3,3,4,4,5]);
  console.log(set) // Set(5)[1, 2, 3, 4, 5]
  for (let val of set) {
    console.log(val); // 1 2 3 4 5
  }
  

  // 遍历Map -- 对象保存键值对，任何值（对象或者原始值）都可以作为一个键或者一个值。
  let map = new Map([["a",0],["b",1],["c",2]])
  for (let arr of map) {
    console.log(arr); // ["a",0] ["b",1] ["c",2]
  }

  for (let [key,value] of map) {
    console.log(key); // a b c 
    console.log(value); // 0 1 2
  }

  // 遍历数组
  let arr = [1,2,3,4,5]；
  for (let val of arr) {
    console.log(val); // 1 2 3 4 5
  }

  // 遍历DOM
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>

  let dom = document.getElementsByTagName('li')
  for (let item of dom) {
    console.log(item);
  }
  /*输出:
    <li>1</li>
    <li>2</li>
    <li>3</li>
  */
```

## 3. forEach
```JavaScript
  /*
    forEach:
    数组一个方法，用于遍历数组，性能相较于for循环弱;
    定义和用法：forEach()用于调用数组的每个元素，并将元素传递给回调函数;    
    注：forEach在运行途中无法跳出循环，break和return不起作用，对于空数组不会执行回调函数;
    语法：array.forEach(function(currentValue, index, arr), thisValue);  
    方法中的参数: function（回调函数）和currentValue（当前元素）是必需的i, index是当前元素索引值，
    arr元素是所属的数组对象，thisValue是传递给函数的值，一般用"this"值。如果这个参数为空，"undefined" 会传递给 "this" 值。
  */
  const arr = [1, 2, 3, 4, 5];
  arr.forEach((value, index) => console.log('index的值是：'+index, 'value的值是：'+value))
  /*输出:
    0 1
    1 2
    2 3
    3 4
    4 5
  */

  let arr1 = [[{b : 'c', d : 'f'}]]
  let arr2 = [[{e : 'g', k : 'h'}]]
  let arr5 = arr1.concat(arr2);
  console.log(arr5); //[[{b : c, d : f}], [{e : g, k : h}]]
```

## 总结
1. **for in**
对于数组，迭代出来的是数组元素；对于对象，迭代出来的是对象的属性。
缺点: 键名是字符串；会遍历对象本身的所有可枚举属性和从它原型继承而来的可枚举属性，仅迭代对象本身的属性，要结合hasOwnProperty来使用，；某种情况下回按任意顺序遍历。

2. **for of**
和for in一样简介的语法但是没有for in的缺点，还提供了遍历多种数据结构的统一操作接口。

3. **forEach**
针对数组，运行过程中无法跳出循环；空数组无法执行回调函数。