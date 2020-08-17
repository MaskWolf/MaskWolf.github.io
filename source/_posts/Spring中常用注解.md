---
title: Spring中常用注解
toc: true
date: 2020-2-27 09:35:34
tags:
	- Spring
---
1. @Component 创建类对象,相当于配置`<bean/>`
2. @Service 与@Component 功能相同
  2.1 写在 ServiceImpl 类上
3. @Repository 与@Component 功能相同
  3.1 写在数据访问层类上
<!-- more -->
4. @Controller 与@Component 功能相同
4.1 写在控制器类上
5. @Resource(不需要写对象的 get/set)
  5.1 java 中的注解
  5.2 默认按照 byName 注入,如果没有名称对象,按照 byType 注入
  5.2.1 建议把对象名称和 spring 容器中对象名相同
6. @Autowired(不需要写对象的 get/set)
6.1 spring 的注解
6.2 默认按照 byType 注入
7. @Value() 获取 properties 文件中内容
8. @Pointcut() 定义切点
9. @Aspect() 定义切面类
10. @Before() 前置通知
11. @After 后置通知
12. @AfterReturning 后置通知,必须切点正确执行
13. @AfterThrowing 异常通知
14. @Arround 环绕通知
