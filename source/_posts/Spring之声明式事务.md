---
title: Spring之声明式事务
toc: true
date: 2020-2-27 09:15:43
tags:
	- Spring
---
1. 编程式事务
	1.1 由程序员编程事务控制代码
	1.2 OpenSessionInView 编程式事务
2. 声明式事务
	2.1 事务控制代码已经由 spring 写好.程序员只需要声明出哪些方法需要进行事务控制和如何进行事务控制
<!-- more -->
3. 声明式事务都是针对于 ServiceImpl 类下方法的
4. 事务管理器基于通知(advice)的

## 在spring配置文件中配置声明式事务

```xml
<context:property-placeholder location="classpath:db.properties,classpath:second.properties"/>
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName" value="${jdbc.driver}"></property>
	<property name="url" value="${jdbc.url}"></property>
	<property name="username" value="${jdbc.username}"></property>
	<property name="password" value="${jdbc.password}"></property>
</bean>
<!-- spring-jdbc.jar 中 -->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<property name="dataSource" ref="dataSource"></property>
</bean>
<!-- 配置声明式事务 -->
<tx:advice id="txAdvice" transaction-manager="txManager">
	<tx:attributes>
		<!-- 哪些方法需要有事务控制 -->
		<!-- 方法以 ins 开头事务管理 -->
		<tx:method name="ins*" />
		<tx:method name="del*" />
		<tx:method name="upd*" />
		<tx:method name="*" />
	</tx:attributes>
</tx:advice>
<aop:config>
<!-- 切点范围设置大一些 -->
<aop:pointcut expression="execution(*top.ljc.service.impl.*.*(..))" id="mypoint" />
	<aop:advisor advice-ref="txAdvice" pointcut-ref="mypoint" />
</aop:config>
```

## 声明式事务中属性解释
1. name=”” 哪些方法需要有事务控制
	1.1 支持`*`通配符
2. readonly=”boolean” 是否是只读事务
	2.1 如果为 true,告诉数据库此事务为只读事务.数据化优化,会对性能有一定提升,所以只要是查询的方法,建议使用此数据
	2.2 如果为 false(默认值),事务需要提交的事务.建议新增,删除,修改
3. propagation 控制事务传播行为
	3.1 `REQUIRED (默认值)`: 如果当前有事务,就在事务中执行,如果当前没有事务,新建一个事务
	3.2 `SUPPORTS`:如果当前有事务就在事务中执行,如果当前没有事务,就在非事务状态下执行
	3.3 `MANDATORY`:必须在事务内部执行,如果当前有事务,就在事务中执行,如果没有事务,报错
	3.4 `REQUIRES_NEW`:必须在事务中执行,如果当前没有事务,新建事务,如果当前有事务,把当前事务挂起
	3.5 `NOT_SUPPORTED`:必须在非事务下执行,如果当前没有事务,正常执行,如果当前有事务,把当前事务挂起.
	3.6 `NEVER`:必须在非事务状态下执行,如果当前没有事务,正常执行,如果当前有事务,报错
	3.7 `NESTED`:必须在事务状态下执行.如果没有事务,新建事务,如果当前有事务,创建一个嵌套事务
4. isolation=””事务隔离级别
	4.1 在多线程或并发访问下如何保证访问到的数据具有完整性的
	4.2 脏读
	一个事务(A)读取到另一个事务(B)中未提交的数据,另一个事务中数据可能进行了改变,此时 A 事务读取的数据可能和数据库中数据是不一致的,此时认为数据是脏数据,读取脏数据过程叫做脏读
	4.3 不可重复读
	- 主要针对的是某行数据(或行中某列)
	- 主要针对的操作是修改操作
	- 两次读取在同一个事务内
	- 当事务 A 第一次读取事务后,事务 B 对事务 A 读取的淑君进行修改,事务 A 中再次读取的数据和之前读取的数据不一致,过程不可重复读

	4.4 幻读
	- 主要针对的操作是新增或删除
	- 两次事务的结果.
	- 事务 A 按照特定条件查询出结果,事务 B 新增了一条符合条件的数据.事务 A 中查询的数据和数据库中的数据不一致的,事务 A 好像出现了幻觉,这种情况称为幻读

	4.5 `DEFAULT`: 默认值,由底层数据库自动判断应该使用什么隔离界别
	4.6 `READ_UNCOMMITTED`: 可以读取未提交数据,可能出现脏读,不重复读,幻读，但效率最高
	4.7 `READ_COMMITTED`:只能读取其他事务已提交数据.可以防止脏读,可能出现不可重复读和幻读
	4.8 `REPEATABLE_READ`: 读取的数据被添加锁,防止其他事务修改此数据,可以防止不可重复读.脏读,可能出现幻读
	4.9 `SERIALIZABLE`: 排队操作,对整个表添加锁.一个事务在操作数据时,另一个事务等待事务操作完成后才能操作这个表（是最安全的也是效率最低的）
5. rollback-for=”异常类型全限定路径”
	5.1 当出现什么异常时需要进行回滚
	5.2 建议:给定该属性值，手动抛异常一定要给该属性值
6. no-rollback-for=””
	6.1 当出现什么异常时不滚回事务
