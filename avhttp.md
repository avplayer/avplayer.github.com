---
layout: index
title: avhttp by avplayer.org 社区
github: https://github.com/avplayer/avhttp
---

avhttp HTTP异步并发下载库
=======

# 起源


问题起源于 microcai　和　jack 的一次谈话。他们注意到了　HTTP 多线程下载模式，其实本质上不过是向服务器发起了多个TCP连接。使用一个线程一样能完成这样的工作——只要他们使用的是异步方式进行的。boost.asio 是一个非常优秀的异步网络库，要是能基于 asio 开发，就能实现单线程并发下载。得益于asio的良好架构，如果单线程性能不足的时候，只需要简单的开启多个线程跑　asio::io_service::run() 即可。这样是进行多线程还是单线程都可以由用户灵活的控制。


# 引线

虽然　jack 和　microcai 意识到了一个基于　asio 的并发　HTTP 下载库的重要性。但是真正导致他们动手的原因却是一个女人　——　猫是也。

猫据说希望开发一个某客户端，需要用到多线程HTTP下载功能，于是向社区列位大牛求救。　microcai 告诉她，jack在一份*私有*项目里实现过一个多线程　HTTP 下载代码，找他要一下， jack 说不定能剥离出来给猫用。

jack　说剥离太麻烦了，反正一直有个想用　asio 重写一个　HTTP 并发下载库的想法，不如重复发明一下轮子，用　asio 的方式彻底重写一个满意的HTTP库。

恰逢　avbot 的　WebQQ 协议需要　HTTP 库，但是基于 asio 的　HTTP库目前只有urdl (也是asio作者的大作，可惜已经不维护了)可以用，于是勉强用了这个早就停止开发的urdl，而且修修补补才勉强凑合使用。

重写一个真正合用的基于asio的异步HTTP库确实能帮助avbot项目，于是microcai也赞成jack的决定。了解microcai脾气的人都知道，microcai是一个坚定的反轮子党——一切重复发明轮子的行为都要被他批判。

# 命名

通过讨论，决定将库的名称命名为avhttp。av是avplayer.org社区项目的标志性前缀，http表示他是一个用于支持HTTP协议的库。

# 设计

avhttp由2大部分组成：avhttp::http_stream 和avhttp::multi_download。

* avhttp::http_stream是一个异步HTTP实现。每个http_stream对象支持一个HTTP会话。这个http_stream也是 avbot 所需要的。
* avhttp::multi_download 利用http_stream进行HTTP访问，内部通过调度逻辑组合，将复杂的多TCP下载逻辑都封装起来。

另外包含一些支持类，比如　avhttp::url 用于解析URI字符串，avhtt::request_opts 用户设置HTTP请求头。


# 实现

jack 承当了主要的编码工作。microcai懒人只负责骂jack，尤其是他作出了错误的技术决定的时候。
当　http_stream　接近完成的时候, microcai 以迅雷不及掩耳之速度将 avbot 移植到 avhttp　上。并利用变态的腾讯服务器做了第一个测试。找到了许多意料之外的bug。这个bug故事"恐怕要消失在历史了"。


# 使用

avhttp 是HeaderOnly　的库，使用的时候不需要编译，也不用添加库。只需要 #include <avhttp.hpp>　即可开始享受

TO BE CONTINED ...

