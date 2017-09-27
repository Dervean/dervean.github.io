---
layout: post
title: "MySQL installed on CentOS"
author: Dervean
description: "MySQL installed"
categories: [linux]
tags: [linux,mysql,wifi,battery,yum,cmake]
redirect_from:
  - /2017/09/27/
---

装MySQL笔记.

---

## Wifi
使用命令行连接WiFi.

>//查询可用的无线网卡
>
>iw dev
>
>//启用无线卡,'name'指网卡号
>
>ip link set [name] 
>
>//查看无线网卡连接情况
>
>ip link set [name] up
>
>//查看所有可用的无线网络信号
>
>iw [name] scan \| grep SSID
>
>//连接无线网,'admin'指WiFi信号id,'password'指密码
>
>wpa_supplicant -B -i [name] -c <(wpa_passphrase "[admin]" "[password]")
>
>//分配IP地址（通过dhclient控制网卡进行网络操作获取IP）
>
>dhclient wlp2s0
>
>//查看无线网卡地址信息
>
>ip addr show [name]


## Battery
查看电源电量信息.(百分比)

>cat /sys/class/power_supply/BAT1/capacity

## Cmake

>//安装编译源码所需的工具和库
>
>yum install gcc gcc-c++ ncurses-devel perl
>
>//下载cmake
>
>wget https://cmake.org/files/v3.3/cmake-3.9.1.tar.gz
>
>//解压
>
>tar zxvf cmake-3.9.1.tar.gz
>
>cd cmake-3.9.1
>
>//编译并安装
>
>./configure
>
>make
>
>make install
>
>//移动
>
>mv cmake-3.3.2  /opt/cmake
>
>//设置全局变量(PATH=/opt/cmake/bin:$PATH)
>
>vi /etc/profile
>
>source /etc/profile

## MySQL

- 本想着离线安装，但出现了一些问题，索性还是直接**YUM**来的更加简便.

>//查看系统中是否已安装 MySQL 服务：
>
>rpm -qa \| grep mysql
>
>//如果**已安装**则删除 MySQL 及其依赖的包：
>
>yum -y remove mysql-libs.x86_64
>
>//下载 mysql57-community-release-el7-11.noarch.rpm 的 YUM 源：
>
>wget http://repo.mysql.com/mysql57-community-release-el7-11.noarch.rpm
>
>//安装 mysql57-community-release-el7-8.noarch.rpm：
>
>rpm -ivh mysql57-community-release-el7-8.noarch.rpm
>
>//安装 MySQL
>
>yum install mysql-server
>
>//重置密码
>
>mysql -u root
>
>mysql > use mysql;
>
>mysql > update user set password=password('123456') where user='root';
>
>mysql > exit;

- 显示所有的用户.

>SELECT User, Host, Password FROM mysql.user;

## Yum

1.提示**"Another app is currently holding the yum lock; waiting for it to exit..."**,yum在锁定状态中.

>//强制关掉yum进程：
>
>rm -f /var/run/yum.pid

2.安装的chrome浏览器莫名其妙的没了，我怀疑是自动更新的缘故.

//CentOS7关闭自动下载更新

>systemctl start crond
>
>yum -y install cronie
>
>yum -y install yum-cron
>
>systemctl start yum-cron
>
>sudo vi /etc/yum/yum-cron.conf
>
> - update_messages = no
>
> - download_updates = no


## 用户与用户组

- 用户列表文件：/etc/passwd
- 用户组列表文件：/etc/group

- 查看系统中有哪些用户：cut -d : -f 1 /etc/passwd
- 查看可以登录系统的用户：cat /etc/passwd \| grep -v /sbin/nologin \| cut -d : -f 1
- 查看用户操作：w命令(需要root权限)
- 查看某一用户：w 用户名
- 查看登录用户：who
- 查看用户登录历史记录：last





