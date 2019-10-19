---
title: quartz定时功能
categories:
  - Blog
tags:
  - 定时器
abbrlink: 38358
date: 2017-03-24 00:00:00
---
## quartz定时器

### 配置文件定时

**配置文件**
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
	http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <bean id="executor" class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor">
        <property name="corePoolSize" value="10" />
        <property name="maxPoolSize" value="100" />
        <property name="queueCapacity" value="500" />
    </bean>

    <!-- spring自动任务调度器配置 -->
    <!-- 要调用的工作类 -->
    <bean id="job1"
          class="com.***.***.quartz.SpringQtz"></bean>

    <bean id="jobTask1"
          class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
        <!-- 调用的类 -->
        <property name="targetObject">
            <ref bean="job1"/>
        </property>
        <!-- 调用类中的方法 -->
        <property name="targetMethod">
            <value>qtzTest</value>
        </property>
        <!-- 是否允许任务并发执行。当值为false时，表示必须等到前一个线程处理完毕后才再启一个新的线程 -->
        <property name="concurrent" value="false"/>
    </bean>


    <!-- 触发器配置 时间指定 -->
    <bean id="cronTrigger1" class="org.springframework.scheduling.quartz.CronTriggerBean">
        <property name="jobDetail" ref="jobTask1"></property>
        <!-- cron表达式 -->
        <property name="cronExpression">
            <!-- 每隔10秒执行一次 -->
            <value>0/10 * * * * ?</value>
        </property>
    </bean>


    <!-- 总管理类 如果将lazy-init='false'那么容器启动就会执行调度程序 -->
    <bean id="startQuertz" lazy-init="false" autowire="no"
          class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
        <property name="triggers">
            <list>
                <!-- 触发器列表 -->
                <ref bean="cronTrigger1"/>
            </list>
        </property>
    </bean>

</beans>
```


**spring引入配置文件**
``` xml
<import resource="spring-timer.xml" />
```