---
title: Jsp基础知识二
thumbnail:
toc: true
date: 2019-11-20 22:23:08
tags:
	- Servlet&Jsp
---
Jsp的九大内置对象&四个作用域对象&Jsp的路径
## jsp的九大内置对象
内置对象：jsp文件在转译成其对应的Servlet文件的时候自动生成的并声明的对象。我们在jsp页面中直接使用即可

注意：内置对象在jsp页面中使用，使用局部代码块或者脚本段语句来使用。不能够在全局代码块中使用
<!-- more -->
1. `pageContext`:页面上下文对象，封存了其他内置对象。封存了当前jsp的运行信息
注意：每个Jsp文件单独拥有一个pageContext对象
作用域：当前页面
2. `request`：封存当前请求数据的对象。由tomcat服务器创建。一次请求
3. `session`:此对象用来存储用户的不同请求的共享数据的。一次会话
4. `application`：也就是ServletContext对象，一个项目只有一个。存储用户共享数据的对象，以及完成其他操作。项目内
5. `response`:响应对象，用来响应请求处理结果给浏览器的对象。设置响应头，重定向
6. `out`:响应对象，Jsp内部使用。带有缓冲区的响应对象，效率高于response对象
7. `page`:代表当前Jsp的对象。相当于java中的this
8. `exception`：异常对象。存储了当前运行的异常信息
	注意：使用此对象需要在page指定中使用属性isErrorPage="true"开启
9. `config`：也就是ServletConfig，主要是用来获取web.xml中的配置数据，完成一些初始化数据的读取


## 四个作用域对象
1. `pageContext`:当前页面.解决了在当前页面内的数据共享问题。获取其他内置对象
2. `request`:一次请求。一次请求的servlet的数据共享。通过请求转发，将数据流转给下一个servlet
3. `session`:一次会话.一个用户的不同请求的数据共享。将数据从一次请求流转给其他请求
4. `application`:项目内.不同用户的数据共享问题。将数据从一个用户流转给其他用户

作用：数据流转

## Jsp的路径
### 在jsp中资源路径可以使用相对路径完成跳转，但是：
问题一：资源的位置不可随意更改
问题二：需要使用../进行文件夹的跳出。使用比较麻烦

### 使用绝对路径(必须会)
`/虚拟项目名/项目资源路径`

```
<a href="/jsp/jspPro.jsp">jspPro.jsp</a>
<a href="/jsp/a/a.jsp">a.jsp</a><br />
```
注意：在jsp中资源的第一个/表示的是服务器根目录，相当于前面有`http://localhost:8080`
### 使用jsp中自带的全局路径声明
```java
<%
	String path = request.getContextPath();
	String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<base href="<%=basePath%>">
```
作用：给资源前面添加项目路径`http://127.0.0.1:8080/虚拟项目名/`
