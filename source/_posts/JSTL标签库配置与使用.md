---
title: JSTL标签库配置与使用
thumbnail:
toc: true
date: 2019-12-2 10:26:05
tags:
	- Servlet&Jsp
---
## 在IDEA的Web项目中配置JSTL
### 下载JSTL所需jar包
官网下载地址 [http://tomcat.apache.org/taglibs/standard/](http://tomcat.apache.org/taglibs/standard/)
<!-- more -->
![](http://cdn.liaojincan.top/2019-11-28134449.png)
![](http://cdn.liaojincan.top/2019-11-28134926.png)

### 将所需的jar包导入工程中
![](http://cdn.liaojincan.top/20191128140130.png)

### 为需要用到的标签库在项目中配置uri
![](http://cdn.liaojincan.top/20191128140704.png)
![](http://cdn.liaojincan.top/20191128141342.png)

## JSTL标签库的使用
### 核心标签库
在JSP页面声明引入核心标签库
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

1. 基本标签
	- 将数据输出给客户端
	`<c:out value="数据" default="默认值"></c:out>`
	数据可为常量值也可是EL表达式
	- 存储数据到作用域对象中
	`<c:set var="键名" value="数据" scope="作用域对象"></c:set>`
	scope可选值page、request、session、application
	- 删除作用域中的指定键的数据
	`<c:remove var="键名" scope="作用域对象"/>`
	**注意：** 如果在不指定作用域的情况使用该标签删除数据，会将四个作用域对象中的符合要求的数据全部删除

2. 逻辑标签
	- 进行逻辑判断，相当于java代码的单分支判断。
		```java
		<c:if test="${表达式}">
			前端代码
		</c:if>
		```
		**注意：** 逻辑判断标签需要依赖于EL的逻辑运算，也就是表达式中涉及到的数据必须从作用域中获取
	- 进行多条件的逻辑判断，类似java中的多分支语句
		```java
		<c:choose>
			<c:when test="">执行内容</c:when>
			<c:when test="">执行内容</c:when>
			...
			<c:otherwise>执行内容</c:otherwise>
		</c:choose>
		```
		**注意：** 条件成立只会执行一次，都不成立则执行otherwise

3. 循环标签
	作用：循环内容进行处理
	```java
	<c:forEach begin="1" end="4" step="2">
			循环体
	</c:forEach>
	```
	使用:
	- begin：声明循环开始位置
	- end：声明循环结束位置
	- step：设置步长
	- varStatus：声明变量记录每次循环的数据(角标，次数，是否是第一次循环，是否是最后一次循环)
	- items:声明要遍历的对象。结合EL表达式获取对象
	- var:声明变量记录每次循环的结果。存储在作用域中，需要使用EL表达式获取

	注意:数据存储在作用域中，需要使用EL表达式获取
	例如：`${vs.index}--${vs.count}--${vs.first}--${vs.last}`
