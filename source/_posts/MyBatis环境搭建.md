---
title: MyBatis环境搭建
toc: true
date: 2020-1-19 11:21:31
tags:
	- MyBatis
---
## {% post_link 在IDEA中搭建WEB项目开发环境 %}

## 导入MyBatis所需jar包
### 下载地址
[https://github.com/mybatis/mybatis-3/releases](https://github.com/mybatis/mybatis-3/releases)
<!-- more -->
### 各jar包作用
![](http://cdn.liaojincan.top/20200119152237.png)

注意:
1. 项目所需jar包全在mybatis/lib目录下
2. 除mybatis所需jar包外还需要导入数据库驱动包

## 创建mybatis.xml全局配置文件
### 引入 DTD 或 Schema（保证XML文件格式正确）
在IDEA中可以从网络上抓取并自动配置，也可以手动配置本地dtd或schema
![](http://cdn.liaojincan.top/20200119154621.png)

### mybatis.xml文件内容
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!-- 开启log4j日志支持 -->
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <typeAliases>
        <!-- 给top.ljc.pojo包下的所有类取别名为类名(不区分大小写) -->
        <package name="top.ljc.pojo"/>
    </typeAliases>
    <!-- default 引用 environment 的 id,当前所使用的环境 -->
    <environments default="default">
        <environment id="default">
            <!-- 使用原生JDBC事务 -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置数据库源 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="199827"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <package name="top.ljc.mapper"/>
    </mappers>
</configuration>
```

## mapper包下创建"实体类名Mapper.xml"文件
### 作用
类似数据访问层实现类，在其中编写需要执行的SQL命令

### 文件内容
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namesapce:理解成实现类的全路径(包名+类名) -->
<mapper namespace="" >
  <!-- id:方法名
  parameterType:定义参数类型
  resultType:返回值类型
  如果方法返回值是 list,在 resultType 中写 List 的泛型
  因为 mybatis是对 jdbc 封装,一行一行读取数据
  -->
  <select id="" resultType="">
  SQL
  </select>
</mapper>
```
