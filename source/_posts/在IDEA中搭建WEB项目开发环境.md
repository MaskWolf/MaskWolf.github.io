---
title: 在IDEA中搭建WEB项目开发环境
toc: true
date: 2019-12-5 17:32:05
tags:
	- JavaEE
---

### 创建项目
记得要勾选Web Application
![](http://cdn.liaojincan.top/20191205165514.png)
<!-- more -->
![](http://cdn.liaojincan.top/20191205165634.png)

### 在WEB-INF目录下创建文件夹libs和classes
- libs:放置项目所需的各种.jar文件
- classes:存放src下源文件编译后生成的类生成文件

![](http://cdn.liaojincan.top/20191205165853.png)
### 将libs和classes文件夹与项目关联
设置类生成文件输出路径为刚才创建的classes文件夹
![](http://cdn.liaojincan.top/20191205170211.png)
设置刚才创建的libs文件夹为存放.jar文件的文件夹
![](http://cdn.liaojincan.top/20191205170313.png)
![](http://cdn.liaojincan.top/20191205170354.png)
![](http://cdn.liaojincan.top/20191205170439.png)
IEDA没有工具栏的可以按照下图开启toolbar
![](http://cdn.liaojincan.top/20191205170000.png)

### 配置Tomcat本地容器
创建Tomcat本地容器
![](http://cdn.liaojincan.top/20191205170543.png)
![](http://cdn.liaojincan.top/20191205170620.png)
以热部署方式部署项目到Tomcat容器中
![](http://cdn.liaojincan.top/20191205170706.png)
![](http://cdn.liaojincan.top/20191205172822.png)
