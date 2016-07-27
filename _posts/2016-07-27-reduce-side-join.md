---
layout: post
title: "使用Map/Reduce实现SQL表联结"
image: reduce-side-join-banner.png
video: false
---
MR是Hive的默认执行引擎，Hive QL的表联结在MR中是如何实现的呢？通常的方法是在Reduce中进行联结操作。在Map的输出Key和Value了引入标签来标记表，自定义分区函数和分组函数，分区和分组阶段要忽略这个标签，但在排序阶段中要根据Key和标签排序，在Reduce中根据标签将Values分组，输出Values分组的笛卡尔积就是join的结果了。
![Reduce Side Join]({{ site.baseurl }}/content/images/reduce-side-join.png)

参考文档：
[《Join Algorithms using Map/Reduce》](http://www.inf.ed.ac.uk/publications/thesis/online/IM100859.pdf)
