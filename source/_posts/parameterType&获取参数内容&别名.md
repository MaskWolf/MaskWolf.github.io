---
title: parameterType&获取参数内容&别名
toc: true
date: 2020-1-20 13:08:20
tags:
	- MyBatis
---
## parameterType
1. 在XXXMapper.xml中，`<select><delete><update>`等标签的`parameterType`属性可以控制参数类型
<!-- more -->
2. `SqlSession`的`selectList()`和`selectOne()`的第二个参数以及`selectMap()`的第三个参数都表示方法的参数

### 示例
通过id查询人员详细信息，传递的参数是整数类型，返回值类型是自定义的People实体类
```xml
<select id="selById" parameterType="int" resultMap="people">
		select * from people where id = #{0}
</select>
```
```java
People p = session.selectOne("a.b.selById",1);
System.out.println(p);
```

## 获取参数内容
### `#{}`获取参数内容
1. 使用索引,从 0 开始 #{0}表示第一个参数
2. 也可以使用#{param1}第一个参数
3. 如果只有一个参数(基本数据类型或 String),mybatis对#{}里面内容没有要求只要写内容即可
4. 如果参数是对象#{属性名}
5. 如果参数是 map 写成#{key}

### `${}`获取参数内容
与`#{}`获取参数语法一致

### `#{}`和`${}`的区别
1. `#{}`获取参数的内容支持索引获取,param1获取指定位置参数，并且 SQL 使用?占位符
2. `${}` 字符串拼接不使用?占位符,默认找${内容}内容的get/set方法,如果写数字,就是一个数字

## typeAliases别名
### 给某个类起别名
`alias`属性指定别名名称，如不填写，系统默认将类名的全小写作为别名
```xml
<typeAliases>
	<typeAlias type="top.ljc.pojo.People" alias="peo"/>
</typeAliases>
```

### 给某个包下的所有类起别名
**别名为类名,且不区分大小写**
```xml
<typeAliases>
	<package name="top.ljc.pojo" />
</typeAliases>
```
