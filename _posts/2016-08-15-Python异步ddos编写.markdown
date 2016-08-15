---
layout: post
title: Python异步ddos编写
tagline: "ddos\_asyncio"
category: null
tags: []
published: true

---
#0x01 为什么要使用恊程 Coroutines

Python中支持多线程,也支持多进程.用Py来进行多线程编程大家都明白有GIL的问题,但多进程也同样有模型复杂,变量同步等问题.恊程就是一种特别的多线程例子,基于异步事件驱动,可以让单线程程序在等待IO时去完成另外其他的任务,从而达到异步并发的效果.


```
Coroutines可以做到的一些效果:


result = yield from future – 停止当前的coroutine直到future完成,future可以是一个函数,也可以是一段语句或者exception.

result = yield from coroutine – 
唤醒另一个coroutine直到它完成或者raise出一个exception.

return expression – 将计算结果通过yield from传回给原函数.

raise exception – 传回一个 exception.
```
