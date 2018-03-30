---
layout: post
title: "使用socks5代理安装更新"
author: Dervean
description: "初识 Hadoop"
categories: [linux]
tags: [linux]
redirect_from:
  - /2018/03/30/
---

今天遇到 sudo apt-get update 某些无法访问的问题，原来 apt 走的是 http协议，所以想到了可以使用 socks 代理，也可以让安装更加快速。

---

我使用的是 tsocks:

1. sudo apt-get install tsocks

2. vi /etc/tsocks.conf

   $$
   \text{local = 192.168.1.0/255.255.255.0  #local 表示不使用 socks 代理的网络} \\
   \text{local = 127.0.0.0/255.0.0.0} \\
   \text{server = 127.0.0.1   #socks 服务器 IP} \\
   \text{server_type = 5      #socks 版本} \\
   \text{server_port = 1080   ＃端口}
   $$

3. 使用方法: **sudo tsocks apt-get update** 即在命令前加上 tsocks。












