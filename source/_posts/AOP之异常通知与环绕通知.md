---
title: AOP之异常通知与环绕通知
toc: true
date: 2020-2-27 08:30:10
categories: 
tags:
	- Spring
---
## 异常通知
异常通知：只有当切点报异常才能触发异常通知

### AspectJ方式实现
1. 新建类,在类写任意名称的方法
```java
public class MyThrowAdvice{
	public void myexception(Exception e1){
		System.out.println("执行异常通知"+e1.getMessage());
	}
}
```
<!-- more -->
2. 在 spring 配置文件中配置
2.1 `<aop:aspect>`的 ref 属性表示:方法在哪个类中
2.2 `<aop: xxxx/>`表示什么通知
2.3 method: 当触发这个通知时,调用哪个方法
2.4 throwing: 异常对象名，必须和通知中方法参数名相同(可以不在通知中声明异常对象)
```xml
<bean id="mythrow" class="top.ljc.advice.MyThrowAdvice"></bean>
<aop:config>
	<aop:aspect ref="mythrow">
	<aop:pointcut expression="execution(*top.ljc.test.Demo.demo1())" id="mypoint"/>
	<aop:after-throwing method="myexception" pointcut-ref="mypoint" throwing="e1"/>
	</aop:aspect>
</aop:config>
<bean id="demo" class="top.ljc.test.Demo"></bean>
```
### Schema-based方式实现
1. 新建一个类实现 throwsAdvice 接口
1.1 必须自己写方法，且必须叫afterThrowing
1.2 有两种参数方式，必须是1个或4个
1.3 异常类型要与切点报的异常类型一致
```java
public class MyThrow implements ThrowsAdvice{
//public void afterThrowing(Method m, Object[] args,Object target, Exception ex) {
//	System.out.println("执行异常通知");
//}
	public void afterThrowing(Exception ex) throws Throwable {
		System.out.println("执行异常通过-schema-base 方式");
	}
}
```
2. 在 ApplicationContext.xml 配置
```xml
<bean id="mythrow" class="top.ljc.advice.MyThrow"></bean>
<aop:config>
	<aop:pointcut expression="execution(*top.ljc.test.Demo.demo1())" id="mypoint"/>
	<aop:advisor advice-ref="mythrow" pointcut-ref="mypoint" />
</aop:config>
<bean id="demo" class="top.ljc.test.Demo"></bean>
```

## 环绕通知(Schema-based方式)
把前置通知和后置通知都写到一个通知中,组成了环绕通知
1. 新建一个类实现 MethodInterceptor
```java
public class MyArround implements MethodInterceptor {
@Override
	public Object invoke(MethodInvocation arg0) throws Throwable {
		System.out.println("环绕-前置");
		Object result = arg0.proceed();//放行,调用切点方式
		System.out.println("环绕-后置");
		return result;
	}
}
```
2. 配置 applicationContext.xml
```xml
<bean id="myarround" class="top.ljc.advice.MyArround"></bean>
<aop:config>
	<aop:pointcut expression="execution(*top.ljc.test.Demo.demo1())" id="mypoint"/>
	<aop:advisor advice-ref="myarround" pointcut-ref="mypoint" />
</aop:config>
<bean id="demo" class="top.ljc.test.Demo"></bean>
```
