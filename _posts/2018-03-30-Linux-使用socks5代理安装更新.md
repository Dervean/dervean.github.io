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
   \begin{array}{rl}
   \text{local = 192.168.1.0/255.255.255.0} & \text{#local 表示不使用 socks 代理的网络} \\
   \text{local = 127.0.0.0/255.0.0.0} & \\
   \text{server = 127.0.0.1  & \text{#socks 服务器 IP} \\
   \text{server_type = 5     & \text{#socks 版本} \\
   \text{server_port = 1080  & \text{＃端口}
   \end{array}
   $$

3. 使用方法: **sudo tsocks apt-get update** 即在命令前加上 tsocks。












