---
title: '探究JavaScript数据结构与算法:复杂度分析'
categories: JavaScript
toc: true
date: 2019-12-03 10:27:32
tags: 数据结构与算法
---

## 前言
一个算法的性能，可以从时间和空间这两个维度去分析，即算法执行的时间和所占用的空间。本文将通过不同的算法代码从最坏情况来分析其复杂度。

## 大O表示法
表示代码执行所耗费的时间/空间随数据规模增长的变化趋势，用来衡量一个算法的性能和复杂程度，通常通过以下函数表示，按耗费时间从小到大排列:
| 符号      | 名称    |
|----------|-------- |
| O(1)     | 常数阶    |
| O(logn)  | 对数阶    |
| O(n)     | 线性阶    |
| O(nlogn) | nlogn阶  |
| O(n^2^)  | 平方阶    |
| O(n^3^)  | 立方阶    |
| O(2^n^)  | 指数阶    |
| O(n!)    | 阶乘阶    |

## 时间复杂度
评估执行程序所需的时间，可以估算程序对CPU的使用程度。下面分析常用的时间复杂度。
#### O(1)
常数阶，看代码：
```javascript
function func(n) {
  return n;
}
```
参数n传入值无论是1还是100000，执行次数都是一致的，和参数无关，性能是一样的，因此其时间复杂度表示为O(1)。
#### O(logn)
对数阶，看代码：
```javascript
function func(n) {
  var i = 1;
  while (i <= n) {
    i = i * 2;
  }
}
```
`while`语句的执行次数与参数值n的关系为 2^0^2^1^2^2^2^3^...2^k^...2^x^ = n, 只需知道x值是多少，就知道这段代码执行的次数了，通过 2^x^ = n 求解 x = log<sub>2</sub>n ，因此上面代码的时间复杂度为O(log<sub>2</sub>n)。因为算法复杂度描述的是算法执行所耗费的时间/空间随数据规模增长的变化趋势，所以常量、低阶、系数实际上对这种变化趋势不产生决定性影响，所以把底数2忽略掉，最终的结果是O(logn)。

#### O(n)
线性阶，看代码：
```javascript
function getSum(n) {
  var sum = 0;
  for (var i = 0; i < n; i++) {
    sum += i;
  }
  return sum;
}
```
由于for循环的次数等于参数n的数值，所以以上代码的时间复杂度是O(n)。

#### O(nlogn)
nlogn阶，看代码：
```javascript
function funcA(n) {
  var i = 1;
  while (i <= n) {
    i = i * 2;
  }
}

function getSum(n) {
  var sum = 0;
  for (var i = 0; i < n; i++) {
    sum += funcA(n);
  }
  return sum;
}
```
由前面的分析可知`funcA(n)`和`getSum(n)`的时间复杂度分别为O(logn)和O(n)，因此上面代码的时间复杂度为O(nlogn)。

#### O(n^2^)
平方阶，看代码：
```javascript
function getSum(n) {
  var sum = 0;
  for (var i = 0; i < n; i++) {
    for (var j = 0; j < n; j++) {
      sum += i + j;
    }
  }
  return sum;
}
```
第一个for循环执行次数是n，第二个执行次数是n，由于两个for循环是内嵌，所以这段代码循环执行次数是n^2^，因此时间复杂度为O(n^2^)。
#### O(n^3^)
立方阶，看代码：
```javascript
function getSum(n) {
  var sum = 0;
  for (var i = 0; i < n; i++) {
    for (var j = 0; j < n; j++) {
      for (var k = 0; k < n; k++) {
        sum += i + j + k;
      }
    }
  }
  return sum;
}
```
这段代码由3个for循环内嵌组成，每个for循环执行次数是n, 总共执行n^3^次，因此时间复杂度为O(n^3^)。

## 空间复杂度
评估执行程序所需的存储空间，可估算出程序对内存的使用程度。下面分析常用的空间复杂度。
#### O(1)
常数阶，看代码
```javascript
function func(n) {
  var i = 1;  // 第A行
  while (i <= n) {
    i = i * 2;
  }
}
```
第A行代码，申请了一个空间储存变量i，其他代码没有占用更多的空间了，所以上面的代码空间复杂度为O(1)。
#### O(n)
线性阶，看代码：
```javascript
function func(n) {
  var arr = [];  // 第A行
  for (var i = 0; i < n; i++) {
    arr.push(i);
  }
  return arr;
}
```
第A行代码，申请了一个空间储存变量arr，for循环内每执行一次便往数组追加一个元素，数组长度也随之变化，最终数组的长度为参数值n。该段代码占用的空间大小随参数n增大而增大，因此空间复杂度为O(n)。
#### O(n^2^)
平方阶，看代码：
```javascript
function func(n) {
  var arr = [];  // 第A行
  for (var i = 0; i < n; i++) {
    for (var j = 0; j < n; j++) {
      arr.push(i + j);  // 第B行
    }
  }
  return arr;
}
```
第A行代码，申请了一个空间储存变量arr，第B行代码的执行次数为n^2^，因此arr数组的长度为n^2^，因此空间复杂度为O(n^2^)。

## 总结
本文总结了如何使用大O表示法来描述时间复杂度和空间复杂度。