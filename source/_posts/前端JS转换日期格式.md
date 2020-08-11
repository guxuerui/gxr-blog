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
  const fomatDate = (format='Y-M-D h:m', timestamp=Date.now()) => {
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
      .replace('D'. dateInfo.D)
      .replace('h', formatNum(dateInfo.h))
      .replace('m', formatNum(dateInfo.m))
      .replace('s', formatNum(dateInfo.s));
    return res;
  }
```
示例:
```
  
```