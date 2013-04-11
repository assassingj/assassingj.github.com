---
layout: post
title: "golang写个简单的cmus歌词插件"
date: 2013-04-11 21:53
comments: true
categories: 
- technology
- golang
---

一直用cmus听歌，终于发现有时候想看看歌词，但cmus下没有合适的插件，所以自己先写个最最简单的，后续有兴趣的时候可以改进
目前已实现：  

1.  检测cmus运行状态   
2.  获取cmus当时正在play的歌曲meta信息    
3.  根据歌手和歌曲名去歌词服务器query歌词,用的是geci.me 这个API    
4.  打印第一匹配的歌词    

待实现:

1.  终端下的termbox  
2.  同一首歌的cache  
3.  自主search 选择歌词  
4.  歌词滚动？？   
4.  ...


<script src="https://gist.github.com/assassingj/5363551.js"></script>

