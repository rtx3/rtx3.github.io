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



