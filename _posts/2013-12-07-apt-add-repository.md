---
layout: post
title: "如何设置代理使用apt-add-repository"
description: ""
category: 
tags: [ubuntu, skype, zotero]
---
{% include JB/setup %}
>* This will become a table of contents (this text will be scraped).
>{:toc}

## 问题的起源

我目前上国际网时，需要使用代理服务器。`git`我已经调教好代理服务器的设置了，用chrome上网时用SwitchySharp插件也很方便。老大难就是`apt-add-repository`了。

碰巧我现在想要解决两个问题：

1. 给Ubuntu 13.04装一个在状态栏里能显示skype图标的补丁。详情见[这里](http://www.webupd8.org/2013/05/how-to-get-systray-whitelist-back-in.html)。

2. 安装zotero-standalone。有一个很方便的ppa，见[这里](http://askubuntu.com/questions/332109/how-to-install-zotero-in-ubuntu)。

所以狠下心，想把这个问题解决掉。

## 问题的解决

查到[这个帖子](http://askubuntu.com/questions/53146/how-do-i-get-add-apt-repository-to-work-through-a-proxy)的王牌答案后，才发现非常简单：

    export http_proxy=http://proxy:port
    export https_proxy=http://proxy:port
    sudo -E apt-add-repository ppa:linaro-maintainers/toolchain

其中，`sudo -E`是让sudo使用环境变量，具体可以见[这里](https://wiki.archlinux.org/index.php/Sudo_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29#.E7.8E.AF.E5.A2.83.E5.8F.98.E9.87.8F)。

如果你的代理服务器需要身份验证的话，可以参考[这里](http://askubuntu.com/questions/60217/apt-get-update-with-an-in-password-error)的例子：

    export http_proxy=http://deepak:Deepak%40123@12.1.1.1:3128

这里使用了转义符`%40`来表示密码中实际出现的`@`。
