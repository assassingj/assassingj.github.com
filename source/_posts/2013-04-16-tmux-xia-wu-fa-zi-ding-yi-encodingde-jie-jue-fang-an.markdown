---
layout: post
title: "tmux 下无法自定义encoding的解决方案"
date: 2013-04-16 10:56
comments: true
categories: 
- technology
- linux
---

之前在国际站，使用utf-8是非常自然的事情,甚至无论中美所有服务器时区也按照标准的USA来设。到了ALIPAY后，everything changed... 代码，日志文件很统一地用了GBK编码,于是出现了tmux无法应付的场景..

开始我用iterm2新建了一套profile,初始化时指定character encoding为gb2312,可以满足需求，昨天将其改为luit了,这样不用新开tab

实施[luit](http://invisible-island.net/luit/)非常简单，唯一需要注意的是mac lion下可能会出现
>couldn't copy terminal setting

这样的error, 我download了luit的最新源码，自己compile出来是没这个问题的,在.zshrc中加入alias
<code>
    alias ssh2server='luit -encoding gbk ssh xxx@shterm.xxx.net'
</code>

以后上线上服务器直接ssh2server吧。
