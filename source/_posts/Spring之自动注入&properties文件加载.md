---
title: Spring之自动注入&properties文件加载
toc: true
date: 2020-2-27 08:54:14
tags:
	- Spring
---
## 自动注入
1. 在 Spring 配置文件中对象名和 ref=”id”id 名相同使用自动注入,可以不配置`<property/>`
2. 两种配置办法
	2.1 在`<bean>`中通过 autowire=”” 配置,只对这个`<bean>`生效
	2.2 在`<beans>`中通过 default-autowire=””配置,表当当前文件中所有`<bean>`都是全局配置内容
<!-- more -->
3. autowire=""可取值
	3.1 default: 默认值,根据全局 default-autowire=””值，默认全局和局部都没有配置情况下，相当于 no
	3.2 no: 不自动注入
	3.3 byName: 通过名称自动注入，在 Spring 容器中找类的 Id
	3.4 byType: 根据类型注入
	- pring容器中不可以出现两个相同类型的`<bean>`
	3.5 constructor: 根据构造方法注入
	- 提供对应参数的构造方法(构造方法参数中包含注入对戏那个)
	- 底层使用 byName, 构造方法参数名和其他`<bean>`的 id相同

## properties文件加载
1. 在 src 下新建 xxx.properties 文件
2. 在 spring 配置文件中先引入 xmlns:context,在下面添加
2.1 如果需要记载多个配置文件逗号分割
```xml
<context:property-placeholder location="classpath:xxx.properties"/>
```
3. 添加了属性文件记载,并且在`<beans>`中开启自动注入注意的地方
	3.1 SqlSessionFactoryBean 的 id 不能叫做 sqlSessionFactory
	3.2 修改
	把原来通过 ref 引用替换成 value 赋值,自动注入只能影响ref,不会影响 value 赋值
```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="basePackage" value="top.ljc.mapper"></property>
<property name="sqlSessionFactoryBeanName" value="factory"></property>
</bean>
```
4. 在被 Spring 管理的类中通过@Value(“${key}”)取出 properties 中内容
	4.1 添加注解扫描
	```xml
	<context:component-scanbase-package="top.ljc.service.impl"></context:component-scan>
	```
	4.2 在类中添加
	- key 和变量名可以不相同
	- 变量类型任意,只要保证 key 对应的 value 能转换成这个类型就可以
	```java
	@Value("${my.demo}")
	private String test;
	```
