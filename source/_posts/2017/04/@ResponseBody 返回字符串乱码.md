---
title: '@ResponseBody 返回字符串乱码'
categories:
  - 技术笔记
tags:
  - Java
abbrlink: 17357
date: 2017-04-06 00:00:00
---

### 今天在用ajax post方法调用java方法时,返回的结果中文始终是???这种格式。一开始我以为是response没有转码导致的,但是java代码中加入转码后依然无效

``` java
response.setCharacterEncoding("UTF-8");
```

### 最后查资料发现是spring mvc的一个bug，spring MVC有一系列HttpMessageConverter去处理用@ResponseBody注解的返回值，如返回list则使用MappingJacksonHttpMessageConverter，返回string，则使用StringHttpMessageConverter，这个convert使用的是字符集是iso-8859-1,而且是final的


### 网上目前我找到的有两种解决办法

* 自己继承AbstractHttpMessageConverter,写一个类复制 StringHttpMessageConverter.java的代码,将
public static final Charset DEFAULT_CHARSET = Charset.forName("ISO-8859-1");
改为
public static final Charset DEFAULT_CHARSET = Charset.forName("UTF-8");
spring-servlet的配置文件如下
``` xml
<bean  
    class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">  
    <property name="messageConverters">  
        <list>  
            <bean class="com.renren001.converter.UTF8StringHttpMessageConverter" />  
        </list>  
    </property>  
</bean> 
```

* StringHttpMessageConverter默认iso-8859-1编码，但是会根据请求头信息指定的编码格式来转换，所以只需要在ajax请求的时候指定头信息Accept属性
$.ajax({
...
headers: { 
Accept : "text/plain; charset=utf-8",
}
});