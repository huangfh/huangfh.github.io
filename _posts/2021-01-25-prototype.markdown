---
layout: post
read_time: true
show_date: true
title:  原型链
date:   2021-01-25 13:32:20 -0600
description: 原型连原理
img: posts/20210125/image.png
tags: [prototype]
author: huangfh
github:  huangfh
toc: yes
mathjax: yes
---
## 为什么要有原型链？

其他高级语言，都有基于类（ class ）的继承机制。而JavaScript本身没有 class 实现，即便是在 ES2015/ES6 中引入了 class 关键字，但那也只是语法糖。javascript依旧是基于原型链来实现继承的。

## 分清概念：constructor， __proto__，prototype
1.constructor、 __proto__属性是对象所独有的；
2. prototype属性是函数独有的
3. 上面说过js中函数也是对象的一种，那么函数同样也有属性constructor、 __proto__

prototype ：
设计之初就是为了实现继承，让由特定函数创建的所有实例共享属性和方法，不需要为多个实例创建属性方法，而是将属性方法创建在构造函数的原型对象上（prototype）。那些不需要共享的才创建在构造函数中。原型属性，原型方法

Parent.prototype.name = "我是原型属性，所有实例都可以读取到我";

__proto__
相当于通往prototype（“琅琊福地”）唯一的路（指针）
让“徒弟”、“徒孙” 们找到自己“师父”、“师父的师父” 提供给自己的方法和属性



```javascript
var Parent = function(){
  this.a = 1;
  this.b = 2;
}
//定义一个函数，那它只是一个普通的函数，下面我们让这个函数变得不普通
var p1 = new Parent();

//这时这个Parent就不是普通的函数了，它现在是一个构造函数。因为通过new关键字调用了它
//创建了一个Parent构造函数的实例 p1
Parent.prototype.b = 3;
Parent.prototype.c = 4;
var p2 = Object.create(p1); // p2继承自p1

//此时p2的原型链如下：
//{}--proto-> {a:1,b:2}--proto->{b:3,c:4}--proto->object.prototype--proto->null

```



```javascript
p1.__proto__ === Parent.prototype; // true


Parent.prototype.__proto__ === Object.prototype; //true
```


一层层的往上找，就是原型链。

constructor属性
是让“徒弟”、“徒孙” 们知道是谁创造了自己，这里可不是“师父”啊

## 使用不同的方法来创建对象和生成原型链
### 使用语法结构创建的对象
```javascript
var o = {a: 1};

// o 这个对象继承了 Object.prototype 上面的所有属性
// o 自身没有名为 hasOwnProperty 的属性
// hasOwnProperty 是 Object.prototype 的属性
// 因此 o 继承了 Object.prototype 的 hasOwnProperty
// Object.prototype 的原型为 null
// 原型链如下:
// o ---> Object.prototype ---> null

var a = ["yo", "whadup", "?"];

// 数组都继承于 Array.prototype
// (Array.prototype 中包含 indexOf, forEach 等方法)
// 原型链如下:
// a ---> Array.prototype ---> Object.prototype ---> null

function f(){
  return 2;
}

// 函数都继承于 Function.prototype
// (Function.prototype 中包含 call, bind等方法)
// 原型链如下:
// f ---> Function.prototype ---> Object.prototype ---> null
```

### 使用构造器创建的对象

在 JavaScript 中，构造器其实就是一个普通的函数。当使用 new 操作符 来作用这个函数时，它就可以被称为构造方法（构造函数）。

```javascript
function Graph() {
  this.vertices = [];
  this.edges = [];
}

Graph.prototype = {
  addVertex: function(v){
    this.vertices.push(v);
  }
};

var g = new Graph();
// g 是生成的对象，他的自身属性有 'vertices' 和 'edges'。
// 在 g 被实例化时，g.[[Prototype]] 指向了 Graph.prototype。
```

### 使用 Object.create 创建的对象
ECMAScript 5 中引入了一个新方法：Object.create()。可以调用这个方法来创建一个新对象。新对象的原型就是调用 create 方法时传入的第一个参数：

```javascript
var a = {a: 1};
// a ---> Object.prototype ---> null

var b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a); // 1 (继承而来)

var c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

var d = Object.create(null);
// d ---> null
console.log(d.hasOwnProperty); // undefined, 因为d没有继承Object.prototype
```
### 使用 class 关键字创建的对象

ECMAScript6 引入了一套新的关键字用来实现 class。使用基于类语言的开发人员会对这些结构感到熟悉，但它们是不同的。JavaScript 仍然基于原型。这些新的关键字包括 class, constructor，static，extends 和 super。
```javascript 
"use strict";

class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength);
  }
  get area() {
    return this.height * this.width;
  }
  set sideLength(newLength) {
    this.height = newLength;
    this.width = newLength;
  }
}

var square = new Square(2);
```

## 性能
在原型链上查找属性比较耗时，对性能有副作用，这在性能要求苛刻的情况下很重要。另外，试图访问不存在的属性时会遍历整个原型链。

遍历对象的属性时，原型链上的每个可枚举属性都会被枚举出来。要检查对象是否具有自己定义的属性，而不是其原型链上的某个属性，则必须使用所有对象从 Object.prototype 继承的 hasOwnProperty (en-US) 方法。下面给出一个具体的例子来说明它：
```javascript
console.log(g.hasOwnProperty('vertices'));
// true

console.log(g.hasOwnProperty('nope'));
// false

console.log(g.hasOwnProperty('addVertex'));
// false

console.log(g.__proto__.hasOwnProperty('addVertex'));
// true
```
hasOwnProperty (en-US) 是 JavaScript 中唯一一个处理属性并且不会遍历原型链的方法。（译者注：原文如此。另一种这样的方法：Object.keys()）

注意：检查属性是否为 undefined 是不能够检查其是否存在的。该属性可能已存在，但其值恰好被设置成了 undefined。

## 总结：
说到继承，JavaScript 只有一种结构：对象。
每个实例对象（ object ）都有一个私有属性（称之为 __proto__ ）指向它的构造函数的原型对象（prototype ）。该原型对象也有一个自己的原型对象( __proto__ ) 。
该原型对象也有一个自己的原型对象( __proto__ ) ，层层向上直到一个对象的原型对象为 null。根据定义，null 没有原型，并作为这个原型链中的最后一个环节。

![](../assets/img/posts/20210125/image.png)

## hasOwnProperty，hasProperty，
hasOwnProperty: 对象自身属性，不包含从原型链上继承的属性。
for in 遍历所有可枚举属性。

## 实例方法 和 静态方法
- say是静态方法，没在原型链上，不能被继承。
- getName是可以被实例访问。

```javascript
var Person=function(){
  this.run = function (){
    console.log('I can run');
  }
};

Person.say=function(){
    console.log('I am a Person,I can say.')
};

Person.prototype.getName=function(name){
    console.log('My name is '+name);
}

Person.say()//静态方法. 

var person = new Person();
person.getName('huangfh')//实例方法
person.run()
person.say() // undefined



```



