---
title: 基于renren-fast开发，负载均衡缺少pom文件大坑
date: 2021-01-06 12:16:12
categories: renren-fast
tags:
- 后端
---

@通过网关连接renren-fast时候报的错

#  Error creating bean with name 'ribbonLoadBalancingHttpClient'
通过spring gateway 网关把请求分配给renren-fast的时候出现了以下错误。
```
2021-01-06 22:47:02.793 ERROR 14024 --- [ctor-http-nio-2] a.w.r.e.AbstractErrorWebExceptionHandler : [b41c453c] 500 Server Error for HTTP GET "/api/captcha.jpg?uuid=197cc4fb-3695-43cd-8293-8cf4dc31b91f"
```
couse by 1
```
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'ribbonLoadBalancer' defined in org.springframework.cloud.netflix.ribbon.RibbonClientConfiguration: Unsatisfied dependency expressed through method 'ribbonLoadBalancer' parameter 2; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'ribbonServerListFilter' defined in org.springframework.cloud.netflix.ribbon.RibbonClientConfiguration: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.netflix.loadbalancer.ServerListFilter]: Factory method 'ribbonServerListFilter' threw exception; nested exception is java.lang.NoClassDefFoundError: com/netflix/servo/monitor/Monitors
```
couse by 2
```
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'ribbonServerListFilter' defined in org.springframework.cloud.netflix.ribbon.RibbonClientConfiguration: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.netflix.loadbalancer.ServerListFilter]: Factory method 'ribbonServerListFilter' threw exception; nested exception is java.lang.NoClassDefFoundError: com/netflix/servo/monitor/Monitors
```
![](https://img-blog.csdnimg.cn/20210106222317765.png)根本没有这个东西

所以参考
链接: [link]( https://www.cnblogs.com/ye-feng-yu/p/11106006.html).的博客gateway ribbon 负载均衡这块，发现自己的pom文件里没有如下依赖
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
    <version>2.1.0.RELEASE</version>
</dependency>
```

这个加上，重新reload Maven 重跑gateway
![](https://img-blog.csdnimg.cn/2021010700364017.png)
原先获取图片的地方由500 变成了 200  成功了 