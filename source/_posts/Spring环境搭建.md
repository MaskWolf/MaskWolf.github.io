---
title: Spring环境搭建
toc: true
date: 2020-2-11 18:21:47
tags:
	- Spring
---
## {% post_link 在IDEA中搭建WEB项目开发环境 %}

## 下载jar包
### 下载地址
[https://repo.spring.io/webapp/#/artifacts/browse/tree/General/libs-release-local/org/springframework/spring](https://repo.spring.io/webapp/#/artifacts/browse/tree/General/libs-release-local/org/springframework/spring)
<!-- more -->
### 导入核心包
- spring-beans-4.1.6.RELEASE.jar
- spring-context-4.1.6.RELEASE.jar
- spring-core-4.1.6.RELEASE
- spring-expression-4.1.6.RELEASE.jar

PS：此外，spring-framework工作还**必须有** commons-logging-1.1.3.jar

## 在src下新建xml文件
1. xml文件名称和路径自定义，但建议名称为applicationContext.xml，路径在src下
2. applicationContext.xml 中配置的信息最终存储到了 `AppliationContext` 容器中
3. spring 配置文件是基于 schema
	3.1 schema 文件扩展名`.xsd`
	3.2 可以把 schema 理解成 DTD 的升级版（比 DTD 具备更好的扩展性）
	3.3 每次引入一个 xsd 文件就是一个 namespace(xmlns)
4. 配置文件中只需要引入基本 schema
	4.1 通过`<bean/>`标签创建对象
	4.2 默认在配置文件被加载时创建对象

实体类
```java
public class People {
    private int id;
    private String name;
}
```

配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd">
	 <!-- id设置对象的标识，class指定创建的类对象的全限定路径 -->
	 <bean id="people" class="top.ljc.pojo.People"/>
</beans>
```

## 创建测试类
1. getBean(“`<bean>`标签 id 值”,返回值类型)，如果没有第二个参数默认返回Object
2. getBeanDefinitionNames()返回一个Spring容器中目前所有管理的所有对象的名称的数组

```java
public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        People people = applicationContext.getBean("people", People.class);
        System.out.println(people); //People{id=0, name='null'}
    }
}
```

至此，springframework基础环境搭建成功！
