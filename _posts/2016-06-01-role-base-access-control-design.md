---
layout: post
title: "系统账号权限设计"
image: access-control.jpg
video: false
---
1. 账户、角色、权限之间的关系
![Role-base accces control]({{ site.baseurl }}/content/images/role-base-access-control.png)
如果使用关系数据库来存储数据，账户、角色、权限分别对应一张数据库表，账户与角色、角色与权限表用来关联数据。
2. 功能模块的权限设置与拦截代码

给每一个需要授权才能访问的功能点创建响应的权限，创建一个注解类Privilege用来给方法标注权限，使用AOP(面向切面编程) 检查用户是否拥有方法标注的权限。
{% highlight java %}
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Privilege{
   String name() default "";
}
{% endhighlight %}

{% highlight java %}
@Aspect
public class AuthenticationAspect{
    @Before("execution(* com.xyz.myapp.*.*(..))")
    public void doAccessCheck() {
        //检查用户的角色的权限是否包含注解中的权限。
    }

}
{% endhighlight %}
3. 权限的维度

权限的维度与具体的系统相关，这是需要重点分析的地方。比如广告追踪系统，权限的维度有：

  * APP
  * 广告网络
  * 统计指标
  * 创建Campaign


**参考文档：**
[《系统账户权限设计》](http://wenku.baidu.com/view/062189f57c1cfad6195fa7c9.html?qq-pf-to=pcqq.c2c)
