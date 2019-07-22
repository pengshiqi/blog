---
title: Shadowsocks 原理
date: 2017-08-25 15:59:14
tags: Network
categories: Network
---

据说明年3月将大量封禁 VPN ，于是想研究一下 Shadowsocks 的原理，看看是否还能使用。

<!-- more -->

Shadowsocks 是目前最好的科学上网方式，**它的流量经过加密，所以没有流量特征，不会被GFW阻断**！

之前写过[如何搭建 Shadowsocks 的教程](https://pengshiqi.github.io/2016/09/21/build-shadowsocks-VPN/)，这次写一写 Shadowsocks 背后的原理。

Shadowsocks 不同于 VPN(Virtual Private Network)，它是 Proxy ，Github 上的介绍是： 

* ** A fast tunnel proxy that helps you bypass firewalls ** 

## Long long ago...

在很久很久以前，我们访问各种网站都是简单而直接的，用户的请求通过互联网发送到服务提供方，服务提供方直接将信息反馈给用户。

![icon](shadowsocks2.png)

## When evil comes...

然后有一天，GFW 就出现了，它像一个收过路费的强盗一样夹在用户(client)和服务(server)之间，每当用户需要信息，都需要经过GFW，GFW会将它不喜欢的内容过滤掉。于是当用户触发GFW的过滤规则的时候，就会收到`Connection Reset`这样的相应内容，而无法接收到正常的请求内容。

![icon](shadowsocks3.png)


## SSH tunnel

于是人们想到了利用境外服务器代理的方式来绕过GFW的过滤，其中包含了各种HTTP代理服务、Socks服务、VPN服务...其中`SSH Tunnel`方式比较有代表性。

1) 首先用户和境外服务器基于 SSH 建立起一条加密的通道。

2) 3) 用户通过建立起的隧道进行代理，通过SSH server向真实的服务器发起请求。

4) 5) 服务器通过SSH server，再通过创建好的隧道返回给用户。

![icon](shadowsocks4.png)

由于SSH本身就是基于RSA加密技术的，所以GFW无法从数据传输的过程中进行关键词分析，避免了被重置链接的问题。但由于创建隧道和数据传输的过程中，SSH本身的特征很明显，所以GFW一度通过分析连接的特征进行干扰，导致SSH存在被定向干扰的问题。

## Shadowsocks

于是`clowwindy`同学分享并开源了他的解决方案。

简单地理解，Shadowsocks就是将原来SSH创建的Socks5协议拆开成Server端和Client端，所以下面这个原理图基本上和利用SSH Tunnel类似。

1) 6) 客户端发出的请求基于Socks5协议跟SS local端进行通讯，由于这个SS local一般是本机或路由器局域网的其他机器，不经过GFW，所以解决了上面被GFW通过特征分析进行干扰的问题。

2) 5) SS local 和 SS server两端通过多种可选的加密方法进行通讯，**经过GFW的时候是常规的TCP包，没有明显的特征码而且GFW也无法对通讯数据进行解密！**

3) 4) SS server将收到的加密数据进行解密，还原原来的请求，再发送到用户需要访问的服务器，获取响应原路返回。

![icon](shadowsocks5.png)

## Summary

因此，Shadowsocks的优点在于它通过流量混淆隐秘解决了GFW通过分析流量特征从而干扰的问题，这是它优于SSH和VPN翻墙的地方（但VPN更注重加密安全性）。缺点也依然明显，需要一点点技术和资源（墙外VPS服务器）来搭建Shadowsocks服务。

## Reference

* [写给非专业人士看的 Shadowsocks 简介](https://vc2tea.com/whats-shadowsocks/)

* [ShadowSocks的翻墙原理](https://tumutanzi.com/archives/13005)
