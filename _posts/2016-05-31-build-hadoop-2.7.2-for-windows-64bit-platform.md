---
layout: post
title: "在64位Windows上编译Hadoop2.7.2"
image: hadoop.png
video: false
---
hadoop-2.7.2-src的根目录有一个编译说明文件BUILDING.txt,里面列出编译时所需要的工具。划掉的是可选并且是我没有选择的。

----------------------------------------------------------------------------------
**Building on Windows**

----------------------------------------------------------------------------------
Requirements:

* Windows System
* JDK 1.7+
* Maven 3.0 or later
* ~~Findbugs 1.3.9 (if running findbugs)~~
* ProtocolBuffer 2.5.0
* CMake 2.6 or newer
* [Windows SDK 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=8279) ~~or Visual Studio 2010 Professional~~
* ~~Windows SDK 8.1 (if building CPU rate control for the container executor)~~
* ~~zlib headers (if building native code bindings for zlib)~~
* Internet connection for first build (to fetch all Maven and Hadoop dependencies)
* Unix command-line tools from ~~GnuWin32~~: sh, mkdir, rm, cp, tar, gzip. These
  tools must be present on your PATH.

**补充编译时需要注意的事项：**

1. 因为GnuWin32不包含sh.exe，使用Cygwin64代替。（如果是32位系统，请使用32位的版本）
2. Visual C++ 2010 x64 Redistributable v10.0.30319 如果你已经安装了更新版本的Visual C++ 2010 x64 Redistributable，Windows SDK 7.1就是安装失败，需要先卸载更新版本的Visual C++ 2010 x64 Redistributable，然后安装Windows SDK 7.1
3. Microsoft .NET Framework v4.0 如果你安装了V4.5或更新的版本，你需要先卸载然后安装V4.0版本的。
4. 安装ProtocolBuffer：下载、解压protoc-2.5.0-win32.zip，然后将其目录添加到PATH。
5. 将包含 MSBuild.exe的文件夹添加到PATH（C:\Windows\Microsoft.NET\Framework64\v4.0.30319）
6. 如果JDK路径不能包含空格，如果把JDK安装在 Program Files，在设置JAVA_HOME变量时用Windows缩写 PROGRA~1代替Program Files
7. 设置环境变量Platform 为 x64或 Win32
8. 如果你使用Java 8来编译，需要禁用doclint。在hadoop src的pom.xml里找到
maven-javadoc-plugin插件，在其<configuration></configuration>节点添加属性 <additionalparam>-Xdoclint:none</additionalparam>

环境准备好之后，打开命令行工具,执行下面的maven命令来编译代码。
{% highlight shell %}
	$ cd hadoop-2.7.2-src
	$ mvn package -Pdist,native-win -DskipTests -Dtar
{% endhighlight %}

本文主要参考了karthikj1的博文[《Apache Hadoop 2.7.1 binary for Windows 64-bit platform》](https://github.com/karthikj1/Hadoop-2.7.1-Windows-64-binaries)。这篇文章写的很详细，一看就知道是亲自编译后的经验所得。没有他的帮助我不会这么顺利地在Windows下编译Hadoop 2.7.2，在此深表谢意。
