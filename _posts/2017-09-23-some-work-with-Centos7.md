---
layout: post
title: "Hi, CentOS."
author: "Dervean"
---
很久很久之前就想认认真真地玩一玩Linux系统，但一直以来都有些杂七杂八的事情阻碍我(￢︿̫̿￢☆)，折腾了一下午Linux系统，有种一见如故的感觉！

#第一次接触Linux
在大学的时候就折腾过Linux系统，当时还是因为要学Hadoop才想学这玩意，第一次是用的VMware装的ubuntu12.04版本的Linux，因为电脑配置渣，4G内存，加上VMware吃内存吃的紧，在虚拟机上完全跑不动啊，而且当时校园网又渣，那体验简直糟糕突破天际。后来又折腾双系统，倒也装成功了，最后不小心改了主引导块，两个系统一起报废，当时倒是把我吓得不轻，唯一好的体验就是又装了一次WIN7系统（当时觉得装一个系统是个很有成就感的事情）。这让我对Linux系统产生了心理阴影，好长时间不想接触这玩意。

#重新拾起学习的兴趣
今年6月份想自己DIY一台台式机，研究各种硬件配置和比较性价比，最后在7月份组了一台式，装了WIN10系统，想着不能两个都搞WIN系统，便考虑在笔记本上装个Linux学着玩玩，网上对比了一下各种Linux系统，Ubuntu发行速度太快，UI界面吃内存吃的紧，而且搞开发的貌似都更倾向于选Red Hat，于是最后我选择使用CentOS 7，不过装了Linux快一个多月了都没怎么碰这玩意。

#契机
这两天老板要求助教要在可视化课上做展示，给大伙介绍可视化工具如何使用，我负责介绍[Vega](https://vega.github.io/vega/)这个virtualization grammar，展示需要使用电脑操作IDE一步步演示项目如何建立使用，我这还装着Linux呢> <，没办法了，赶鸭子上架，得赶快把Linux熟悉了才好。

#一下午的折腾
早上十点左右开始，我与OS的相爱相杀便正式开始（￣︶￣）↗。

##装chrome浏览器
因为谷歌停止了对CentOS的支持，所以得自己找网上的资源。(步骤参照[博客](https://www.tecmint.com/install-google-chrome-on-redhat-centos-fedora-linux/))
{% highlight js %}
yum update google-chrome-stable
{% endhighlight %}

###Step 1 Enable Google YUM repository
Create a file called **/etc/yum.repos.d/google-chrome.repo** and add the following lines of code to it.
{% highlight js %}
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
{% endhighlight %}
###Step 2 Installing Chrome Web Browser
check whether the latest version available
{% highlight js %}
yum info google-chrome-stable
{% endhighlight %}
install
{% highlight js %}
yum install google-chrome-stable
{% endhighlight %}
###Step 3: Starting Chrome Web Browser
{% highlight js %}
google-chrome &
{% endhighlight %}

最后，作为新时代的四有新人也需要科学[上网](https://laod.cn/hosts/2017-google-hosts.html)，下载完最新的host文件，直接copy到**/etc/hosts**，然后**log out log in**即可(为了刷新DNS缓存我费了不少功夫，但最后怎么折腾都不行，发现还是直接注销最方便)。

##装webstorm
这里主要研究了一下Linux目录结构，因为一开始我不知道程序该装到哪个文件夹里去，不过我看到网上一个[博客](http://blog.csdn.net/aqxin/article/details/48324377)，里面把Linux文件夹和Windows做对比，便觉得豁然开朗。
{% highlight js %}
/usr：系统级的目录，可以理解为C:/Windows/
/usr/lib，理解为C:/Windows/System32
/usr/local：用户级的程序目录，可以理解为C:/Progrem Files/
/opt：用户级的程序目录，可以理解为D:/Software
/usr/src：系统级的源码目录
/usr/local/src：用户级的源码目录
{% endhighlight %}

1. 从[官网](http://www.jetbrains.com/webstorm/download/#section=linux)上下载包。
2. 解压
{% highlight js %}
tar xvzf ~/Downloads/WebStorm\*.tar.gz -C /tmp/
{% endhighlight %}
3. 移动
{% highlight js %}
sudo mv /tmp/WebStorm\* /opt/Webstorm
{% endhighlight %}
4. 加软连接
{% highlight js %}
sudo ln -s /opt/WebStorm/bin/webstorm.sh /usr/local/bin/webstorm
{% endhighlight %}
5. 启动
{% highlight js %}
webstorm
{% endhighlight %}

##装nodejs
从[官网](https://nodejs.org/en/download/)下载最新的nodejs包，然后直接解压塞到**/opt/node**里，然后需要修改环境变量(NODE_HOME)，我在这里栽了个跟头，我把环境变量写进**/etc/profile**文件里，可是每次关闭shell再重打开就怎么也用不了**node**和**npm**。原来修改环境变量也有三种方式。
1. 修改**/etc/profile**：每次打开shell需要执行**source /etc/profile**环境变量才生效，关闭shell即作废。
2. 修改**/etc/environment**:设置整个系统的环境，与登录用户无关，即不需要每次**source /etc/environment**。
3. 修改**/etc/bashrc**：为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取。
最后我还是选择了**1**，多练习敲这个命令，以后再改。

##vega demo
[Vega](https://vega.github.io/vega/)里有个模块[vega-embed](https://github.com/vega/vega-embed),使用这个模块可以把Vega嵌入到网页里，
{% highlight js %}
vega.embed('#vis', spec)
{% endhighlight %}
需要**npm install**一下，可是我这里报了许多错误，从网上找解决方案，找了许久也没什么头绪，便也不管了，最后**npm run build**竟然也成功了，可能有种东西就叫做“玄学”。

![placeholder](https://github.com/Dervean/dervean.github.io/tree/master/images/vega.png "unemployment in U.S.A.")

##换GNOME主题
不得不说，CentOS的桌面太丑太丑太丑了(虽然占用资源少)，促使我要换了桌面♪(´▽｀)。本来是打算在主题网站上下载一个主题然后安装，但貌似GNOME主题是分成几部分安装的，我只是安装了一个theme，然后界面不完全，最后安装了gnome-theme，直接进入不了GNOME界面，不过这里还是把错误的做法记录下来以后再看吧。

<ins>错误的做法(ノ｀Д)ノ：</ins>

1. 先查一下GNOME版本：
{% highlight js %}
gnome-about --gnome-version
{% endhighlight %}
显示没有**gnome-about**这个命令，再试试...

{% highlight js %}
gnome-session --version
{% endhighlight %}
可以。是GNOME3

2. 去[GNOME主题网站](https://www.opendesktop.org/s/Gnome/browse/)上下载一个主题，我选择了T4G_3.0_theme。

3. 下载主题后解压，把整个解压后的文件夹放入**/usr/share/themes**，然后使用**gnome-tweak-tool**加载主题
{% highlight js %}
mv T4G_3.0_theme /usr/share/themes
gnome-tweak-tool
{% endhighlight %}

4. 加载主题后发现Centos底部任务栏和上部任务栏挡住了，美感大跌，忍不了，得把它们给删了。

* 删除Centos底部任务栏：
{% highlight js %}
cd /usr/share/gnome-shell/
备份
cp -r /usr/share/gnome-shell/extensions/  /usr/share/gnome-shell/extensions.backup/
删除任务栏
rm -fr /usr/share/gnome-shell/extensions/window-list@gnome-shell-extensions.gcampax.github.com
删除位置栏
rm -fr  /usr/share/gnome-shell/extensions/places-menu@gnome-shell-extensions.gcampax.github.com
reboot
{% endhighlight %}

* 隐藏顶栏：

备份
{% highlight js %}
cd /usr/share/gnome-shell
cp -r /usr/share/gnome-shell/modes/   /usr/share/gnome-shell/modes.backup/
cp  -r /usr/share/gnome-shell/theme/  /usr/share/gnome-shell/theme.backup/
{% endhighlight %}

修改**classic.json**
{% highlight js %}
cd modes/
vi classic.json
修改如下
 "panel":{ "left": [],
    "center": [],
     "right": []
   }
{% endhighlight %}

修改**gnome-classic.css**
{% highlight js %}
cd ../theme/
vi gnome-classic.css
修改如下
\#panel {

    background-color: #e9e9e9;

    background-gradient-direction: vertical;

    background-gradient-end: #d0d0d0;
    border-top-color: #666; /* we don't supportnon-uniform border-colors and
                               use the top bordercolor for any border, so we
                               need to set iteven if all we want is a bottom
                               border */
    border-bottom: 1px solid #666;
    app-icon-bottom-clip: 0px;
    **color: transparent;**
    /* hrm, still no multipoint gradients
    background-image: linear-gradient(left,rgba(255, 255, 255, 0),rgba(255, 255, 255, 1) 50%，rgba(255, 255, 255, 0)) !important;*/
   }
{% endhighlight %}

修改**gnome-shell.css**
{% highlight js %}
修改如下
第一处
\#panel {
    background-color:transparent;
    font-weight: bold;
    height: 0px;
   }
第二处
 .panel-logo-icon {
  padding-right: .4em;
  icon-size: 1px;
  }
{% endhighlight %}

5. 界面还是不完整，把**T4G_3.0_theme**里面的**gnome-theme**单独放在**/usr/share/gnome-shell/**中试试
{% highlight js %}
rm -r /usr/share/gnome-shell/theme
mv /usr/share/themes/T4G_3.0_theme/gnome-theme /usr/share/gnome-shell/theme
{% endhighlight %}
按**alt+F2**可以调出对话框，再按**r**即可重启GNOME......Game Over，GNOME无法重新打开

<ins>挽救：</ins>

6. 在登陆用户界面，直接**ctrl+alt+F2**调出**startx**，也就是直接在shell中敲命令，通过之前备份的**/usr/share/gnome-shell/theme.backup**重新恢复
{% highlight js %}
mv /usr/share/gnome-shell/theme.backup /usr/share/gnome-shell/theme
{% endhighlight %}
重新启动，即可恢复GNOME

<ins>换一种姿势：</ins>

7. 既然从主题上面安装吃了亏，另辟蹊径就好了，我还可以用软件(**cairo-dock**)改造（￣︶￣）↗，不过有好多工具都找不到，顺便改一下yum源吧
{% endhighlight %}
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
{% highlight js %}

8. 装**cairo-dock**，这让我体会到了Ubuntu的好处╯︿╰，能一键apt-get的好处真不是盖的，我想上[官网](https://pkgs.org/download/cairo-dock)上下载，光是那些一层一层的依赖包就让我感觉毛骨悚然。。果断选择其他方法（￣︶￣）↗　。
**Nux Dextop**是类似CentOS、RHEL、ScientificLinux的第三方RPM仓库（比如：Ardour，Shutter等等）。目前，Nux Dextop对CentOS/RHEL 6|7可用。
{% endhighlight %}
下载
wget http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
安装依赖库
sudo yum -y install epel-release
安装
rpm -ivh nux-dextop-release-0-5.el7.nux.noarch.rpm
sudo yum install cairo-dock
{% highlight js %}

由于**Nux Dextop**仓库可能会与其他第三方库有冲突，比如（Repoforge和ATrpms）。
所以，建议默认情况下不启用**Nux Dextop**仓库。(参考[在CentOS或RHEL上安装Nux Dextop仓库](http://www.jianshu.com/p/86d16189832e))
(这个我倒没在意，就不改了~)
{% endhighlight %}
打开/etc/yum.repos.d/nux-dextop.repo，将"enabled=1" 修改为 "enabled=0"。
$ sudo vi /etc/yum.repos.d/nux-dextop.repo
当需要使用Nux Dextop仓库时，显式启用仓库。
$ sudo yum --enablerepo=nux-dextop install <package-name>
{% highlight js %}

9. 界面上还会有底部任务栏和顶部横条，参照**第4步**改造一下就好了

10. 点击cairo-dock图标即可

![placeholder](https://github.com/Dervean/dervean.github.io/tree/master/images/Screenshot_from_2017-09-24_02-30-38.png.png "Screenshot")
