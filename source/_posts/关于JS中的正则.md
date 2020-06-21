---
title: 关于JS中的正则
date: 2020-06-21 17:18:54
tags: 
  - JavaScript
  - 前端
categories:
  - Web前端
---
## 关于JS中正则的使用
  正则表达式在前端开发中的使用频率可以说是非常高了，巧妙的使用正则可以大大提高我们的开发效率。正则中有几种通用符号表示，表示对应的字符可出现的频率:
  ```JavaScript
    ?: 可有可无，最多一次
    *: 可有可无，多了不限
    +: 至少一次，多了不限

    强调: 字符集，默认仅修饰相邻的前一个字符集
  ```
## 下面是几种正则表达式的使用方法
### 给单词末尾依次添加数字
关于replace方法详情[看这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
```JavaScript
  let str = "Tom is a good man";
  let i = 1;
  str = str.replace(/b[a-zA-Z]+\b/g, function ($1) {
    return $1 + i++;
  });
  console.log(str); // Tom1 is2 a3 good3 man5
```
### 将单词首字符转化为大写
```JavaScript
  let str = "we who and who";
  str = str.replace(/\b[a-z]/g, function (kwd) {
    return kwd.toUpperCase();
  })
  console.log(str); // We Who And Who
```
### 删除字符串开头或结尾的空字符串
```JavaScript
  function ltrim (str) {
    return str.replace(/^\s+/, '');
  }

  function rtrim (str) {
    return str.replace(/\s+$/, '');
  }

  function trim (str) {
    return str.replace(/^\s+|\s+$/g, '');
  }

  let str = " \t路 飞 \t";
  console.log(ltrim(str)); //路飞
  console.log(rtrim(str)); // 路飞
  console.log(trim(str)); //路 飞
```
### 日期时间格式化
```JavaScript
  let date = "20200621日下1753";
  date = date.replace(/(\d{4})(\d{2})(\d{2})([\u4e00-\u9fa5])([\u4e00-\u9fa5])(\d{2})(\d{2})/, "$1年$2月$3日 星期$4 $5午 $6:$7");
  console.log(date); // 2020年06月21日 星期日 下午 17:53
```
### 金额校验, 大于0且只能输入最多13位整数和最多两位小数
```JavaScript
  let str = '1232321.33';
  let str1 = '1232321.332';
  let str2 = '123453453453452321.32';
  let str3 = '0.34';
  let reg = /^(?!0+$)(?!0*\.0*$)\d{1,13}(\.\d{1,2})?$/;
  console.log(reg.test(str)); // true
  console.log(reg.test(str1)); // false
  console.log(reg.test(str2)); // false
  console.log(reg.test(str3)); // true
```