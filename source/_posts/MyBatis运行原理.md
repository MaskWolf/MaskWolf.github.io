---
title: MyBatis运行原理
toc: true
date: 2020-2-8 15:31:14
tags:
	- MyBatis
---
## 文字解释
在 MyBatis 运行开始时需要先通过 Resources 加载全局配置文件

下面需要实例化SqlSessionFactoryBuilder 构建器，帮助 SqlSessionFactory 接口实现类 DefaultSqlSessionFactory
<!-- more -->
在实例化 DefaultSqlSessionFactory 之前需要先创建 XmlConfigBuilder解析全局配置文件流，并把解析结果存放在 Configuration 中

之后把 Configuratin 传递给 DefaultSqlSessionFactory

到此 SqlSessionFactory 工厂创建成功

再由 SqlSessionFactory 工厂创建 SqlSession

每次创建 SqlSession 时，都需要由 TransactionFactory 创建 Transaction 对象，同时还需要创建 SqlSession 的执行器 Excutor ，最后实例化 DefaultSqlSession ，传递给 SqlSession 接口

根据项目需求使用 SqlSession 接口中的 API 完成具体的事务操作

如果事务执行失败，需要进行 rollback 回滚事务

如果事务执行成功提交给数据库，关闭 SqlSession

到此就是 MyBatis 的运行原理(给面试官说的)

## 流程图
<div align='center'>
![](http://cdn.liaojincan.top/202028153435.png" width='400' height='900'/>
</div>
## 运行过程中涉及到的类
1. Resources MyBatis 中 IO 流的工具类
	1.1 加载配置文件
2. SqlSessionFactoryBuilder() 构建器
	2.1 作用:创建 SqlSessionFactory 接口的实现类
3. XMLConfigBuilder MyBatis 全局配置文件内容构建器类
	3.1 作用负责读取流内容并转换为 JAVA 代码.
4. Configuration 封装了全局配置文件所有配置信息.
	4.1 全局配置文件内容存放在 Configuration 中
5. DefaultSqlSessionFactory 是 SqlSessionFactory 接口的实现类
6. Transaction 事务类
	6.1 每一个 SqlSession 会带有一个 Transaction 对象.
7. TransactionFactory 事务工厂
	7.1 负责生产 Transaction
8. Executor MyBatis 执行器
	8.1 作用:负责执行 SQL 命令
	8.2 相当于 JDBC 中 statement 对象(或 PreparedStatement 或 CallableStatement)
	8.3 默认的执行器 SimpleExcutor
	8.4 批量操作 BatchExcutor
	8.5 通过 openSession(参数控制)
9. DefaultSqlSession 是 SqlSession 接口的实现类
10. ExceptionFactory MyBatis 中异常工厂
