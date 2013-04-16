---
layout: post
title: "tmux使用心得小记"
date: 2012-10-25 13:55
comments: true
categories: 
- technology
- linux


---
##前言

有大屏幕后不用终端复用软件来提高效率简直就是暴殄天物, 之前一直用的是terminator终端来完成这个功能,但有一些局限性:
    1.mac下没有terminator,虽然有iterm之类替代,但非吾所愿,我要的是通用解决方案
    2.和X做了绑定, 纯命令行下无法使用terminator

tmux与GNU screen类似,但是来自openbsd, 使用它最直接的用处是可以切屏,当然好处远不止于此.下面是tmux的一些feature:

* A powerful, consistent, well-documented and easily scriptable command interface.
* A window may be split horizontally and vertically into panes.
* Panes can be freely moved and resized, or arranged into preset layouts.
* Support for UTF-8 and 256-colour terminals.
* Copy and paste with multiple buffers.
* Interactive menus to select windows, sessions or clients.
* Change the current window by searching for text in the target.
* Terminal locking, manually or after a timeout.
* A clean, easily extended, BSD-licensed codebase, under active development.

##安装
tmux的安装非常简单,ubuntu下   
<code>sudo aptitude install tmux</code>  
mac下ports或者brew都可以直接install,以brew为例  
<code>sudo brew install tmux</code>   
想自己编译可以从[官网][1] download源码  

##基本用法
最好的办法man,openbsd上也提供了一份[手册][2]
开启一个终端, 输入tmux命令. 基本上所有的操作都需要先按下prefix键(默认是ctrl+b),
比如  
1. 水平分割一个pane,先ctrl+b,再双引号  
2. 垂直分割一个pane,先ctrl+b, 再百分号  
3. 脱离当前会话,ctrl+b后 d, 此处的脱离是暂时离开,实际当前所有的工作场景都是保留的,并且在后台持续运行,后续可以通过tmux attach重新接入进来,有点nohup的感觉,所以你可以今天SSH上某台服务器,开个tmux,跑某个任务,然后注销这次SSH,明天再SSH上来attach上次的会话,看看任务是否跑完  

不习惯看英文的同学[此处][3]有一些基本的中文操作手册


##个性化折腾
生命不息,折腾不止.自己定制的才是最适合自己的,如此强大的工具要慢慢挖掘
1. 改prefix键, ctrl+b没手感,改为ctrl+h
修改~/.tmux.conf(如果没有则新建一个),新增如下内容:  
<code>
set -g prefix C-h
unbind-key C-b
</code>

2. 启动shell时自动进入tmux, 如果没有会话则新建一个,有则直接attach
在.zshrc(bash用户可能是.bashrc)的最后加入tmux就直接启动了,但要做些逻辑判断
最后的脚本如下:  
<code>
test -z "$TMUX" && (tmux attach || tmux new-session)
</code>

3. 定制状态栏, 想在状态栏上加点自己随时感兴趣的东西
因为是动态IP,平常时不时会要ifconfig一下,于是将IP固定显示在状态栏左边  
<code>
set -g status-left-ength 21
set -g status-left "#[bg=green,fg=white,bold]#(ifconfig | grep -A1 eth0 | tail -1 | awk '{print $2}' | awk -F: '{print $2}')"
</code>
开发的机器经常高load,可以将load放到状态栏右边,成天对着黑白terminal,怕忘记时间再在状态栏加上时间显示(一分钟refresh一次)  
<code>
set -g status-right "#[bg=green,fg=white,bold]#(cut -d ' ' -f 1-3 /proc/loadavg) ● #[bg=green,fg=white,bold]#(date +%H:%M)"
set -g status-interval 60
</code>
(其实在tmux中, 按下prefix键后 加t 会有个全屏的时钟出来)

4. 启动时初始化窗口
想骚包吗？想假装高手吗，想打开tmux就自动就分N个窗口吗？
在shell启动脚本中加入这些(替换之前的自启动脚本)   
<script src="https://gist.github.com/4058032.js?file=tmux-init.sh"></script>
使用以上脚本会开两个window，并把第2个window分割成3份，最上面跑着htop,左边有个vim，右边一个dstat，效果如下图   
<a href="http://www.flickr.com/photos/85525219@N04/8178078344/" title="all by 爱琳说, on Flickr"><img src="http://farm9.staticflickr.com/8488/8178078344_8c8ce9f1a5.jpg" width="500" height="313" alt="all"></a>   
具体需要启动什么，启动几个窗口，分割分几个pane, 都可以修改脚本定制   

5. 多pane同步
连了好几台服务器，想同时执行相同的命令，同时观察结果吗？在按下prefix键后按冒号进入command模式
输入   
<code>
setw synchronize-panes
</code>   
你输入的命令将在所有pane中出现执行   
<a href="http://www.flickr.com/photos/85525219@N04/8178095816/" title="syn-pane by 爱琳说, on Flickr"><img src="http://farm9.staticflickr.com/8486/8178095816_1841eca896.jpg" width="500" height="313" alt="syn-pane"></a>

[1]: http://tmux.sourceforge.net/  "tmux web"
[2]: http://www.openbsd.org/cgi-bin/man.cgi?query=tmux&sektion=1 "manual page"
[3]: https://wiki.freebsdchina.org/software/t/tmux ""
