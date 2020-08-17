---
title: Servlet生命周期&常见错误&三个方法
toc: true
date: 2019-11-13 18:04:10
tags:
  - Servlet&Jsp
---

## Servlet的生命周期
- 从第一次调用到服务器关闭。
- 如果Servlet在web.xml中配置了load-on-startup，生命周期为从服务器启动到服务器关闭
<!-- more -->
注意：
1. init方法是对Servlet进行初始化的一个方法，会在Servlet第一次加载进行存储时执行
2. destory方法是在servlet被销毁时执行，也就服务器关闭时。

## Servlet的常见错误

### 404错误:资源未找到
1. 在请求地址中的servlet的别名书写错误。
2. 虚拟项目名称拼写错误

### 500错误：内部服务器错误
1. java.lang.ClassNotFoundException: top.ljc.servlet.ServletMothod
解决：在web.xml中校验servlet类的全限定路径是否拼写错误。
2. 因为service方法体的代码执行错误导致
解决：根据错误提示对service方法体中的代码进行错误更改。
### 405错误:请求方式不支持
原因：请求方式和servlet中的方法不匹配所造成的。
解决：尽量使用service方法进行请求处理，并且不要再service方法中调用父类的service。

## service、doGet、doPost方法的区别
Service方法：可以处理get/post方式的请求，**如果servlet中有Service方法，会优先调用service方法对请求进行处理。**

doGet方法：处理get方式的请求

doPost方法：处理post方式的请求

**注意：** 如果在覆写的service方法中调用了父类的service方法(super.service(arg0, arg1)),则service方法处理完后，会再次根据请求方式响应的doGet和doPost方法执行。所以，一般情况下，我们是不在覆写的service中调用父类的service方法的，避免出现405错误。
