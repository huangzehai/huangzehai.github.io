---
layout: post
title: "OpenJDK 1.7 SimpleDateFormat Bug"
image: OpenJDK.png
video: false
---
最近在Log Server生成小时日志，当时的时间的下午3点钟，我期望一个文件名包含15的日志生成，可是一直不出现，一开始以为是自己代码的问题，反复检查几遍还是找不出问题。最终采用排除一切干扰的方法，写一个最简单的测试类DateDemo。竟然打印出的小时是3。然后我执行了$java -version，发现正在使用的JDK是OpenJDK 1.7，是其他同事安装的。原来这是Open JDK 1.7 SimpleDateFormat类的一个bug，没有正确解析小时“HH”。安装Oracle的JDK后就修复了这个问题。

{% highlight css %}
public class DateDemo {
    private  static final String FORMAT="yyyy-MM-dd HH";
    public static void main(String[] args) {
        DateFormat df =new SimpleDateFormat(FORMAT);
        String date= df.format(new Date());
        System.out.println(date);
    }
}
{% endhighlight %}

当前时间：Wed May  4 18:25:32 CST 2016

#### 打印结果为：
Oracle JDK:
*2016-05-04 18*  
Linux Open JDK:
*2016-05-04 06*
