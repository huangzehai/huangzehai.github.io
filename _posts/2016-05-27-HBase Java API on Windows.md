---
layout: post
title: "在Windows上使用Java API操作HBase"
image: hbase_logo_with_orca_large.png
video: false
---
### 概述
在Windows上安装VirtualBox虚拟机，在虚拟机上安装Ubuntu 12.04 Server，在Ubuntu中安装HBase单机版。在Windows上使用Java API操作HBase。

### Ubuntu

域名配置
{% highlight css %}
adam@hbase:~/hbase-1.2.1/conf$ cat /etc/hostname
hbase
{% endhighlight %}


{% highlight css %}
adam@hbase:~/hbase-1.2.1/conf$ cat /etc/hosts
127.0.0.1    localhost
# For region servers
10.1.2.127 hbase

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
{% endhighlight %}

#### hbase-site.xml

{% highlight css %}
<configuration>
<property>
    <name>hbase.rootdir</name>
    <value>file:///home/adam/data/hbase</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/home/adam/data/zookeeper</value>
  </property>
  <!-- optional -->
  <property>
  <name>hbase.zookeeper.quorum</name>
  <value>hbase</value>
</property>
</configuration>
{% endhighlight %}

regionservers[optional]
hbase

### Windows Client

域名配置
{% highlight css %}
# MyVM
10.1.2.127 hbase
{% endhighlight %}

Java API的maven依赖仅需要hbase-client。
{% highlight css %}
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.2.1</version>
</dependency>
{% endhighlight %}

在Windows上调用HBase的Java API会报找不到winutils.exe的错误。
可以设置hadoop.home.dir为hadoop common来修复这个问题。Hadoop官方没有编译好的Windows Hadoop发行包，需要自己在Windows上编译或者下载别人编译好的hadoop common包.
{% highlight css %}
public class HBaseDemo {
    public final static byte[] family = Bytes.toBytes("info");

    public static void main(String[] args) throws IOException, ServiceException {
        System.setProperty("hadoop.home.dir", "D:\\hadoop-common-2.7.2-bin");
        Configuration configuration = HBaseConfiguration.create();
        configuration.set("hbase.zookeeper.quorum", "hbase");
        configuration.set("hbase.zookeeper.property.clientPort", "2181");

        HTable table = new HTable(configuration, "users");
        Put put = new Put("row".getBytes());
        put.add(family, "11".getBytes(), "256".getBytes());
        table.put(put);
    }
}
{% endhighlight %}
