---
layout: post
title: "Linux 应用系列之 Sublime Text"
author: Dervean
description: "ubuntu 16.04 LTS Sublime Text"
categories: [shortcut_keys]
tags: [shortcut_keys]
redirect_from:
  - /2018/05/16/
---

Sublime Text 3

---

# 安装

安装路径:

$$
\text{/opt/sublime_text}
$$

# 中文输入支持

* 建立 sublime_imfix.c 文件并输入下列代码:

~~~c
#include <gtk/gtkimcontext.h>
void gtk_im_context_set_client_window (GtkIMContext *context,
            GdkWindow    *window)
{
    GtkIMContextClass *klass;
    g_return_if_fail (GTK_IS_IM_CONTEXT (context));
    klass = GTK_IM_CONTEXT_GET_CLASS (context);
    if (klass->set_client_window)
        klass->set_client_window (context, window);
    g_object_set_data(G_OBJECT(context),"window",window);
    if(!GDK_IS_WINDOW (window))
        return;
    int width = gdk_window_get_width(window);
    int height = gdk_window_get_height(window);
    if(width != 0 && height !=0)
        gtk_im_context_focus_in(context);
}
~~~

* 生成共享库 .so 文件:

~~~
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
~~~

如果此时出现错误: 

$$
\text{sublime_imfix.c:1:30: fatal error: gtk/gtkimcontext.h: 没有那个文件或目录}
$$

则输入一下命令再重试:

~~~
sudo apt-get install libgtk2.0-dev
~~~

* 复制共享库文件到 Sublime Text 3 所在文件夹

~~~
sudo cp libsublime-imfix.so /opt/sublime_text/
~~~

* 配置共享库文件

修改 .desktop 文件:

(1) [Desktop Entry] 中:

$$
\text{Exec=/opt/sublime_text/sublime_text %F}
$$

改为

$$
\text{Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"}
$$

(2) [Desktop Action Window] 中:

$$
\text{Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text -n""}
$$

(3) [Desktop Action Document] 中:

$$
\text{Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text --command new_file"}
$$

#快捷键

|种类		|按键					|功能												|
|:---------:|:---------------------:|---------------------------------------------------|
|选择		|Ctrl+D					|选中光标所占的文本，继续操作则会选中下一个相同的文本			|
|			|Ctrl+L					|选中整行												|
|编辑		|Ctrl+Shift+D 			|制光标所在整行，插入到下一行								|
|			|Ctrl+/					|注释单行												|
|			|Ctrl+Shift+/			|注释多行												|
|搜索		|Ctrl+G					|打开搜索框，自动带 :，输入数字跳转到该行代码				|
|			|Ctrl+R					|打开搜索框，自动带 @，输入关键字，查找文件中的函数名			|
|			|Ctrl+P					|打开搜索框											|






















