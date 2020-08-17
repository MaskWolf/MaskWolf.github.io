---
title: 命名规范&MVC
toc: true
date: 2019-12-2 12:24:57
tags:
	- JavaEE
---

## 命名规范
1. 项目名：不使用中文，无其他要求
2. 包：公司（个人）域名倒写
3. 数据访问层：dao,persist,**mapper**
<!-- more -->
4. 实体：entity,model,bean,javabean,**pojo**
5. 业务逻辑：**service**,biz
6. 控制器：controller,**servlet**,action,web
7. 过滤器：filter
8. 异常：exception
9. 监听器：listener
10. 注释：
  - 在类和方法上使用文档注释`/** */`
  - 在方法里面使用`/* */`或`//`
11. 类：大驼峰
12. 方法：小驼峰

注意：一般异常是在数据访问层和控制层进行处理，业务层只将异常抛出

## MVC开发模式
1. M：Model 模型，实体类，业务，dao
2. V：view 视图，JSP
3. C：Controller 控制器，servlet
  - 作用：视图和逻辑分离
4. MVC开发模式适用场景：大型项目的开发，越往底层复用越高
5. 图实例
![](http://cdn.liaojincan.top/20190325115734553.png)
  - 先设计数据库
  - 其次实体类
  - 再者数据访问层
  - 再写业务逻辑
  - 再就是写控制器
  - 最后写视图
