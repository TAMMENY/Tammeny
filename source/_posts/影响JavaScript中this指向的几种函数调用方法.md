---
title: 影响JavaScript中this指向的几种函数调用方法
categories: JavaScript
toc: true
date: 2018-11-11 15:40:30
tags: 设计模式
---

## 前言
初学javascript总会对this指向感到疑惑，想要深入学习javascript，必须先理清楚和this相关的几个概念。javascript中this总是指向一个对象，但具体指向谁是在运行时根据函数执行环境动态绑定的，而并非函数被声明时的环境。除去不常用的with和eval的情况，具体到实际应用中，this指向大致可以分为以下4种。
## 作为对象的方法调用
当函数作为对象的方法被调用时，this指向该对象：
```javascript
var person = {
  name: 'twy',
  getName: function() {
    console.info(this === person);  // 输出true
    console.info(this.name);     // 输出twy
  }
}
person.getName();
```

## 作为普通函数调用
当函数作为普通的函数被调用时，非严格模式下this指向全局对象：
```javascript
function getName(){
  // 非严格模式
  console.info(this === window); // 浏览器环境下输出true
}
getName();
```
严格模式下this为undefined：
```javascript
function getName(){
  // 严格模式
  "use strict"
  console.info(this === window); // 输出false
}
getName();
```
## 构造器调用
当new一个对象时，构造器里的this指向new出来的这个对象：
```javascript
function person(){
  // 构造函数
  this.color = 'white';
}
var boy = new person();
console.info(boy.color);  // 输出white
```

## call或apply调用
用 `Function.prototype.apply` 或 `Function.prototype.call` 可以动态改变传入函数的this指向：
```javascript
// 声明一个父亲对象，getName方法返回父亲的名字
var father = {
  name: 'twy',
  getName: function(){
    return this.name;
  }
}
// 生命一个儿子对象，但是没有返回名字的功能
var child = {
  name: 'chy'
}
console.info(father.getName()); // 输出twy

// 使用call或apply将father.getName函数里this指向child
console.info(father.getName.call(child)); // 输出chy
console.info(father.getName.apply(child)); // 输出chy
```
下一篇文章我将重点介绍call和apply。
[理解JavaScript中的call-apply和bind方法](https://www.tangwenyong.com/2018/11/12/理解JavaScript中的call-apply和bind方法/)
## 最后
将this理解透彻，是一个jser必须要做的事情。