---
layout: post
title: "使用国内RubyGem镜像"
image: Ruby_logo.png
video: false
---

rubygems.org 托管在Amazon S3上，在国内访问其资源文件会间歇性连接失败，执行gem install rack时很久没有响应。解决的方法是使用国内的RubyGem镜像。

#### 国内的RubyGem镜像有：
* 中山大学：[http://mirror.sysu.edu.cn/rubygems/](http://mirror.sysu.edu.cn/rubygems/)
* 山东理工大学：[http://ruby.sdutlinux.org/](http://ruby.sdutlinux.org/)
* 淘宝网：[http://ruby.taobao.org/](http://ruby.taobao.org/)
* Ruby China: [https://gems.ruby-china.org/](https://gems.ruby-china.org/)

#### 如何使用？
{% highlight shell %}
$ gem sources --add http://mirror.sysu.edu.cn/rubygems --remove https://rubygems.org/
$ gem sources -l
{% endhighlight %}
