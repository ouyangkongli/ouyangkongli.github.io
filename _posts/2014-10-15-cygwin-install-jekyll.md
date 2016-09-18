---
layout: post
title: "安装jekyll"
tags: [cygwin, jekyll, Ubuntu]
category: [technique]
---
  

##Ubuntu上的jekyll

强烈建议拥有一个基于linux系统的开发环境，以后会带来许多意向不到的益处。
虽说在linux上安装jekyll等会方便很多，不过在本人的Ubuntu14.04上安装jekyll仍然有许多问题，google了很久，折腾了很久，才找到一个近乎完美的解决方法。这是一个技术大牛**[基于jekyll写的一篇在Ubuntu上安装jekyll的教程](http://michaelchelen.net/81fa/install-jekyll-2-ubuntu-14-04/)**，供广大技术爱好者参考（此blog采用的theme，个人认为非常漂亮，值得推荐）。另外，考虑到国内一部分受困于GFW的同学，你可以直接**[点击此处直接下载](/resources/Install-Jekyll-2-on-Ubuntu14.04–michaelchelen.pdf "下载pdf文件")**观看。
  
##cygwin上安装jekyll(不能保证成功)

###卸载已安装的ruby

用对应的包管理器卸载
如果是rvm安装的，直接删除目录`~/.rvm`，然后删除`.zshrc`和`.zlogin`文件，去掉`.profile`和`.bash_profile`中关于rvm的语句即可。

### 安装rvm

[安装指南](https://rvm.io/rvm/install/)

    echo ipv4 >> ~/.curlrc
    curl -L https://get.rvm.io | bash -s stable --ruby

检测rvm安装

    type rvm | head -n 1

此时会显示`rvm:not found`，不用紧，还需要一步：

    source ~/.rvm/scripts/rvm
	type rvm | head -n 1

现在显示`rvm is a function`，说明rvm安装成功。
查看是否还有依赖问题：

    rvm requirements

### 管理ruby环境

下面这几步其实不必要了，只是展示一下安装的过程，你也可以安装其它的ruby版本。
<!-- more -->

    rvm list known
    rvm install 1.9.3
    rvm use 1.9.3 --default

**这里要重点指出一点，**若你在使用cygwin自带的ruby时，出现了莫名其妙的错误，那么最好卸载cygwin自带的ruby，使用rvm重新安装，而不是再去折腾cygwin，后果只有折腾过的人才会明白。
然而，在使用rvm安装ruby时，仍然可能出现若干错误，例如miniruby.exe权限、fork()等。前几天，我遇到了这两个问题，为避免各位重蹈覆辙，特分享可能的解决方案。  
  
1. **关闭防火墙**。我之前在安装ruby的时候，使用命令 `curl -L https://get.rvm.io | bash`时， comodo会弹出窗口提示是否拦截程序联网，虽然我点击了允许联网，但是貌似已经晚了。
安装程序过一会便会出错终止，例如出现miniruby.exe permission 等错误。后来，卸载了comodo，这个问题便消失了。这里说明一下，并不是comodo不好，只是我还没有学会合理利用comodo。  
2. **fork()问题**。在cygwin中利用rvm安装ruby时，时不时的会遇到rebase的问题（不只是rvm）。这个问题可以通过cygwin中的rebaseall脚本解决。  
  
  首先，在cygwin中执行以下2条语句：
    
    find /home/username//.rvm/rubies/ -iname "*.dll" -print >> /home/username/filelist.txt  
    find /home/username/.rvm/rubies/ -iname "*.so" -print >> /home/username/filelist.txt

其次，关闭所有依赖cygwin.dll的进程，然后使用管理员权限使用cmd在cygwin/bin下执行以下命令：
    
    /bin/rebaseall -v -T /home/username/filelist.txt
    
此时，便可解决fork()问题。
  
  查看安装的版本号和路径

{% highlight ruby %}
ruby -v
gem -v
which ruby
which gem
{% endhighlight %}

### 安装jekyll

用gem安装：

{% highlight ruby %}
gem update --system
gem list
gem install jekyll
{% endhighlight %}

如果出现`spawn.h`的错误，这是由于`posix-spawn`的bug引起的，需要自己编译安装：

{% highlight ruby %}
gem install rake-compiler -v 0.7.6
git clone git://github.com/rtomayko/posix-spawn.git
cd posix-spawn
rake gem
gem install pkg/posix-spawn-0.3.6
{% endhighlight %}

再`gem install jekyll`就没问题啦。

## 参考页面

[Windows,Cygwin and Jekyll](http://matt.scharley.me/2012/03/10/windows-cygwin-and-jekyll.html)
