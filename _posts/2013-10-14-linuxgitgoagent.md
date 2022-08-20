---
layout: post
title: "linux下设置git使用goagent代理"
description: ""
category: 
tags: [git]
---
{% include JB/setup %}
## 设置git使用代理

[statoverflow上的讨论](http://stackoverflow.com/questions/783811/getting-git-to-work-with-a-proxy-server)

    git config --global http.proxy http://localhost:8087

## 配置ca-certificates

由于github现在使用https方式，我们还需要做一步：

    sudo cp ~/Goagent/local/CA.crt /usr/share/ca-certificates/goagent.crt
    sudo chmod a+r /usr/share/ca-certificates/goagent.crt
    sudo dpkg-reconfigure ca-certificates

其中第一行的CA.crt的路径视你goagent的安装地址而定。

最后一个命令会打开图形界面，使用**空格键**勾选goagent就可以了。
