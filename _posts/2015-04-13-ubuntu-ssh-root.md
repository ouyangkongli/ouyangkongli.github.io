---
layout: post 
keywords: mRemoteNG ssh root
description:
title: "利用mRemoteNG，使用root用户ssh登陆ubuntu"
categories: [tool]
tags: [tool, mRemoteNG, root, ssh]
group: archive
icon: file-alt
---
{% include oukongli/setup %}

windows上有许多优秀的远程登陆工具，mRemoteNG是其中一个，本人在尝试使用
'ssh root@192.168.x.x'  
时，得到错误信息  
`Permission denied`,  
可以确认的是ubuntu上端口已开，因为使用  
`ssh oukongli@192.168.x.x`  
是没有问题的。  
经过一番摸索，原来是ubuntu在默认情况下关闭了root用户的登陆，解决方法为：  
在`/etc/ssh/sshd_config`中,  
把`PermitRootLogin without-password`改为`PermitRootLogin yes`。

<!-- more -->

或者  
  
Or, you can use SSH keys. If you don't have one, create one using ssh-keygen (stick to the default for the key, and skip the password if you feel like it). Then do sudo -s (or whatever your preferred method of becoming root is), and add an SSH key to /root/.ssh/authorized_keys:

`cat /home/user/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys`（未经测试）

