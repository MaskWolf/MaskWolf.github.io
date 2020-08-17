---
title: Spring之AOP
toc: true
date: 2020-2-27 08:14:10
tags:
	- Spring
---
## AOP
1. AOP:中文名称面向切面编程
2. 英文名称:(Aspect Oriented Programming)
3. 正常程序执行流程都是纵向执行流程
	3.1 面向切面编程,在原有纵向执行流程中添加横切面
	3.2 不需要修改原有程序代码
	- 高扩展性
	- 原有功能相当于释放了部分逻辑，让职责更加明确
<!-- more -->
4. 面向切面编程是什么?
	在程序原有纵向执行流程中,针对某一个或某一些方法添加通知，形成横切面过程就叫做面向切面编程
![](http://cdn.liaojincan.top/2020224113621.png)

### 常用概念
1. 原有功能:切点，pointcut
2. 前置通知:在切点之前执行的功能，before advice
3. 后置通知:在切点之后执行的功能，after advice
4. 如果切点执行过程中出现异常,会触发异常通知throws advice
5. 所有功能总称叫做切面
6. 织入:把切面嵌入到原有功能的过程叫做织入

### 实现AOP的两种放式
1. Schema-based
	- 每个通知都需要实现接口或类
	- 配置 spring 配置文件时在`<aop:config>`配置
2. AspectJ
	- 每个通知不需要实现接口或类
	- 配置 spring 配置文件是在`<aop:config>`的子标签`<aop:aspect>`中配置

## 基于Schema-based方式实现
1. 导入 jar
2. 新建通知类并实现接口
	2.1 新建前置通知类
	- arg0: 切点方法对象 Method 对象
	- arg1: 切点方法参数
	- arg2:切点在哪个对象中
	```java
	public class MyBeforeAdvice implementsMethodBeforeAdvice {
		@Override
		public void before(Method arg0, Object[] arg1, Objectarg2) throws Throwable {
			System.out.println("执行前置通知");
		}
	}
	```
	2.2 新建后置通知类
	- arg0: 切点方法返回值
	- arg1:切点方法对象
	- arg2:切点方法参数
	- arg3:切点方法所在类的对象
	```java
	public class MyAfterAdvice implements AfterReturningAdvice {
	@Override
		public void afterReturning(Object arg0, Method arg1, Object[] arg2, Object arg3) throws Throwable {
			System.out.println("执行后置通知");
		}
	}
	```
3. 配置 spring 配置文件
	3.1 引入 aop 命名空间
	3.2 配置通知类的<bean>
	3.3 配置切面
	3.4 * 通配符,匹配任意方法名,任意类名,任意一级包名
	3.5 如果希望匹配任意方法参数 (..)
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:aop="http://www.springframework.org/schema/aop"
		xsi:schemaLocation="http://www.springframework.org/schema/beans
			http://www.springframework.org/schema/beans/spring-beans.xsd
			http://www.springframework.org/schema/aop
			http://www.springframework.org/schema/aop/spring-aop.xsd">
		<!-- 配置通知类对象,在切面中引入 -->
		<bean id="mybefore" class="top.ljc.advice.MyBeforeAdvice"></bean>
		<bean id="myafter" class="top.ljc.advice.MyAfterAdvice"></bean>
		<!-- 配置切面 -->
		<aop:config>
			<!-- 配置切点 -->
			<aop:pointcut expression="execution(*top.ljc.test.Demo.demo2())" id="mypoint"/>
			<!-- 配置通知 -->
			<aop:advisor advice-ref="mybefore" pointcut-ref="mypoint"/>
			<aop:advisor advice-ref="myafter" pointcut-ref="mypoint"/>
		</aop:config>
		<!-- 配置 Demo 类,测试使用 -->
		<bean id="demo" class="top.ljc.test.Demo"></bean>
	</beans>
	```
4. 编写测试代码
```java
public class Test {
	public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		Demo demo = ac.getBean("demo",Demo.class);
		demo.demo1();
		demo.demo2();
		demo.demo3();
	}
}
```
5. 运行结果
demo1
执行前置通知
demo2
执行后置通知
demo3

## 使用 AspectJ 方式实现
1. 新建类,不用实现接口
	1.1 类中方法名任意
```java
public class MyAdvice {
	public void mybefore(String name1,int age1){
		System.out.println("前置"+name1 );
	}
	public void mybefore1(String name1){
		System.out.println("前置:"+name1);
	}
	public void myaftering(){
		System.out.println("后置 2");
	}
	public void myafter(){
		System.out.println("后置 1");
	}
	public void mythrow(){
		System.out.println("异常");
	}
	public Object myarround(ProceedingJoinPoint p) throws Throwable{
		System.out.println("执行环绕");
		System.out.println("环绕-前置");
		Object result = p.proceed();
		System.out.println("环绕后置");
		return result;
	}
}
```
2. 配置 spring 配置文件
	2.1 `<aop:after/>`后置通知,是否出现异常都执行
	2.2 `<aop:after-returing/>`后置通知,只有当切点正确执行时执行
	2.3 `<aop:after/>`,`<aop:after-returing/>`,`<aop:after-throwing/>`执行顺序和配置顺序有关
	2.4 execution()括号中内容不能包括args
	2.5 execution()表达式中间使用and，不能使用`&&`，应该让spring把and解析成`&&`
	2.6 args(名称) 名称是自定义的，顺序和 demo1(参数,参数)对应
	2.7 `<aop:before/>`中 arg-names="名称"，名称来源于expression=""中 args()，名称必须一样
	- args() 有几个参数,arg-names 里面必须有几个参数
	- arg-names=""里面名称必须和通知方法参数名对应
```xml
<aop:config>
	<aop:aspect ref="myadvice">
	<aop:pointcut expression="execution(*top.ljc.test.Demo.demo1(String,int)) and args(name1,age1)" id="mypoint"/>
	<aop:pointcut expression="execution(*top.ljc.test.Demo.demo1(String)) and args(name1)" id="mypoint1"/>
	<aop:before method="mybefore" pointcut-ref="mypoint" arg-names="name1,age1"/>
	<aop:before method="mybefore1" pointcut-ref="mypoint1" arg-names="name1"/>
	</aop:aspect>
</aop:config>
```

## 使用注解(基于Aspect)
1. spring 不会自动去寻找注解,必须告诉 spring 哪些包下的类中可能有注解
引入xmlns:context
`<context:component-scan base-package="top.ljc.advice"></context:component-scan>`
2. @Component
	2.1 相当于`<bean/>`
	2.2 如果没有参数,把类名首字母变小写,相当于`<bean id=””/>`
	2.3 @Component(“自定义名称”)
### 实现步骤
1. 在 spring 配置文件中设置注解在哪些包中
```xml
<context:component-scan base-package="top.ljc.advice,top.ljc.test">
</context:component-scan>
```
2. 在 Demo 类中添加@Componet
	2.1 在方法上添加@Pointcut(“”) 定义切点
```java
@Component
public class Demo {
	@Pointcut("execution(*top.ljc.test.Demo.demo1())")
	public void demo1() throws Exception{
		//int i = 5/0;
		System.out.println("demo1");
	}
}
````
3. 在通知类中配置
	3.1 @Component 类被 spring 管理
	3.2 @Aspect 相当于`<aop:aspect/>`表示通知方法在当前类中
```java
@Component
@Aspect
public class MyAdvice {
	@Before("top.ljc.test.Demo.demo1()")
	public void mybefore(){
		System.out.println("前置");
	}
	@After("top.ljc.test.Demo.demo1()")
	public void myafter(){
		System.out.println("后置通知");
	}
	@AfterThrowing("top.ljc.test.Demo.demo1()")
	public void mythrow(){
		System.out.println("异常通知");
	}
	@Around("top.ljc.test.Demo.demo1()")
	public Object myarround(ProceedingJoinPoint p) throws Throwable{
		System.out.println("环绕-前置");
		Object result = p.proceed();
		System.out.println("环绕-后置");
		return result;
	}
}
```
