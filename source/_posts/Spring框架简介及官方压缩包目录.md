---
title: Spring框架简介及官方压缩包目录
toc: true
date: 2020-2-11 18:09:48
tags:
	- Spring
---

1. 主要发明者：Rod Johnson
2. 轮子理论推崇者：
	2.1 轮子理论：不用重复发明轮子
	2.2 IT 行业：直接使用写好的代码
3. Spring 框架宗旨：不重新发明技术，让原有技术使用起来更加方便
<!-- more -->

## Spring 几大核心功能
1. IoC/DI (控制反转/依赖注入)
2. AOP (面向切面编程)
3. 声明式事务

## Spring 框架 runtime
![](http://cdn.liaojincan.top/2020211180300.png)
1. test： spring 提供测试功能
2. Core Container：核心容器(Spring 启动最基本的条件)
	2.1 Beans ： Spring 负责创建类对象并管理对象
	2.2 Core： 核心类
	2.3 Context： 上下文参数，获取外部资源或者管理注解等
	2.4 SpringEl： expression.jar
3. AOP： 实现 aop 功能需要依赖
4. Aspects： 切面 AOP 依赖的包
5. Data Access/Integration： spring 封装数据访问层相关内容
	5.1 JDBC：Spring 对 JDBC 封装后的代码
	5.2 ORM：封装了持久层框架的代码， 例如 Hibernate
	5.3 transactions：对应 spring-tx.jar，声明式事务使用
6. WEB：需要 spring 完成 web 相关功能时需要
	6.1 例如：由 tomcat 加载 spring 配置文件时需要有 spring-web 包

PS： 从 Spring3 开始把 Spring 框架的功能拆分成多个 jar，Spring2 及以前就一个 jar

## Spring 框架中重要概念
1. 容器(Container)： Spring 当作一个大容器
2. BeanFactory 接口(老版本)
	2.1 新版本中 ApplicationContext 接口是 BeanFactory 的子接口，BeanFactory 的功能在 ApplicationContext 中都有
