---
layout: post
read_time: true
show_date: true
title:  事件循环
date:   2021-02-10 13:32:20 -0600
description: eventloop
img: posts/20210210/eventStack.png
tags: [eventloop]
author: huangfh
github: amaynez/GameOfLife/
toc: true
---
## 事件循环机制，宏任务，微任务

## 前提知识：
- 浏览器多线程，js单线程。
- node的异步靠底层的多进程实现。
- chrome 每个页卡都有一个渲染进程。
- js是单线程，为了协调事件，用户交互，脚本，ui渲染，网络处理，防止主线程阻塞，事件循环方案应运而生，js每次执行完同步代码，就进入eventloop阶段。

![浏览器线程](./assets/img/posts/20210210/chromiumThread.png "浏览器线程")
<small>浏览器线程</small>

## 定义：
```
JS引擎常驻于内存中，等待宿主将JS代码或函数传递给它。
也就是等待宿主环境分配宏观任务，反复等待 - 执行即为事件循环。
```

```
ES6 规范中，microtask 称为 jobs，macrotask 称为 task
宏任务是由宿主发起的，而微任务由JavaScript自身发起。
```


## 浏览器EventLoop：
![浏览器线程](./assets/img/posts/20210210/eventLoop.png "浏览器线程")


1. 主线程每次执行时，先看看要执行的是同步任务，还是异步的API
2. 同步任务就继续执行，一直执行完
3. 遇到异步API就将它交给对应的异步线程，自己继续执行同步任务
4. 异步线程执行异步API，执行完后，将异步回调事件放入事件队列上
5. 主线程手上的同步任务干完后就来事件队列看看有没有任务
6. 主线程发现事件队列有任务，执行每个宏任务前先执行完所有的微任务。执行完微任务渲染，执行完每个宏任务渲染。同一层级，先宏任务，宏任务时创建的微任务一次性执行完，然后下一个宏任务。

7. 主线程不断循环上述流程

## Node.js的Event Loop
Node.js是运行在服务端的js，虽然他也用到了V8引擎，但是他的服务目的和环境不同，导致了他API与原生JS有些区别，他的Event Loop还要处理一些I/O，比如新的网络连接等，所以与浏览器Event Loop也是不一样的。Node的Event Loop是分阶段的，如下图所示：
![浏览器线程](./assets/img/posts/20210210/nodejsEventLoop.png "浏览器线程")

1. timers: 执行 setTimeoutsetInterval的回调
2. pending callbacks: 执行延迟到下一个循环迭代的 I/O 回调
3. idle, prepare: 仅系统内部使用
4. poll: 检索新的 I/O 事件;执行与 I/O 相关的回调。事实上除了其他几个阶段处理的事情，其他几乎所有的异步都在这个阶段处理。
5. check: setImmediate
6. close callbacks: 一些关闭的回调函数，如：socket.on('close', ...)

![浏览器线程](./assets/img/posts/20210210/eventStack.png "浏览器线程")



|  | 宏任务（macrotask）      | 微任务（microtask） |
| 谁发起的 | 宿主（Node、浏览器） | JS引擎 |
| 谁先运行 | 后运行 | 先运行 |
| 会触发新一轮Tick吗 | 会 | 不会 |

具体事件	1. script (可以理解为外层同步代码)
2. setTimeout/setInterval
3. UI rendering/UI事件
4. postMessage，MessageChannel
5. setImmediate，I/O（Node.js）
	1. Promise
2. MutaionObserver
3. Object.observe（已废弃；Proxy 对象替代）
4. process.nextTick（Node.js）



谁先运行	后运行	先运行
会触发新一轮Tick吗	会	不会

区分好哪写是宏任务，哪写是微任务，然后node11之前循环是宏任务列表全部执行完毕再执行微任务列表全部任务，依次循环，11之后和浏览器一样，执行一个宏任务-》清空微任务列表-》一个宏任务这样循环
