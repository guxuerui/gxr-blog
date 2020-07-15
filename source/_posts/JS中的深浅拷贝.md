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
1. 首先可以通过Object.assign来解决，Object.assign会拷贝所有的属性值到新的对象中，如果属性值是对象的话，拷贝的是地址，所以并不是深拷贝。
  ```JavaScript
    let a = {
      age: 1
    }
    let b = Object.assign({}, a)
    a.age = 2
    console.log(b.age) // 1
  ```

2. 还可以通过ES6中的扩展运算符 … 来实现浅拷贝。
  ```JavaScript
    let a = {
      age: 1
    }
    let b = { ...a }
    a.age = 2
    console.log(b.age) // 1
  ```

3. 通常浅拷贝就能解决大部分问题了，但当遇到如下情况时就需要使用深拷贝了。
  ```JavaScript
    let a = {
      age: 1,
      jobs: {
        first: 'FE'
      }
    }
    let b = { ...a }
    a.jobs.first = 'native'
    console.log(b.jobs.first) // native
  ```
  浅拷贝只解决了第一层的问题，如果接下去的值中还有对象的话，那么就又回到最开始的话题了，两者享有相同的地址。要解决这个问题，我们就得使用深拷贝了。

## 深拷贝
1. 通常可以通过 JSON.parse(JSON.stringify(object)) 来解决
```JavaScript
  let a = {
    age: 1,
    jobs: {
      first: 'FE'
    }
  }
  let b = JSON.parse(JSON.stringify(a))
  a.jobs.first = 'native'
  console.log(b.jobs.first) // FE
```

  但是该方法也是有局限性的：

* 会忽略undefined
* 会忽略symbol
* 不能序列化函数
* 不能解决循环引用的对象


2. 如果有这么一个循环引用对象，你会发现并不能通过该方法实现深拷贝，会产生报错
```JavaScript
  let obj = {
    a: 1,
    b: {
      c: 2,
      d: 3,
    },
  }
  obj.c = obj.b
  obj.e = obj.a
  obj.b.c = obj.c
  obj.b.d = obj.b
  obj.b.e = obj.b.c
  let newObj = JSON.parse(JSON.stringify(obj))
  console.log(newObj)
```

3. 在遇到函数、 undefined 或者 symbol 的时候，该对象也不能正常的序列化
```JavaScript
  let a = {
    age: undefined,
    sex: Symbol('male'),
    jobs: function() {},
    name: 'gxr'
  }
  let b = JSON.parse(JSON.stringify(a))
  console.log(b) // {name: "gxr"}
```
在上述情况中，该方法会忽略掉函数和 undefined、symbol 。
但是在通常情况下，复杂数据都是可以序列化的，所以这个函数可以解决大部分问题

4. 使用MessageChannel
如果所需拷贝的对象含有内置类型并且不包含函数，可以使用 MessageChannel，MessageChannel API允许我们创建一个新的消息通道，并通过它的两个MessagePort属性发送数据:
```JavaScript
  function structuralClone(obj) {
    return new Promise(resolve => {
      const { port1, port2 } = new MessageChannel()
      port2.onmessage = ev => resolve(ev.data)
      port1.postMessage(obj)
    })
  }

  var obj = {
    a: 1,
    b: {
      c: 2
    }
  }

  obj.b.d = obj.b

  // 注意该方法是异步的
  // 可以处理 undefined 和循环引用对象
  const test = async () => {
    const clone = await structuralClone(obj)
    console.log(clone)
  }
  test()
```