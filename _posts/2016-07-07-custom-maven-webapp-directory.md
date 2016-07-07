---
layout: post
title: "如何使用Maven Tomcat Plugin运行自定义目录结构的Maven webapp"
image: maven.png
video: false
---
Maven项目建议使用默认的目录结构，但有时候由于历史的原因或者其他的原因，我们无法决定项目的目录结构。

最近我们的项目组也使用Maven来构建项目，但是目录结构却刻意模仿ANT，Java代码和资源文件放到src目录下，Webapp的根目录为应用根目录下的WebRoot。
其他的同事使用Eclipse Tomcat插件来运行WebApp。然而我更喜欢Intellij Idea IDE，免费的社区版没有Tomcat插件，只能使用Maven的Tomcat Plugin来运行和调试WebApp。

其实Maven的Tomcat Plugin也支持自定义的web根目录。只要在配置中设置warSourceDirectory为自定义的Webapp的目录即可。
![Maven项目的目录结构]({{ site.baseurl }}/content/images/maven-project-structure.png)
{% highlight xml %}
<plugin>
	<groupId>org.apache.tomcat.maven</groupId>
	 <artifactId>tomcat7-maven-plugin</artifactId>
	<version>2.2</version>
	<configuration>
	 <warSourceDirectory>WebRoot</warSourceDirectory>
	</configuration>
</plugin>
{% endhighlight %}
