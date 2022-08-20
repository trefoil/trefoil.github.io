---
layout: post
title: "Ubuntu 13.04下解决Rhythmbox乱码问题"
description: ""
category: 
tags: [ubuntu]
---
{% include JB/setup %}
虽然非常想念Windows下的foobar2000，但是正在尝试着迁移到Linux环境下的我，听歌也想试试原生的音乐播放器，于是尝试着从文件夹中导入歌曲。导入之后，就可以整个Playlist播放了。

老问题出现了：导入的歌曲的标签，由于有中文，出现了乱码。作为条件反射，我默默打开google搜索对策。很不错，很快搜索到了解决方案：[好心情Blog-Rhythmbox乱码快速解决](http://www.softbunny.net/post/rhythmbox_encoding.shtml).

按照该文中的解决方案，只需建一个链接为

    env GST_ID3_TAG_ENCODING=GBK rhythmbox %U

的Application launcher就可以了。

至于如何建立launcher，请参考[How to create Application launcher and add icon to Unity in Ubuntu 13.04 / 12.10 / 12.04](http://www.howopensource.com/2012/10/create-application-launcher-add-icon-to-unity-ubuntu-12-10/)。

Good luck!

