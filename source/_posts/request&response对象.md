---
title: request&response对象
toc: true
date: 2019-11-13 17:36:40
tags:
  - Servlet&Jsp
---
## request对象
作用：request对象中封存了当前请求的所有请求信息

注意：request对象由tomcat服务器创建，并作为实参传递给处理请求的servlet的service方法。
<!-- more -->
### 获取请求头数据
```java
req.getMethod();//获取请求方式
req.getRequestURL();//获取请求URL信息
req.getRequestURI();//获取请求URI信息
req.getScheme();//获取协议
```

### 获取请求行数据
```java
req.getHeader("键名");//返回指定的请求头信息
req.getHeaderNames();//返回请求头的键名的枚举集合
```

### 获取用户数据
```java
req.getParameter("键名");//返回指定的用户数据
req.getParameterValues("键名");//返回同键不同值的请求数据(多选)，返回的数组
req.getParameterNames()//返回所有用户请求数据的枚举集合
```

注意：如果要获取的请求数据不存在，不会报错，会返回null

## response对象
作用：用来响应数据到浏览器的一个对象

### 设置响应头
```java
setHeader(String name,String value);//在响应头中添加响应信息，但是同键会覆盖
addHeader(String name,String value);//在响应头中添加响应信息，但是不会覆盖。
```
### 设置响应状态
```java
sendError(int num,String msg);//自定义响应状态码。
```
### 设置响应实体
```java
resp.getWrite().write(String str);//响应具体的数据给浏览器
```
### 设置响应编码格式
```java
resp.setContentType("text/html;charset=utf-8");
```
