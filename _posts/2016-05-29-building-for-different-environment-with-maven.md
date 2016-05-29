---
layout: post
title: "不同的环境使用不同的配置文件"
image: maven.png
video: false
---

从开发者的角度看软件开发的过程大概分为三个阶段：开发、测试、上线。不同的开发阶段有不同的运行环境，运行环境包括应用服务器、数据库服务器、缓存服务器等，不同的运行的环境有不同的配置文件。在一个项目中往往存在多套配置文件，怎么样使得多套配置文件共存于同一个项目，然后打包时根据环境的不同的使用相应的配置文件呢？

如果你的项目使用Maven来构建，可以使用Maven Profile来解决这个问题。

在Maven项目的resouces下给不同的环境分别创建一个文件夹，开发环境：dev，测试环境:test,产品环境：production。类似下图：
![项目结构]({{ site.baseurl }}/content/images/project-structure.png)

在pom.xml中配置profile，分别设置不同环境的environment属性，供maven resources plugin使用。接着配置maven resources plugin，打包时有一个步骤是拷贝资源文件，这里的配置是要将指定环境的资源文件拷贝到classpath根目录。
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.hzh</groupId>
    <artifactId>maven-tutorial</artifactId>
    <version>1.0-SNAPSHOT</version>


    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <excludes>
                    <exclude>dev/**</exclude>
                    <exclude>test/**</exclude>
                    <exclude>production/**</exclude>
                </excludes>
            </resource>
        </resources>
        <plugins>
            <plugin>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.7</version>
                <executions>
                    <execution>
                        <id>copy-resources</id>
                        <phase>validate</phase>
                        <goals>
                            <goal>copy-resources</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>${project.build.outputDirectory}</outputDirectory>
                            <resources>
                                <resource>
                                    <directory>src/main/resources/${environment}</directory>
                                    <filtering>true</filtering>
                                </resource>
                            </resources>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>


    <profiles>
        <profile>
            <id>dev</id>
            <properties>
                <environment>dev</environment>
            </properties>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        <profile>
            <id>test</id>
            <properties>
                <environment>test</environment>
            </properties>
        </profile>
        <profile>
            <id>production</id>
            <properties>
                <environment>production</environment>
            </properties>
        </profile>
    </profiles>
</project>
{% endhighlight %}

配置好之后，就是怎么打包了。通过P参数指定profile就可以给不同环境分别打包了。
{% highlight shell %}
$ mvn clean package -P dev
{% endhighlight %}


