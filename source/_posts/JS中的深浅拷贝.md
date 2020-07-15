---
title: JS中的深浅拷贝
date: 2020-07-14 17:49:48
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
我们已经知道了对象类型在赋值过程 中其实是复制了地址，从而导致改变了一方其他也被改变的情况。通常在开发中我们不希望出现这样的情况，这时可以使用浅拷贝来解决。

```JavaScript
  let a = {
    age: 1
  }
  let b = a
  a.age = 2
  console.log(b.age) // 2
```

## 浅拷贝
