---
title: spring aop事务
categories:
  - Blog
tags:
  - Spring
abbrlink: 43959
date: 2017-06-29 00:00:00
---

由于今天项目配置了spring aop事务发现不生效,检查配置文件如下没有问题

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">

	<!-- 配置事务管理器 -->
	<bean id="mysqlTransactionManager"
		  class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>

	<!-- 使用annotation注解方式配置事务 -->
	<tx:annotation-driven transaction-manager="mysqlTransactionManager"/>

	<!-- AOP配置事物 -->
	<tx:advice id="mysqlTransactionAdvice" transaction-manager="mysqlTransactionManager">
		<tx:attributes>
			<tx:method name="query*" read-only="true" propagation="REQUIRED"/>
			<tx:method name="delete*" propagation="REQUIRED" rollback-for="Exception"/>
			<tx:method name="update*" propagation="REQUIRED" rollback-for="Exception"/>
			<tx:method name="insert*" propagation="REQUIRED" rollback-for="Exception"/>
			<tx:method name="add*" propagation="REQUIRED" rollback-for="Exception"/>
			<tx:method name="save*" propagation="REQUIRED" rollback-for="Exception"/>
			<tx:method name="rollBack*" propagation="REQUIRED" rollback-for="Exception"/>
		</tx:attributes>
	</tx:advice>

	<!-- 配置AOP切面 -->
	<aop:config>
		<aop:pointcut id="mysqlTransactionPointCut"
					  expression="execution(* com.at.service.impl.*.*(..))"/>
		<aop:advisor pointcut-ref="mysqlTransactionPointCut"
					 advice-ref="mysqlTransactionAdvice"/>
	</aop:config>
</beans>

```

网上查询了下资料,发现是由于mysql数据库存储引擎导致的, MyISAM不支持事务,需要修改成InnoDB

**查看**

``` mysql
show table status;
```

**修改**

``` mysql
alter table table_name engine=innodb;
```