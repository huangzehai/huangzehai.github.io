---
layout: post
title: "在Windows上使用Java API操作HBase"
image: hbase_logo_with_orca_large.png
video: false
---
### HBase Configuration
{% highlight css %}
adam@hbase:~/hbase-1.2.1/conf$ cat /etc/hostname
hbase
{% endhighlight %}

{% highlight css %}
adam@hbase:~/hbase-1.2.1/conf$ cat /etc/hosts
127.0.0.1    localhost
#For region servers
10.1.2.127 hbase

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
{% endhighlight %}

##hbase-site.xml

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

###Windows Client:
####hosts
{% highlight css %}
# MyVM
10.1.2.127 hbase
{% endhighlight %}

{% highlight css %}
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.2.1</version>
</dependency>
{% endhighlight %}

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