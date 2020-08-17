---
title: 使用Spring简化MyBatis
toc: true
date: 2020-2-24 08:47:10
tags:
	- Spring
---

## 导包
![](http://cdn.liaojincan.top/2020224090838.png" width='200' height='400'/>
<!-- more -->
mybatis所有jar，spring基本包，spring-jdbc，spring-tx，spring-aop，spring-web，spring整合mybatis的包

## 配置 web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
						http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
	<!-- 上下文参数 -->
	<context-param>
	<param-name>contextConfigLocation</param-name>
	<!-- spring 配置文件 -->
	<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
	<!-- 封装了一个监听器,帮助加载 Spring 的配置文件爱 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
</web-app>
```
## 编写spring配置文件applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans
	xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd">
	<!-- 数据源封装类 .数据源:获取数据库连接,spring-jdbc.jar 中 -->
	<bean id="dataSouce" class="org.springframework.jdbc.datasource.DriverMan gerDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://localhost:3306/ssm"></property>
		<property name="username" value="root"></property>
		<property name="password" value="smallming"></property>
	</bean>
	<!-- 创建 SqlSessionFactory 对象 -->
	<bean id="factory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 数据库连接信息来源于 dataSource -->
		<property name="dataSource" ref="dataSouce"></property>
	</bean>
	<!--
		扫描器相当于 mybatis.xml 中 mappers 下 package 标签,
		扫描 top.ljc.mapper 包后会给对应接口创建对象
	-->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 要扫描哪个包 -->
		<property name="basePackage" value="top.ljc.mapper"></property>
		<!-- 和 factory 产生关系 -->
		<property name="sqlSessionFactory" ref="factory"></property>
	</bean>
	<!-- 由 spring 管理 service 实现类 -->
	<bean id="airportService" class="top.ljc.service.impl.AirportServiceImpl">
		<property name="airportMapper" ref="airportMapper"></property>
	</bean>
</beans>
```
## 编写代码
1. 正常编写 pojo
2. 编写 mapper 包下时必须使用接口绑定方案或注解方案(必须有接口)
3. 正常编写 Service 接口和 Service 实现类
3.1 需要在 Service 实现类中声明 Mapper 接口对象,并生成get/set 方法
4. spring 无法管理 Servlet,在 service 中取出 Servie 对象

```java
@WebServlet("/airport")
public class AirportServlet extends HttpServlet{

	private AirportService airportService;

	@Override
	public void init() throws ServletException {
		//对 service 实例化
		//ApplicationContext ac = newClassPathXmlApplicationContext("applicationContext.xml");
		//spring 和 web 整合后所有信息都存放在
		webApplicationContext ApplicationContext ac = WebApplicationContextUtils.getRequiredWebApplicationContext(getServletContext());
		airportService=ac.getBean("airportService",AirportServiceImpl.class);
	}

	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
		req.setAttribute("list", airportService.show());
		req.getRequestDispatcher("index.jsp").forward(req,resp);
	}
}
```
