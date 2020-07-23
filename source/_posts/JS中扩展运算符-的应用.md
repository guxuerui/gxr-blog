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

1. 由于扩展运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了
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

2. 下面是扩展运算符取代apply方法的一个实际的例子，应用Math.max方法，简化求出一个数组最大元素的写法
```JavaScript
  // ES5 的写法
  Math.max.apply(null, [14, 3, 77])
  
  // ES6 的写法
  Math.max(...[14, 3, 77])
  
  // 等同于
  Math.max(14, 3, 77);
```

3. 通过concat函数，将一个数组添加到另一个数组的尾部。
```JavaScript
  // ES5的 写法
  var arr1 = [0, 1, 2];
  var arr2 = [3, 4, 5];
  Array.prototype.concat.apply(arr1, arr2); // [0, 1, 2, 3, 4, 5]
  
  // ES6 的写法
  let arr1 = [0, 1, 2];
  let arr2 = [3, 4, 5];
  arr1.concat(...arr2); // [0, 1, 2, 3, 4, 5]
```

## 扩展运算符的应用

1. 复制数组
数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。
```JavaScript
  const a1=[1,2]
  const a2 = a1
  a2[0]=2
  a1 // [2, 2]
```
上面代码中，a2并不是a1的克隆，而是指向同一份数据的另一个指针。修改a2，会直接导致a1的变化。
在ES5中，只能用变通的方法来复制数组
```JavaScript
  const a1 = [1, 2]
  const a2 = a1.concat()
  a2[0] = 2
  a1 // [1, 2]
```
上面代码中，a1会返回原数组的克隆，再修改a2就不会对a1产生影响了。
扩展运算符提供了复制数组的更简洁的方法。
```JavaScript
  const a1 = [1, 2]
  // 写法一
  const a2 = [...a1]
  // 写法二
  const [...a2] = a1
```

2. 合并数组
扩展运算符提供了数组合并的新方法。
```JavaScript
  const arr1 = ['a', 'b']
  const arr2 = ['c'];
  const arr3 = ['d', 'e']
  
  // ES5 的合并数组
  arr1.concat(arr2, arr3);
  // [ 'a', 'b', 'c', 'd', 'e' ]
  
  // ES6 的合并数组
  [...arr1, ...arr2, ...arr3]
  // [ 'a', 'b', 'c', 'd', 'e' ]
```
不过，这两种方法都是浅拷贝，使用的时候需注意。
```JavaScript
  const a1 = [{ foo: 1 }]
  const a2 = [{ bar: 2 }]
  
  const a3 = a1.concat(a2)
  const a4 = [...a1, ...a2]
  
  a3[0] === a1[0] // true
  a4[0] === a1[0] // true
```
上面代码中，a3和a4是用两种不同方法合并而成的新数组，但是它们的成员都是对原数组成员的引用，这就是浅拷贝。如果修改了原数组的成员，会同步反映到新数组。

3. 与结构赋值结合
扩展运算符可以与解构赋值结合起来，用于生成数组。
```JavaScript
  // ES5
  a = list[0], rest = list.slice(1)
  // ES6
  [a, ...rest] = list
  //另外的例子
  const [first, ...rest] = [1, 2, 3, 4, 5]
  first // 1
  rest  // [2, 3, 4, 5]

  const [first, ...rest] = [];
  first // undefined
  rest  // []

  const [first, ...rest] = ["foo"]
  first  // "foo"
  rest   // []
```
如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
```JavaScript
  const [...butLast, last] = [1, 2, 3, 4, 5] // 报错
    
  const [first, ...middle, last] = [1, 2, 3, 4, 5] // 报错
```

4. 字符串
扩展运算符还可以将字符串转为真正的数组。
```JavaScript
  [...'hello']
  // [ "h", "e", "l", "l", "o" ]
```
