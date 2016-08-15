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


* result = yield from future – 停止当前的coroutine直到future完成,future可以是一个函数,也可以是一段语句或者exception.

* result = yield from coroutine – 
唤醒另一个coroutine直到它完成或者raise出一个exception.

* return expression – 将计算结果通过yield from传回给原函数.

* raise exception – 传回一个 exception.
```

一个官网的例子:


```
import asyncio
import datetime

@asyncio.coroutine
def display_date(loop):
    end_time = loop.time() + 5.0
    while True:
        print(datetime.datetime.now())
        if (loop.time() + 1.0) >= end_time:
            break
        yield from asyncio.sleep(1)

loop = asyncio.get_event_loop()
# Blocking call which returns when the display_date() coroutine is done
loop.run_until_complete(display_date(loop))
loop.close()
```
这段代码会在前五秒内打印出每秒的datetime.new(),主线程在执行loop之后一直在等待恊程完成,如果有更多的task,这个时候主线程就会去执行其他task直到这些任务全部完成.


##0x02 Chain Coroutine

```
import asyncio

@asyncio.coroutine
def compute(x, y):
    print("Compute %s + %s ..." % (x, y))
    yield from asyncio.sleep(1.0)
    return x + y

@asyncio.coroutine
def print_sum(x, y):
    result = yield from compute(x, y)
    print("%s + %s = %s" % (x, y, result))

loop = asyncio.get_event_loop()
loop.run_until_complete(print_sum(1, 2))
loop.close()
```
将一个coroutine和另一个coroutine用yield from联系起来后即形成了一个coroutine chaining.这两个coroutine其实运行与不同的线程之上,通过yield from来交替传值.

![python pic][2]


[1]:https://github.com/aosabook/500lines "500 lines"
[2]:https://docs.python.org/3.4/_images/tulip_coro.png "pic"

