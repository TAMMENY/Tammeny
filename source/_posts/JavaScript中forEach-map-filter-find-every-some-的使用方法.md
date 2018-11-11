---
title: 'JavaScript中forEach(),map(),filter(),find(),every(),some()的使用方法'
categories: JavaScript
toc: true
date: 2018-09-07 22:57:12
tags: 
- Array
---

`Array`对象提供了多个方法方便我们遍历数据，使得代码更加简洁、易读。题中六个方法在日常工作中经常会用到，每个方法的用法和效果看起来差不多，所以容易混淆，借工作之余将它们的用法总结一下。

## 简述
- `Array.prototype.forEach()`
- `Array.prototype.map()`
- `Array.prototype.filter()`
- `Array.prototype.find()`
- `Array.prototype.every()`
- `Array.prototype.some()`

上述方法的参数都是一致的，通用语法如下：
```javascript
/**
* @param {String} method 遍历方法名称
* @param {Function} callback 为每个元素执行的函数，有三个参数：
*   @param {String} currentValue 当前元素
*   @param {Number} index 当前元素的索引
*   @param {Array} array 当前数组
* @param {Object} thisArg 执行回调时要使用的值（即引用对象）
*/
arr[method](function callback(currentValue[, index[, array]]) {
    // 你的迭代器
}[, thisArg]);
```
不同`method`遍历方法有不同的返回结果：
1. `forEach()` 返回`undefined`，只是针对每个元素执行回调函数；
2. `map()` 返回一个新数组，元素由每个回调函数执行的结果组成；
3. `filter()` 返回一个新数组，元素由原数组中的满足回调函数中指定条件的元素组成；
4. `find()` 返回首个符合回调函数中指定条件的元素，如无则返回 `undefined`；
5. `every()` 返回`true`或`false`，检测数组里所有元素是否符合指定条件；
6. `some()` 返回`true`或`false`，检测数组里是否有元素符合指定条件；

## 使用案例
光看文字描述过于抽象，来一波代码你就明白的了。
#### 1. Array.prototype.forEach()
该方法不会返回任何值，或者说返回`undefined`，对数组的每个元素执行一次回调函数。
```javascript
var array1 = ['a', 'b', 'c'];
array1.forEach(function(element, index) {
  console.log(`第${index+1}个元素：`, element);
});
// 第1个元素: "a"
// 第2个元素: "b"
// 第3个元素: "c"
```
#### 2. Array.prototype.map()
和`forEach()`不同的是，`map()`返回一个新的数组，由数组遍历时执行回调函数的返回值组成。
```javascript
var array1 = [1, 2, 3];
var map1 = array1.map(function(item){
    return item*2;
});
console.info(map1);
// [2, 4, 6];
```
#### 3. Array.prototype.filter()
对于数组中的每个元素，`filter()`都会调用回调 函数一次（采用升序索引顺序），返回一个由原数组中满足回调函数里指定条件的元素组成的新数组。不为数组中缺少的元素调用该回调函数。
```javascript
var array1 = [1, 2, 3];
var filter1 = array1.filter(function(item){
    return item>=2;
});
console.info(filter1);
// [2, 3]
```
#### 4. Array.prototype.find()
`find()` 返回数组中首个符合回调函数中指定条件的元素，当已找到符合条件的元素，之后的值不会再调用回调函数。如果没有符合条件的元素则返回 `undefined`。
```javascript
var array1 = [1, 2, 3];
var find1 = array1.find(function(item){
    return item>=2;
});
console.info(find1);
// 2;
```
#### 5. Array.prototype.every()
用于检测数组所有元素是否都符合回调函数内指定的条件。如果数组中检测到有一个元素不满足，则整个表达式返回 `false`，且剩余的元素不会再进行检测。如果所有元素都满足条件，则返回 `true`。
```javascript
var array1 = [1, 2, 3];
var every1 = array1.every(function(item){
    return item>2;
});
console.info(every1);
// false;
```
#### 6. Array.prototype.some()
和`every()`的功能相似，不同的是`some()`只要检测到有一个元素满足回调函数内指定的条件，则返回`true`，否则返回`false`。
```javascript
var array1 = [1, 2, 3];
var some1 = array1.some(function(item){
    return item>2;
});
console.info(some1);
// true;
```