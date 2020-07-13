---
title: JS中typeof VS instanceof
date: 2020-07-13 16:47:40
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
在JS中，当我们判断变量类型时，有时候typeof并不能正确的返回结果，而instanceof可以。

## typeof

1. 对于原始类型来说，除了typeof null都可以正确返回类型。

  ```JavaScript
    typeof 1; // 'number'
    typeof '1' // 'string'
    typeof undefined // 'undefined'
    typeof true // 'boolean'
    typeof Symbol() // 'symbol'
    typeof null; // 'object' 但null实际上不是对象
  ```

2. 对于对象来首，除了typeof 函数会返回function，其它情况都会返回object，所以typeof并不能准确判断变量到底是什么类型。

  ```JavaScript
    typeof [] // 'object'
    typeof {} // 'object'
    typeof console.log // 'function''
  ```

## instanceof

1. 如果想判断一个对象的正确类型，可以考虑使用instanceof，因为它的内部机制是通过原型链来判断的。

 ```JavaScript
  [] instanceof Array // true
  {} instanceof Object // true
  console.log instanceof Function // true 

  const Person = function() {}
  const p1 = new Person()
  p1 instanceof Person // true

  var str = 'hello world'
  str instanceof String // false

  var str1 = new String('hello world')
  str1 instanceof String // true
 ```

2. 而对于原始类型来说，想直接通过instanceof来判断类型是不行的，需要做个封装。

  ```JavaScript
    class PrimitiveString {
      static [Symbol.hasInstance](x) {
        return typeof x === 'string'
      }
    }
    console.log('hello world' instanceof PrimitiveString) // true
  ```
Symbol.hasInstance就是一个可以让我们自定义instanceof行为的方法，以上代码等同于typeof ‘hello world’ === ‘string’，结果自然是true。这从侧面反映了instanceof判断类型也不是百分百可信的。