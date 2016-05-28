---
layout: post
title: "代码的味道"
image: diff-types-coffee.jpg
video: false
---
不同的代码可以实现相同的功能，有些代码简短明了，有些则看起来有点绕。我喜欢的代码是那种直截了当的，不需要注释，一眼就能看懂的。若代码是一种食物，好的代码应该也有好的代码味道。

今天看到同事写的一行代码，如下：
{% highlight java %}
String country = StringUtils.isNotEmpty(countryString)?countryString:"-"
{% endhighlight %}
这行代码使用的三元表达式，如果国家名称为空，则使用默认占位符。总的来说，就是获取一个字符串的值，如果字符串为空，则使用默认值。
这行代码countryString出现了两次，看起来不是很直观，看看下面的方法是不是直接明了多了？
{% highlight java %}
String country = StringUtils.defaultIfEmpty(countryString,"-")
{% endhighlight %}
顺便指出此处应该测试字符串是否是blank，而不是仅仅测试是否是empty，避免countryString为空字符串的情况。所以代码应该是
{% highlight java %}
String country = StringUtils.defaultIfBlank(countryString,"-")
{% endhighlight %}
