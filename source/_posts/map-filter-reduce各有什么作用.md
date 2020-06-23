---
title: 'map,filter,reduce各有什么作用'
date: 2020-06-23 10:01:02
tags:
  - JavaScript
  - 前端
categories:
  - Web前端
---
ES6语法可以说是前端开发中最常用的语法了，那么关于map、filter、reduce的基础用法都有那些呢？

## map

map的作用是生成一个新数组，遍历原数组，将每个元素拿出来做一些变换，然后放入到新的数组中。
举个栗子:

```JavaScript
  [1, 2, 3].map(v => v + 1); // [2, 3, 4]
```

另外，map的回调函数接收三个参数，分别是当前索引元素，索引和原数组。
举个栗子:

```JavaScript
  ['1', '2', '3'].map(parseInt);
```
-- 第一轮遍历 parseInt('1', 0) -> 1
-- 第二轮遍历 parseInt('2', 1) -> NaN
-- 第三轮遍历 parseInt('3', 2) -> NaN

parseInt()函数可以解析一个字符串，并返回一个整数。
语法: parseInt(string, radix)

其中:
string: 必须，要被解析的字符串。
radix: 可选，表示要被解析的数字的技术，该值介于 2~36之间。

如果radix参数省略或值为0，则数字将以10为基础来解析；如果它以"0x"或"0X"来开头，则将以16位基数；如果该值小于2或大于36，则parseInt()将返回NaN。

PS: 
(1) 只有字符串中的第一个数字会被返回； 
(2) 开头和结尾的空格是被允许的；
(3) 如果字符串的第一个字符不能被转换为数字，那么parseInt()会返回NaN；

## filter

filter的作用是生成一个新数组，在遍历数组的时候将返回值为true的元素放入新数组，可以利用这个函数删除一些不需要的元素。
举个栗子:

```JavaScript
  let arr = [1, 2, 3, 4, 5];
  let newArr = arr.filter(item => item !== 4);
  console.log(newArr); // [1, 2, 3, 5]
```
filter和map一样，回调函数同样接收三个参数，作用也相同。

## reduce

reduce可以将数组中的元素通过回调函数最终转换为一个值，比如实现一个功能将函数中的元素全部相加得到一个值:

```JavaScript
  let arr = [1, 2, 3], total = 0;
  for (let i = 0; i < arr.length; i++) {
    total += arr[i];
  }
  console.log(total); // 6
```
但是如果用reduce的话，可以简化很多:

```JavaScript
  let arr = [1, 2, 3];
  let total = arr.reduce((sum, curr) => sum + curr, 0);
  console.log(total); // 6
```
对于reduce来说，它接收两个参数，分别是回调函数和初始值。
对于上面的代码来说：
-- 首先初始值为0，该值会在执行第一次回调函数时作为第一个参数传入
-- 回调函数接收四个参数，分别是累计值、当前元素、当前索引、原数组
-- 在第一次执行回调函数时，当前值和初始值相加得出结果1，该结果会在第二次执行回调函数时当做第一个参数传入
-- 所以在第二次执行回调函数时，相加的值就分别是1和2，以此类推，循环结束后结果是6

### 通过reduce来实现一个map函数
```JavaScript
  const arr = [1, 2, 3];
  const mapArray = arr.map(item => item * 2);

  const reduceArray = arr.reduce((acc, curr) => {
    acc.push(curr * 2);
    return acc;
  }, [])

  console.log(mapArray, reduceArray); // [2, 3, 6]   [2, 4, 6]
```