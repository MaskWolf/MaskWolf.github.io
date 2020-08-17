---
title: MyBatis缓存机制
toc: true
date: 2020-2-3 14:05:47
tags:
	- MyBatis
---
1. 应用程序和数据库交互的过程是一个相对比较耗时的过程
2. 缓存存在的意义:让应用程序减少对数据库的访问,提升程序运行效率
<!-- more -->

## MyBatis 中默认 SqlSession 缓存开启
1. 同一个 SqlSession 对象调用同一个`<select>`时,只有第一次访问数据库,第一次之后把查询结果缓存到 SqlSession 缓存区(内存)中
2. 缓存的是 statement 对象，简单记忆就是必须用一个`<select>`
ps:在 myabtis 中一个`<select>`对应一个 statement 对象
3. 有效范围必须是同一个 SqlSession 对象

## 缓存流程
1. 步骤一: 先去缓存区中找是否存在 statement
2. 步骤二:返回结果
3. 步骤三:如果没有缓存 statement 对象,去数据库获取数据
4. 步骤四:数据库返回查询结果
5. 步骤五:把查询结果放到对应的缓存区中

![](http://cdn.liaojincan.top/202023141458.png)

## SqlSessionFactory 缓存
1. SqlSessionFactory 缓存又叫做二级缓存
2. 有效范围:同一个 factory 内任意 SqlSession 都可以获取
3. 使用二级缓存的场景:数据频繁被使用,很少被修改
4. 使用二级缓存步骤
4.1 在 mapper.xml 中添加`<cache readOnly="true"></cache>`
4.2 如果不写 readOnly=”true”需要把实体类序列化
5. 当 SqlSession 对象 close()时或 commit()时会把 SqlSession 缓存的数据刷(flush)到 SqlSessionFactory 缓存区中
