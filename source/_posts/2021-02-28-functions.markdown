---
layout: post
read_time: true
show_date: true
title:  数组/对象/字符串常用方法
date:   2021-02-28 12:32:20 -0600
description: Single neuron perceptron that classifies elements learning quite quickly.
tags: [array, object ]
author: Armando Maynez
github: amaynez/GenericNeuralNetwork/
toc: yes
---
## 数组常用方法
- 创建：new Array ,  fill(0,2,4) 0-2 填充4  [...array1,...array2]
- 遍历/过滤：forEach map reduce  reverse sort flat  filter  every some
- 元素增加/减少：concat push pop  shifit，unshift, splice, slice(2,4)  join(',')
- 查找：  find findIndex includes indexOf     ：findIndex (callback) vs indexOf(item)
- 其他：keys，values，enteries （返回array iterator)） flat

## string常用方法
- 创建： raw？  "abc".repeat(2)  //“abcabc”
- 遍历/过滤：slice   split toLowerCase toString toUpperCase  valueOf
- 元素增加/减少：concat，padStart，padEnd  trim trimStart trimEnd
- 查找：charAt,charCodeAt，startsWith endsWIth startsWith includes(substr, position 区分大小写) indexOf lastIndexOf  match  matchAll  replace replaceAll search  substring

## Object常用方法
 assign keys values entries
需要全部看的就是String，Array，Object的这三个对象的api，原型链，new，Promis这些

## 拷贝
对象：object.assign（只拷贝一层），ES6扩展运算符  json.parse , 递归
数组：slice，concat，扩展符，

