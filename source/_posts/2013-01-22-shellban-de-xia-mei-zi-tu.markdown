---
layout: post
title: "shell版的下妹子图"
date: 2013-01-22 00:43
comments: true
categories: 
---

在osc上看到一篇python写的[下妹子图][1],试着改为一句shell,效果很不错:
<code>
for i in {1..700}; do curl -q "http://jandan.net/ooxx/page-$i#comments" 2>/dev/null| grep 'img src=.*.jpg.*' | sed 's/.*img src=\(.*.jpg\).*$/\1/' | tr '"' ' ' | xargs wget ; done
</code>


[1]: http://www.oschina.net/code/snippet_13769_17481 "下妹子图"


