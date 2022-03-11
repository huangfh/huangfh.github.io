---
layout: post
read_time: true
show_date: true
title: "常用算法总结"
date: 2021-04-02 12:32:20 -0600
img: posts/20210402/post7-header.webp
tags: [algorithm]
author: Armando Maynez
description: "ELI5: what is a neural network."
toc: yes
---
## 排序：
冒泡 插入 选择 交互 快排 希尔排序

## 递归


## 动态规划
自顶向下： 大的变成小的
自底向上： 小的集成大的

## 链表
查找，增加，删除

## 二叉树
遍历（前中后，递归），查找，删除，插入（有序）
平衡二叉树（红黑树）
任意节点的左右子树深度差不超过1。 调整时，分为左旋和右旋。深度logn，提升查找效率。

## 堆：
大顶堆，小顶堆。所有的元素都比左右子树的大或者小。
如果构建堆？


## 红黑树和哈希表的对比
哈希表如何解决冲突
顺序移
开方移
再哈希
两个队列模拟栈；两个栈模拟队列

## 线程和进程的区别
1. 进程包括各种资源
2. 线程是任务调度的最小单位。
3. 线程的调度与切换比进程快很多
4. 一个进程可以包含多个线程。
5. CPU密集型代码(各种循环处理、计算等等)：使用多进程。IO密集型代码(文件处理、网络爬虫等)：使用多线程
开放地址；链式地址；公共溢出区，再哈希
场景：有一个应用会经常创建、删除节点对象，如何优化。（节点池）
非递归实现树的后序遍历

5，实现一个快速排序
https://github.com/anarkh/test/blob/main/arithmetic/quick-sort.js

6，防抖和节流的概念，手动实现相应函数
https://github.com/anarkh/test/blob/main/utils/debounce-throttle.js



