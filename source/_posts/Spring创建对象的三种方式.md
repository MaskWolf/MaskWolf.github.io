---
title: Spring创建对象的三种方式
toc: true
date: 2020-2-24 08:20:58
tags:
	- Spring
---
## 构造方法
1. 无参构造创建：默认情况
2. 有参构造创建：需要明确配置
	2.1 需要在类中提供有参构造方法
	2.2 在 applicationContext.xml 中设置调用哪个构造方法创建对象
	<!-- more -->
	- 如果设定的条件匹配多个构造方法执行最后的构造方法
	- index ：参数的索引,从 0 开始
	- name：参数名
	- type：类型(区分开关键字和封装类 int 和 Integer)
	```xml
	<bean id="peo" class="top.ljc.pojo.People">
		<!--
			ref 引用另一个 bean
			value 基本数据类型或String等
		-->
		<constructor-arg index="0" name="id" type="int"value="123"></constructor-arg>
		<constructor-arg index="1" name="name" type="java.lang.String" value="张三"></constructor-arg>
	</bean>
	```

## 实例工厂
1. 工厂设计模式：帮助创建类对象，一个工厂可以生产多个对象
2. 实例工厂：需要先创建工厂，才能生产对象
3. 实现步骤
	3.1必须要有一个实例工厂
	```java
	public class PeopleFactory {
		public People newInstance(){
			return new People(1,"测试");
		}
	}
	```
	3.2 在 applicationContext.xml 中配置工厂对象和需要创建的对象
	```xml
	<bean id="factory" class="top.ljc.pojo.PeopleFactory"></bean>
	<bean id="peo1" factory-bean="factory" factory-method="newInstance"></bean>
	```

## 静态工厂
1. 不需要创建工厂，快速创建对象
2. 实现步骤
2.1 编写一个静态工厂(在方法上添加 static)
	```java
	public class PeopleFactory {
		public static People newInstance(){
			return new People(1,"测试");
		}
	}
	```
	2.2 在 applicationContext.xml 中
	```xml
	<bean id="peo2" class="top.ljc.pojo.PeopleFactory" factory-method="newInstance"></bean>
	```
