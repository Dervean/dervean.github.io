---
layout: post
title: "MySQL and Python installing"
author: Dervean
description: "MySQL and Python installing"
categories: [linux]
tags: [linux,mysql,python]
redirect_from:
  - /2017/09/27/
---

装MySQL笔记.

---

## MySQL

以下内容参考[在 CentOS7 上安装 MySQL5.7](http://www.linuxidc.com/Linux/2016-06/132676.htm)

1.本想着离线安装，但出现了一些问题，索性还是直接**YUM**来的更加简便.

>//查看系统中是否已安装 MySQL 服务
>
>rpm -qa \| grep mysql
>
>//如果**已安装**则删除 MySQL 及其依赖的包
>
>yum -y remove mysql-libs.x86_64
>
>//下载 mysql57-community-release-el7-11.noarch.rpm 的 YUM 源
>
>wget http://repo.mysql.com/mysql57-community-release-el7-11.noarch.rpm
>
>//安装 mysql57-community-release-el7-8.noarch.rpm
>
>rpm -ivh mysql57-community-release-el7-8.noarch.rpm
>
>//安装 MySQL
>
>yum install mysql-server

2.安装完毕后，在  /var/log/mysqld.log 文件中会自动生成一个随机的密码

>grep "password" /var/log/mysqld.log
>
>使用获得的密码登陆
>
>mysql -u root -p [password]

3.登录之后会要求立即修改密码,我本想设置个简单的数字密码，可是报错("Your password does not satisfy the current policy requirements")，才知道MySQL现在对密码有要求，不仅要达到最小长度，还要必须含有数字，小写或大写字母，特殊字符.
我只是用于学习使用，就不用搞这么麻烦了，把对密码的要求改一下就行.

>//修改validate_password_policy参数的值,这个必须改
>
>set global validate_password_policy=0;
>
>//密码长度最小值
>
>set global validate_password_length=6;
>
>//密码中特殊字符的长度
>
>set global validate_password_special_char_count=0;
>
>//密码中大小字母的长度
>
>set global validate_password_mixed_case_count=0;
>
>//重置密码
>
>set password = password('[password]'); 

4.修改字符编码方式

>//查看 MySQL 的字符集
>
>show variables like '%character%';

设置 MySQL 的字符集为 UTF-8，打开 /etc 目录下的 my.cnf 文件（此文件是 MySQL 的主配置文件）.

>/etc/my.cnf

在 [mysqld] 前添加如下代码

>[client]
>
>default-character-set=utf8

在 [mysqld] 后添加如下代码：

>character_set_server=utf8

5.MySQL操作.

- 显示所有的用户

>SELECT User, Host, Password FROM mysql.user;

- 启动 MySQL 服务

>service mysqld start

- 关闭 MySQL 服务

>service mysqld stop

- 重启 MySQL 服务

>service mysqld restart

- 查看 MySQL 的状态

>service mysqld status

- /var/lib/mysql 存放数据库文件的目录

- /var/log 目录下的 mysqld.log 文件记录 MySQL 的日志

- 默认端口号为 3306

- 如果忘记密码时，可用如下方法重置

>service mysqld stop
>
>mysqld_safe --user=root --skip-grant-tables --skip-networking &
>
>mysql -u root
>
>mysql> use mysql;
>
>mysql> update user set password=password("new_password") where user="root";
>
>mysql> flush privileges;


## Python

>//更新gcc
>
>yum -y install gcc
>
>//下载软件包
>
>wget https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tgz
>
>//解压
>
>tar -zxvf Python-3.6.2.tgz
>
>//进入解压目录
>
> cd Python-3.6.2
>
>//创建安装目录
>
>mkdir /opt/Python
>
>//编译
>
>./configure –prefix=/opt/Python
>
>//安装
>
>make && make install
>
>//建立软连接
>
>ln -s /opt/Python/bin/python3.6  /usr/bin/python3



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

## Yum

1.提示**"Another app is currently holding the yum lock; waiting for it to exit..."**,yum在锁定状态中.

>//强制关掉yum进程：
>
>rm -f /var/run/yum.pid

2.安装的chrome浏览器莫名其妙的没了，我怀疑是自动更新的缘故.

>//CentOS7关闭自动下载更新
>
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