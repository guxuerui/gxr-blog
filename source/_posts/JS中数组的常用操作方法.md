---
title: JS中数组的常用操作方法
date: 2020-06-29 14:34:14
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
数组可以说是前端最常用的一种数据类型了，一些ES6的数组操作方法可以大大方便日常工作。关于数组的定义和基本属性可以看[Array - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)，下面就列举出常用的数组操作方法：

## 改变原数组的操作方法
下面的这些方法会改变调用它们的数组自身的值

### Array.prototype.copyWithin()
浅拷贝数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。

```JavaScript
  let arr = [1, 2, 3, 4, 5];
  console.log(arr.copyWithin(0, 3, 4)); // [4, 2, 3, 4, 5]
  console.log(arr.copyWithin(1, 3)); // [4, 4, 5, 4, 5]
```

### Array.prototype.fill()
用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引。

```JavaScript
  const array1 = [1, 2, 3, 4];
  // 三个参数分别代表：用来填充数组的值；起始索引，默认值为0；终止索引，默认值为数组长度
  console.log(array1.fill(0, 2, 4)); // [1, 2, 0, 0]

  console.log(array1.fill(5, 1)); // [1, 5, 5, 5]

  console.log(array1.fill(6)); // [6, 6, 6, 6]
```

### Array.prototype.pop()
删除数组的最后一个元素，并返回这个元素。
```JavaScript
  let arr = [1, 2, 3];
  arr.pop(); // 3
```

### Array.prototype.push()
在数组的末尾增加一个或多个元素，并返回数组的新长度。
```JavaScript
  let arr = [1, 2];
  arr.push(3); // 3
  arr; // [1, 2, 3]
```

 ### Array.prototype.reverse()
 颠倒数组中元素的排列顺序，即原先的第一个变为最后一个，原先的最后一个变为第一个。
 ```JavaScript
  let arr = [1, 2, 3];
  arr.reverse(); // [3, 2, 1]
 ```

 ### Array.prototype.shift()
 删除数组的第一个元素，并返回这个元素。
 ```JavaScript
  let arr = [1, 2, 3];
  arr.shift(); // 1
 ```

 ### Array.prototype.unshift()
 在数组的开头增加一个或多个元素，并返回数组的新长度。
 ```JavaScript
  let arr = [1, 2, 3];
  arr.unshift(4); // [3, 1, 2, 3]
 ```

 ### Array.prototype.sort()
 对数组元素进行排序，并返回当前数组。
 ```JavaScript
  let arr = [1, 5, 3, 4, 2];
  arr.sort(); // [1, 2, 3, 4, 5]
  // 从小到大排序
  arr.sort((a, b) => {
    return a - b;
  })
  // 从大到小排序
  arr.sort((a, b) => {
    return b - a;
  })
 ```

 ### Array.prototype.splice()
 在任意的位置给数组添加或删除任意个元素。
 ```JavaScript
  let arr = [1, 2, 3];
  // 添加元素
  let arr1 = arr.splice(3, 0, 4);
  arr1; // []
  arr; // [1, 2, 3, 4]

  // 删除元素
  let removed = arr.splice(3);
  removed; // [4]
  arr; // [1, 2, 3]
 ```


 ## 不会改变原数组的方法
下面的这些方法绝对不会改变调用它们的数组自身的值，只会返回一个新的数组或者返回一个其它的期望值。

 ### Array.prototype.concat()
 返回一个由当前数组和其它若干个数组或者若干个非数组值组合而成的新数组。
 ```JavaScript
  let arr = [1, 2, 3];
  let arr1 = [4, 5, 6];
  arr.concat(arr1); // [1, 2, 3, 4, 5, 6]
 ```

### Array.prototype.includes()
判断当前数组是否包含某指定的值，如果是返回 true，否则返回 false。
```JavaScript
  let arr = [1, 2, 3];
  arr.includes(1); // true
  arr.includes(4); // false
```

### Array.prototype.join()
连接所有数组元素组成一个字符串。
```JavaScript
  let arr = [1, 2, 3];
  arr.join(','); // '1,2,3'
```

### Array.prototype.slice()
返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。原始数组不会被改变。
```JavaScript
  let arr = [1, 2, 3];
  let arr1 = arr.slice(); // [1, 2, 3]
  let arr2 = arr.slice(1); // [2, 3]
  let arr3 = arr.slice(1, 2); // [2]
```

### Array.prototype.toString()
返回一个由所有数组元素组合而成的字符串，遮蔽了原型链上的Object.prototype.toString() 方法。
对于数组对象，toString 方法连接数组并返回一个字符串，其中包含用逗号分隔的每个数组元素。
```JavaScript
  let arr = [1, 2, 3];
  arr.toString(); // '1,2,3'
```

### Array.prototype.toLocaleString()
 返回一个字符串表示数组中的元素。数组中的元素将使用各自的 toLocaleString 方法转成字符串，这些字符串将使用一个特定语言环境的字符串（例如一个逗号 ","）隔开。。遮蔽了原型链上的 Object.prototype.toLocaleString() 方法。
 数组中的元素将会使用各自的 toLocaleString 方法。
 ```JavaScript
  let arr = [1, 'a', new Date()];
  arr.toLocaleString(); // '1,a,6/29/2020, 5:26:22 PM'
 ```

### Array.prototype.indexOf()
返回数组中第一个与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。
```JavaScript
  let arr = [1, 2, 3];
  arr.indexOf(1); // 0
  arr.indexOf(4); // -1
```

### Array.prototype.lastIndexOf()
返回数组中最后一个（从右边数第一个）与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。
```JavaScript
  let arr = [1, 2, 3, 3, 1];
  arr.lastIndexOf(1); // 4
  arr.lastIndexOf(4); // -1
```

## 遍历方法
很多遍历方法都需要指定一个回调函数作为参数。

### Array.prototype.forEach()
为数组中的每个元素执行一次回调函数。
```JavaScript
  let arr = [1, 2, 3];
  arr.forEach(item => console.log(item));// 1 2 3
```

### Array.prototype.every()
如果数组中的每个元素都满足测试函数，则返回 true，否则返回 false。
```JavaScript
  let arr = [1, 2, 3];
  arr.every(item => item > 1); // true
  arr.every(item => item >= 1); // false
```

### Array.prototype.some()
如果数组中至少有一个元素满足测试函数，则返回 true，否则返回 false。
```JavaScript
  let arr = [1, 2, 3];
  arr.some(item => item > 2); // true
  arr.some(item => item > 4); // false
```

### Array.prototype.filter()
将所有在过滤函数中返回 true 的数组元素放进一个新数组中并返回。
```JavaScript
  let arr = [1, 2, 3];
  arr.filter(item => item > 1); // [2, 3]
```

### Array.prototype.find()
找到第一个满足测试函数的元素并返回那个元素的值，如果找不到，则返回 undefined。
```JavaScript
  let arr = [1, 2, 3];
  arr.find(item => item > 1); // 2
  arr.find(item => item > 3); // undefined
```

### Array.prototype.findIndex()
找到第一个满足测试函数的元素并返回那个元素的索引，如果找不到，则返回 -1。
```JavaScript
  let arr = [1, 2, 3];
  arr.find(item => item > 1); // 1
  arr.find(item => item > 3); // undefined
```

### Array.prototype.map()
从左到右为每个数组元素执行一次回调函数, 返回一个由回调函数的返回值组成的新数组。
```JavaScript
  let arr = [1, 2, 3];
  arr.map(item => item + 1); // [2, 3, 4]
```

###  Array.prototype.reduce()
从左到右为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。
```JavaScript
  let arr = [1, 2, 3];
  arr.reduce((sum ,curr) => {
    return sum + curr;
  }, 0); // 6
```

### Array.prototype.reduceRight()
从右到左为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。
```JavaScript
  let arr = [[0, 1], [2, 3], [4, 5]];
  arr.reduceRight((sum ,curr) => {
    return sum.concat(curr);
  }); // [4, 5, 2, 3, 0, 1]
```

### Array.prototype.keys()
返回一个数组迭代器对象，该迭代器会包含所有数组元素的键。
```JavaScript
  let arr = [1, 2, 3];
  let keys = arr.keys(); // Object [Array Iterator] {}
  for (const k of keys) {
    console.log(k); // 0 1 2
  }
```

### Array.prototype.values()
返回一个数组迭代器对象，该迭代器会包含所有数组元素的值。
```JavaScript
  let arr = [1, 2, 3];
  let iterator = arr.values(); // Object [Array Iterator] {}
  for (const k of iterator) {
    console.log(k); // 1 2 3
  }
```

### Array.prototype.entries()
返回一个数组迭代器对象，该迭代器会包含所有数组元素的键值对。
```JavaScript
  let arr = [1, 2, 3];
  let iterator = arr.entries(); // Object [Array Iterator] {}
  for (const k of iterator) { 
    console.log(k); // [0, 1] [1, 2] [2, 3]
  }

  // 或者使用Inerator对象的next方法
  iterator.next().value(); // [0, 1]
  iterator.next().value(); // [1, 2]
  iterator.next().value(); // [2, 3]
  iterator.next().value(); // undefined
```

## 几个简单应用

### 扁平化n维数组
Array.prototype.flat()是ES10的扁平数组的API，n表示唯独，n值为Infinity时为无限大。
```JavaScript
  console.log([1,[2,3]].flat(2)); // [1,2,3]

  // 原理：实际是利用递归和数组合并方法concat实现扁平数组
  function flat(arr) {
    while(arr.some(item => Array.isArray(item))) {
      arr = [].concat(...arr)
    }
    return arr;
  }
```


### 数组去重
```JavaScript
  // 1. 使用indexOf
  let arr1 = [1,2,2,2,3,3,3,4,4,5,6];
  let arr2 = [];
  for (let i = 0; i < arr1.length; i++) {
    if (arr2.indexOf(arr1[i]) < 0>) {
      arr2.push(arr1[i]);
    }
  }
  console.log(arr2); // [1,2,3,4,5,6]

  // 2. 使用ES6的Set方法，set是ES6新出来的一种一种定义不重复数组的数据类型
  // Array.from是将类数组转化为数组，...是扩展运算符，将set里面的值转化为字符串。
  console.log(Array.from(new Set([1,1,2,2,3,3,3,4,4,5]))); // [1,2,3,4,5]
  console.log([...new Set([1,1,2,2,3,3,3,4,4,5])]); // [1,2,3,4,5]
```


### 求数组中的最大值
```JavaScript
  let arr = [1,2,3,4];

  // 1.
  Math.max.apply(this, arr); // 4
  // 2.
  Math.max(...arr); // 4
  // 3.
  arr.reduce((pre, cur) => {
    return Math.max(pre, cur);
  }, 0); // 4
```