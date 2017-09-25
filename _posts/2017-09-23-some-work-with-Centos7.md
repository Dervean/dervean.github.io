---
layout: post
title: "Hi, CentOS."
author: Dervean
description: "First come up with Linux."
categories: [linux]
tags: [linux]
redirect_from:
  - /2017/09/23/
---

一直以来都想认认真真地玩一玩Linux系统，但总有杂七杂八的事情阻碍我，今天折腾了一下午Linux系统，竟然有种一见如故的感觉！

---

## 初识Linux
  在大学的时候就折腾过Linux系统，当时想要学Hadoop，便用VMware装的ubuntu12.04版本的Linux.

  因为电脑配置不好，4G内存，在虚拟机上完全跑不动，而且校园网网速感人，那糟糕体验简直酸爽.

  后来又折腾双系统，倒也装成功了，最后不小心改了主引导块，两个系统一起报废，当时把我吓得不轻，唯一好的体验就是又装了一次WIN7系统（当时觉得装一个系统是个很有成就感的事情）.

不过这让我对Linux系统产生了心理阴影，好长时间不想接触这玩意.

## 重新连接
  今年7月份自己DIY了一台式机，装了WIN10.

  想着不能两个都搞WINDOWS，便考虑在笔记本上装个Linux学着玩玩.

  网上对比了一下各种Linux系统，说是Ubuntu发行速度太快，UI界面吃内存吃的紧，而且搞开发的貌似都更倾向于选Red Hat，加上我之前的心理阴影，于是最后选择使用CentOS 7，但是装了Linux快一个多月了都没怎么碰.

## 契机
  这两天老板要求助教要在可视化课上做展示，给大伙介绍可视化工具如何使用，我负责介绍[Vega](https://vega.github.io/vega/)这个virtualization grammar，展示需要使用电脑操作IDE一步步演示项目如何建立使用.

  正好借这个机会好好折腾一下Linux系统.

  早上十点左右开始，我与OS的相爱相杀便正式开始（￣︶￣）↗.

## Chrome浏览器
因为谷歌停止了对CentOS的支持，所以得找网上的资源.(步骤参照[博客](https://www.tecmint.com/install-google-chrome-on-redhat-centos-fedora-linux/))

    yum update google-chrome-stable

- **Step 1**&nbsp; &nbsp;Enable Google YUM repository.Create a file called **/etc/yum.repos.d/google-chrome.repo** and add the following lines of code to it.

    [google-chrome]
    name=google-chrome
    baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
    enabled=1
    gpgcheck=1
    gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub


- **Step 2**&nbsp; &nbsp;Installing Chrome Web Browser.

    //check whether the latest version available.
    yum info google-chrome-stable
    //install.
    yum install google-chrome-stable

- **Step 3**&nbsp; &nbsp;Starting Chrome Web Browser.

    google-chrome &

作为新时代的四有新人也需要[科学上网](https://laod.cn/hosts/2017-google-hosts.html)，下载完最新的host文件，直接copy到**/etc/hosts**，然后**log out log in**即可(为了刷新DNS缓存费了我不少功夫，但怎么折腾都不行，最后发现还是直接注销最方便).

## Webstorm

这里主要研究了一下Linux目录结构，因为我不知道安装程序该装到哪个文件夹里去，不过当我看到网上一个[博客](http://blog.csdn.net/aqxin/article/details/48324377)，里面把Linux文件夹和Windows做对比，便觉得豁然开朗.

    /usr：系统级的目录，可以理解为C:/Windows/
    /usr/lib，理解为C:/Windows/System32
    /usr/local：用户级的程序目录，可以理解为C:/Progrem Files/
    /opt：用户级的程序目录，可以理解为D:/Software
    /usr/src：系统级的源码目录
    /usr/local/src：用户级的源码目录

- **Step 1**&nbsp; &nbsp;Download package from [webstorm](http://www.jetbrains.com/webstorm/download/#section=linux).

- **Step 2**&nbsp; &nbsp;Unzip the package.

    tar xvzf ~/Downloads/WebStorm-2017.2.4.tar.gz -C /tmp/

- **Step 3**&nbsp; &nbsp;Move the directory to /opt/.

    sudo mv /tmp/WebStorm-2017.2.4 /opt/Webstorm

- **Step 4**&nbsp; &nbsp;Add soft link.

    sudo ln -s /opt/WebStorm/bin/webstorm.sh /usr/local/bin/webstorm

- **Step 5**&nbsp; &nbsp;Start webstorm.

    webstorm

## Nodejs
从[官网](https://nodejs.org/en/download/)下载最新的nodejs包，然后直接解压到**/opt/node**里，修改环境变量(NODE_HOME).

我在这里栽了个跟头，我把环境变量写进**/etc/profile**文件里，可是每次关闭shell再重打开就怎么也用不了**node**和**npm**.

原来修改环境变量也有三种方式：

1. 修改**/etc/profile**：每次打开shell需要执行**source /etc/profile**环境变量才生效，关闭shell即作废.
2. 修改**/etc/environment**:设置整个系统的环境，与登录用户无关，即不需要每次**source /etc/environment**.
3. 修改**/etc/bashrc**：为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取.

最后我还是选择了**方法1**，多练习敲这个命令，以后再改.

## Vega demo
[Vega](https://vega.github.io/vega/)里有个模块[vega-embed](https://github.com/vega/vega-embed),使用这个模块可以把Vega嵌入到网页里.

需要**npm install**，可是却报了许多错误，我从网上找解决方案，找了许久也没什么头绪，便也不管了，最后**npm run build**竟然也成功了，(￣▽￣)"，可能有种东西就叫做“玄学”.

![smiley](/images/vega_employment_USA.png "unemployment in U.S.A.")

## GNOME主题

不得不说，CentOS的桌面太丑太丑太丑了(虽然占用资源少)，促使我要换了桌面♪(´▽｀).

本打算在网站上下载一个主题再装，但貌似GNOME主题需要分成几部分安装，我只安装了一个theme，然后界面不全，接着安装了gnome-theme，结果直接进了GNOME界面，不过这里还是把错误的做法记录下来，等以后再回来瞅瞅吧.

### 错误的做法

- **Step 1**&nbsp; &nbsp;先查一下GNOME版本.
//显示没有gnome-about这个命令
gnome-about --gnome-version
//可以使用，GNOME3
gnome-session --version

- **Step 2**&nbsp; &nbsp;去[GNOME主题网站](https://www.opendesktop.org/s/Gnome/browse/)上下载一个主题，我选择了T4G_3.0_theme.

- **Step 3**&nbsp; &nbsp;下载主题后解压，把整个解压后的文件夹放入**/usr/share/themes**，使用**gnome-tweak-tool**加载主题.

    mv T4G_3.0_theme /usr/share/themes
    gnome-tweak-tool
  
* Step 4&nbsp; &nbsp;加载主题后发现Centos底部任务栏和上部任务栏挡住了，美感大跌，忍不了，得把它们给删了.

  * Delete the bottom bar.

>cd /usr/share/gnome-shell/
>
>//备份
>
>cp -r /usr/share/gnome-shell/extensions/  /usr/share/gnome-shell/extensions.backup/
>
>//删除任务栏
>
>rm -fr /usr/share/gnome-shell/extensions/window-list@gnome-shell-extensions.gcampax.github.com
>
>//删除位置栏
>
>rm -fr  /usr/share/gnome-shell/extensions/places-menu@gnome-shell-extensions.gcampax.github.com

  * Hide the top bar.<br>
    
>//备份
>
>cd /usr/share/gnome-shell
>
>cp -r /usr/share/gnome-shell/modes/   /usr/share/gnome-shell/modes.backup/
>
>cp  -r /usr/share/gnome-shell/theme/  /usr/share/gnome-shell/theme.backup/

>//修改 /usr/share/gnome-shell/modes/classic.json
>
>"panel":{ "left": [],
>
>  "center": [],
>
>   "right": []
>
> }

修改 /usr/share/gnome-shell/theme/gnome-classic.css

    #panel {
        background-color: #e9e9e9;
        background-gradient-direction: vertical;
        background-gradient-end: #d0d0d0;
        border-top-color: #666; /* we don't supportnon-uniform border-colors and
                                   use the top bordercolor for any border, so we
                                   need to set iteven if all we want is a bottom
                                   border */
        border-bottom: 1px solid #666;
        app-icon-bottom-clip: 0px;
        color: transparent;
        /* hrm, still no multipoint gradients
        background-image: linear-gradient(left,rgba(255, 255, 255, 0),rgba(255, 255, 255, 1) 50%，rgba(255, 255, 255, 0)) !important;*/
       }

修改 /usr/share/gnome-shell/theme/gnome-shell.css

    //第一处
    #panel {
        background-color:transparent;
        font-weight: bold;
        height: 0px;
       }
    //第二处
     .panel-logo-icon {
      padding-right: .4em;
      icon-size: 1px;
      }


- **Step 5**&nbsp; &nbsp;界面还是不全，把**T4G_3.0_theme**里面的**gnome-theme**单独放在**/usr/share/gnome-shell/**中.

    rm -r /usr/share/gnome-shell/theme
    mv /usr/share/themes/T4G_3.0_theme/gnome-theme /usr/share/gnome-shell/theme

按**alt+F2**可以调出对话框，再按**r**即可重启GNOME......Game Over，GNOME无法重新打开

### 挽救

- **Step 6**&nbsp; &nbsp;在登陆用户界面，直接**ctrl+alt+F2**调出**startx**，也就是直接在shell中敲命令，通过之前备份的**/usr/share/gnome-shell/theme.backup**重新恢复.

    mv /usr/share/gnome-shell/theme.backup /usr/share/gnome-shell/theme

重新启动，即可恢复GNOME

### 换一种姿势

- **Step 7**&nbsp; &nbsp;既然从主题上面安装吃了亏，另辟蹊径就好了，我还可以用软件(**cairo-dock**)改造（￣︶￣）↗，不过有好多工具都找不到，顺便改一下yum源.

    cd /etc/yum.repos.d 
    备份旧的配置文件
    mv CentOS-Base.repo CentOS-Base.repo.bak 
    下载阿里源的文件
    wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
    添加EPEL源
    sudo wget -P /etc/yum.repos.d/ http://mirrors.aliyun.com/repo/epel-7.repo 
    清理缓存 
    yum clean all 
    重新生成缓存
    yum makecache

- **Step 8**&nbsp; &nbsp;装**cairo-dock**，这让我深切体会到了Ubuntu的好处，很希望能一键apt-get啊，我想着去[cairo-dock](https://pkgs.org/download/cairo-dock)上下载，光是那些一层一层的依赖包就让我感觉毛骨悚然.果断选择其他方法（￣︶￣）↗.

**Nux Dextop**是类似CentOS、RHEL、ScientificLinux的第三方RPM仓库（比如：Ardour，Shutter等等）.

    //下载
    wget http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
    //安装依赖库
    sudo yum -y install epel-release
    //安装
    rpm -ivh nux-dextop-release-0-5.el7.nux.noarch.rpm
    sudo yum install cairo-dock

由于**Nux Dextop**仓库可能会与其他第三方库有冲突，比如（Repoforge和ATrpms）.

所以，建议默认情况下不启用**Nux Dextop**仓库(参考[在CentOS或RHEL上安装Nux Dextop仓库](http://www.jianshu.com/p/86d16189832e))，这个我倒没在意，就没改.

    //打开/etc/yum.repos.d/nux-dextop.repo，将"enabled=1" 修改为 "enabled=0".
    $ sudo vi /etc/yum.repos.d/nux-dextop.repo
    //当需要使用Nux Dextop仓库时，显式启用仓库.
    $ sudo yum --enablerepo=nux-dextop install <package-name>

- **Step 9**&nbsp; &nbsp;界面上还会有底部任务栏和顶部横条，参照**Step 4**改造一下就好了.

- **Step 10**&nbsp; &nbsp;点击cairo-dock图标即可.

![smiley](/images/Screenshot_from_2017-09-24_02-30-38.png "Screenshot")