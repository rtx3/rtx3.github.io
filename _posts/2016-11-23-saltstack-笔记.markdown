---
layout: post
title: saltstack 笔记
tagline: "\_saltstack"
category: null
tags: []
published: true

---
通过 api 调用笔记

##event##


###base###
aptitude install postgresql-server-dev-9.4
通过 retner 调用 postgresql 需要以上基本库,aptitude 可以解决冲突

![salt pstgresql]https://docs.saltstack.com/en/latest/ref/returners/all/salt.returners.postgres_local_cache.html#module-salt.returners.postgres_local_cache

!调用returners.postgres_local_cache 

###ZeroMQ###


embeddabled networking library / concurrency framework
it's a socket-inspired API on which you do zmq_recv() and zmq_send(). But message processing rapidly becomes the central loop, and your application soon breaks down into a set of message processing tasks. It is elegant and natural. And it scales: each of these tasks maps to a node, and the nodes talk to each other across arbitrary transports. Two nodes in one process (node is a thread), two nodes on one box (node is a process), or two nodes on one network (node is a box)—it's all the same, with no application code changes.

salt使用ZeroMQ PUB/SUB 的模式来下发命令
指令分装：

'''
{'tgt_type': 'glob', 'jid': '', 'key': 'LCkViTMgqKBqb5ooG8kznznztLYPsWR1xdTYnAz9udkU9/Lla32yDvUmVKLPaUNSMtbWdBoQPIs=', 'tgt': '*', 'arg': [], 'fun': 'test.ping', 'kwargs': {'show_timeout': False}, 'cmd': 'publish', 'ret': '', 'user': 'root'}
'''

发送到4506命令端口->产生 jid->fire event -> 签名下发

minion 验证->执行->加密回传-> master 收集后反馈

###顺序##
tag:
salt/job/[jid]/new -> find jid -> salt/job/[jid]/ret
###minion 启动顺序###



