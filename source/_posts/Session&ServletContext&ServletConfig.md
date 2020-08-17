---
title: Session&ServletContext&ServletConfig
thumbnail:
toc: true
date: 2019-11-20 09:40:39
tags:
	- Servlet&Jsp
---

## 一、Session技术
作用：解决了一个用户的不同请求的数据共享问题

原理：用户第一次访问服务器，服务器会创建一个session对象给此用户，并将该session对象的JSESSIONID使用Cookie技术存储到浏览器中，保证用户的其他请求能够获取到同一个session象，也保证了不同请求能够获取到共享的数据
<!-- more -->
使用：
1. 创建session对象/获取session对象
`HttpSession hs=req.getSession();`
注意：
	1. 如果请求中拥有session的标识符也就是JSESSIONID，则返回其对应的session队列
	2. 如果请求中没有session的标识符也就是JSESSIONID，则创建新的session对象，并将其JSESSIONID作为cookie数据存储到浏览器内存中
	3. 如果session对象失效了，也会重新创建一个session对象，并将其JSESSIONID存储在浏览器内存中

2. 设置session存储时间
`hs.setMaxInactiveInterval(int seconds);`
注意：在指定的时间内session对象没有被使用则销毁，如果使用了则重新计时

3. 设置session强制失效
`hs.invalidate();`

4. 存储和获取数据
存储：`hs.setAttribute(String name,Object value);`
获取：`hs.getAttribute(String name);`，返回的数据类型为Object

注意：存储的动作和取出的动作发生在不同的请求中，但是存储要先于取出执行

特点：
1. 数据存储在服务器端，由服务器进行创建session对象
2. session技术依赖Cookie技术
3. 生命周期为一次会话，在JSESSIONID和SESSION对象不失效的情况下作用域为整个项目内
4. session对象在服务器端的默认存储时间是30分钟
5. JSESSIONID存储在了Cookie的临时存储空间中，浏览器关闭即失效

## 二、ServletContext对象
作用：解决不同用户共享相同数据的问题

使用:
1. 获取ServletContext对象
```java
ServletContext sc=this.getServletContext();
ServletContext sc2=this.getServletConfig().getServletContext();
ServletContext sc3=req.getSession().getServletContext();
```
2. 使用ServletContext对象完成数据共享
数据存储：`sc.setAttribute(String name, Object value);`
数据获取：`sc.getAttribute("str") `，返回的是Object类型

注意：
1. 不同的用户可以给ServletContext对象进行数据的存取
2. 获取的数据不存在返回null

特点:
1. ServletContext由服务器创建，所有用户共享
2. 生命周期为服务器开启到服务器关闭，作用域为整个项目内

## 三、ServletConfig对象
作用：获取在web.xml中给每个servlet单独配置的数据

使用：
1. 获取ServletConfig对象
`ServletConfig sc=this.getServletConfig();`
2. 获取web.xml中的配置数据
`String code=sc.getInitParameter("config");`

## 四、其他
1. 获取项目中web.xml文件中的全局配置数据
`sc.getInitParameter(String name);`
根据键的名字返回web.xml中配置的全局数据的值，返回String类型，如果数据不存在返回null
2. 获取项目webroot下的资源的绝对路
`String path=sc.getRealPath(String path);	`
获取的路径为项目根目录，path参数为项目根目录中的路径
3. 获取webroot下的资源的流对象
`InputStream is = sc.getResourceAsStream(String path);`
此种方式只能获取项目根目录下的资源流对象，class文件的流对象需要使用类加载器获取，path参数为项目根目录中的路径
