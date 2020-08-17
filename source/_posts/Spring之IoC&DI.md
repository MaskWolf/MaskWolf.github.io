---
title: Spring之IoC&DI
toc: true
date: 2020-2-24 08:27:36
tags:
	- Spring
---

## IoC
IoC，即 Inversion of Control (控制反转)
1. IoC 将原先由程序员主动通过 new 实例化对象的工作转交给了 Spring 负责
2. 控制反转中控制指的是：控制类的对象
3. 控制反转中反转指的是：转交给 Spring 负责
4. IoC 最大的作用：**解耦** (程序员不需要管理对象，解除了对象管理和程序员之间的耦合)
<!-- more -->

## DI
DI，即 Dependency Injection（依赖注入）
1. DI 和 IoC 是一样的
2. 当一个类(A)中需要依赖另一个类()对象时，把 B 赋值给 A 的过程就叫做依赖注入
3. 代码体现
```xml
<bean id="peo" class="top.ljc.pojo.People">
	<property name="desk" ref="desk"></property>
</bean>
<bean id="desk" class="top.ljc.pojo.Desk">
	<property name="id" value="1"></property>
	<property name="price" value="12"></property>
</bean>
```
## 给 Bean 的属性赋值(注入)
- 通过构造方法设置值
- 设置注入(通过 set 方法)

1. 如果属性是基本数据类型或 String 等
```xml
<bean id="peo" class="top.ljc.pojo.People">
	<property name="id" value="222"></property>
	<property name="name" value="张三"></property>
</bean>
```
	等价于
	```xml
	<bean id="peo" class="top.ljc.pojo.People">
		<property name="id">
			<value>456</value>
		</property>
		<property name="name">
			<value>zhangsan</value>
		</property>
	</bean>
	```
2. 如果属性是` Set<?>`
```xml
<property name="sets">
	<set>
		<value>1</value>
		<value>2</value>
		<value>3</value>
		<value>4</value>
	</set>
</property>
```
3. 如果属性是 `List<?>`
```xml
<property name="list">
	<list>
		<value>1</value>
		<value>2</value>
		<value>3</value>
	</list>
</property>
```
	如果 list 中就只有一个值
	```xml
	<property name="list" value="1"></property>
	```
4. 如果属性是数组
如果数组中就只有一个值,可以直接通过 value 属性赋值
```xml
<property name="strs" >
	<array>
		<value>1</value>
		<value>2</value>
		<value>3</value>
	</array>
</property>
```
5. 如果属性是 map
```xml
<property name="map">
	<map>
		<entry key="a" value="b" ></entry>
		<entry key="c" value="d" ></entry>
	</map>
</property>
```
6. 如果属性 Properties 类型
```xml
<property name="demo">
	<props>
		<prop key="key">value</prop>
		<prop key="key1">value1</prop>
	</props>
</property>
```
