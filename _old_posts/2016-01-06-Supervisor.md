---
layout: post
title:  "Erlang Supervisor"
date:   2016-01-06
lastchang: 2016-01-06
categories: Erlang
---

## Supervisor
Supervisor是OTP为了做Restarter实现，总而言之就是一个医生，检测哪些进程死了，需要重启。
Supervisor支持很多种策略，策略的复杂度很让人头疼

one_for_one     	如果子进程终止，只有这个进程会被重启, 这种模式下，supervisor的各个子进程是无关的
one_for_all         如果子进程终止，所有子进程都会被重启, 这个的使用例子回头查找一下
rest_for_one        如果子进程终止，在这个进程重启后，其他所有子进程会被重启
simple_one_for_one  简化one_for_one，所有子进程都动态添加同一种进程的实例

### one_for_one VS simple_one_for_one
one_for_one 用一个列表储存了所有要启动的子进程，并按顺序启动。而simple_one_for_one 用一个进程字典定义所有子进程。所以当一个监督者拥有很多子进程时，遇到进程崩溃，simple_one_for_one 效率会快很多。
simple_one_for_one型监督者只能启动一种子进程，但却可以启动任意多个。它所有的子进程都是运行时动态添加的，监督者本身在启动时不会启动任何子进程。
one for one 启动时，会同时启动一个子进程
simple one for one 启动时，supervisor 并不会启动任何子进程，所有的子进程都是通过 supervisor:start_child(Sup, List) 来动态添加的

simple one for one it's to be used when you want to dynamically add them to the supervisor, rather than having them started statically.



supervisor感觉本身支持多个category，时间就是一开始策略中的定义好的那个List,每个List元素是一个Category，但是感觉不支持在start_child的时候动态修改或者添加策略，此时还是添加一个新的supervisor比较适合, 但是感觉比较麻烦，不能添加新的策略，好好研究一下，也许可以。添加策略后，直接




### Simple Supervisor

## FAQ
查找one_for_all的例子和真正的使用场景
整理一个supervisor下面挂载多个supervisor的例子
Riak对Supervisor的修改
看一下RabbitMQ Riak Ejabbered 等产品对supervisor的使用技巧
supervisor会不会成为瓶颈吧，我是指单进程倒是MsgBox跪了, 感觉这儿也不应该成为瓶颈，因为是监控的进程，如果成为了瓶颈，系统都快跪成什么样子了啊
