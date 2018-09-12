---
title: ca
date: 2018-09-03 19:41:44
tags: 随笔
---


互联网架构中，web-server接入一般使用nginx来做反向代理，实施负载均衡。整个架构分三层：
上游调用层，一般是browser或者APP
中间反向代理层，nginx
下游真实接入集群，web-server，常见web-server的有tomcat，apache
 
整个访问过程为：
browser向daojia.com发起请求
DNS服务器将daojia.com解析为外网IP(1.2.3.4)
browser通过外网IP(1.2.3.4)访问nginx
nginx实施负载均衡策略，常见策略有轮询，随机，IP-hash等
nginx将请求转发给内网IP(192.168.0.1)的web-server
 
由于http短连接，以及web应用无状态的特性，理论上任何一个http请求落在任意一台web-server都应该得到正常处理（如果必须落在一台，说明架构不合理，不能水平扩展）。
 
问题来了，tcp是有状态的连接，客户端和服务端一旦建立连接，一个client发起的请求必须落在同一台tcp-server上，此时如何做负载均衡，如何保证水平扩展呢？
 <!--more-->
二、单机法tcp-server

单个tcp-server显然是可以保证请求一致性：
client向tcp.daojia.com发起tcp请求
DNS服务器将tcp.daojia.com解析为外网IP(1.2.3.4)
client通过外网IP(1.2.3.4)向tcp-server发起请求
 
方案的缺点？
无法保证高可用。
 
三、集群法tcp-server