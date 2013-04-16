---
layout: post
title: "将raspberry pi目录备份到dropbox"
date: 2013-04-16 14:14
comments: true
categories: 
- technology
- raspberry pi
- linux
---

上个周raspberry pi突然就无法引导启动了,看了dmesg的信息，大概是我修改了fstab文件引起的。所幸上面的文件我都rsync过一份到我的mac上，所以干脆重装os。

但这个事情教育我们backup很重要，我决定保险一点，让raspberry pi定时自动做backup，并将backup文件放到dropbox上.步骤有

###  打包指定的目录  
<code>
tar czvf xx.tar.gz ~/xxx
</code>   
我只备份了重要的目录， 如果想backup整个系统，可能需要   
<code>
cd /   
sudo tar czvfp xxx.tar.gz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys /
</code>    

###  上传打包文件至dropbox
懒得再研究dropbox的API了，此处我使用了开源的[dropbox_uploader](https://github.com/andreafabrizi/Dropbox-Uploader) ,github上的README很详尽,我需要做的是在dropbox上新建一个app,并取得api key和app secret即可

将上面两步用脚本包装一下,脚本如下
<script src="https://gist.github.com/assassingj/5393859.js"></script>


###  定时执行
这个借助crontab非常简单,crontab新建一条任务
<code>
0 12 * * * /home/pi/backup.sh
</code>

于是，每天12点就会自动backup目录并上传至dropbox上了,也不用担心哪天忘记rsync
