---
title: 前端JS转换日期格式
date: 2020-08-11 14:49:19
tags: 
  - JavaScript
  - 前端
categories:
  - Web前端
---
**1. 日常开发中，经常会遇到处理日期时间的时候，这是一个很灵活的日期格式化函数，可以根据给定的格式参数进行格式化，可以应对大部分场景。**

代码:
```JS
  const formatDate = (format='Y-M-D h:m', timestamp=Date.now()) => {
    const date = new Date(timestamp);
    const dateInfo = {
      Y: date.getFullYear(),
      M: date.getMonth() + 1,
      D: date.getDate(),
      h: date.getHours(),
      m: date.getMinutes(),
      s: date.getSeconds()
    };
    const formatNum = (n) => n > 10 ? n : '0' + n;
    const res = format
      .replace('Y', dateInfo.Y)
      .replace('M', dateInfo.M)
      .replace('D', dateInfo.D)
      .replace('h', formatNum(dateInfo.h))
      .replace('m', formatNum(dateInfo.m))
      .replace('s', formatNum(dateInfo.s));
    return res;
  }
```
示例:
```JS
  formatDate(); // "2020-8-11 15:21"
  formatDate('M月D日 h:m'); // "8月11日 15:22"
  formatDate('Y年M月D日 h:m:s'); // "2020年8月11日 15:23:14"
  formatDate('h:m Y-M-D', 1597130693212); // "15:24 2020-8-11"
```

**2. 一次性生成一周的时间**
new Array 创建的数组只是添加了length属性，并没有实际的内容。通过处理后，变为可用数组用于循环。
代码:
```JS
  function getWeekTime(){
    return [...new Array(7)].map((j,i)=> new Date(Date.now() + i * 8.64e7).toLocaleDateString())
  }
```

示例:
```JS
  getWeekTime();
  /*输出:
    [ '2020-8-11',
    '2020-8-12',
    '2020-8-13',
    '2020-8-14',
    '2020-8-15',
    '2020-8-16',
    '2020-8-17' ]
  */
```