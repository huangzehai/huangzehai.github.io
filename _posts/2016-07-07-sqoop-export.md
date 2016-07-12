---
layout: post
title: "使用Sqoop将Hadoop|Hive的数据导出到MySQL"
image: sqoop.png
video: false
---
在MySQL数据库中创建表
{% highlight sql %}
CREATE TABLE `NewCity` (
  `ID` int(11) NOT NULL AUTO_INCREMENT,
  `Name` char(35) NOT NULL DEFAULT '',
  `CountryCode` char(3) NOT NULL DEFAULT '',
  PRIMARY KEY (`ID`),
  KEY `CountryCode` (`CountryCode`),
  CONSTRAINT `new_city_ibfk_1` FOREIGN KEY (`CountryCode`) REFERENCES `Country` (`Code`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
{% endhighlight %}

### 将Hadoop中的数据导出到MySQL

{% highlight shell %}
sqoop export --connect jdbc:mysql://localhost/world --username root -P --columns 'ID,Name,CountryCode' --export-dir /user/adam/City --table NewCity
{% endhighlight %}

### 将Hive中的数据导出到MySQL

将Hive数据导出到MySQL，因为Hive的默认字段分隔符跟HDFS不一样，所以需要设置--input-fields-terminated-by '\001'
{% highlight shell %}
sqoop export --connect jdbc:mysql://localhost/world --username root -P --columns 'ID,Name,CountryCode' --export-dir /user/hive/warehouse/city --table NewCity --input-fields-terminated-by '\001'
{% endhighlight %}

**注意**

1. 表名大小写敏感
2. 在分布式环境中MySQL的URL应使用hostname或者IP，如果使用localhost，可能每个节点会连接不同的数据库。
