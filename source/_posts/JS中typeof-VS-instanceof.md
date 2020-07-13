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
