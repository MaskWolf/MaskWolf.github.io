---
title: MyBatis接口绑定及多参数传递
toc: true
date: 2020-2-1 15:26:16
tags:
	- MyBatis
---
作用:实现创建一个接口后把 mapper.xml 由 mybatis 生成接口的实现类,通过调用接口对象就可以获取 mapper.xml 中编写的 sql

ps:后面 mybatis 和 spring 整合时使用的是这个方案
<!-- more -->
## 实现步骤
1. 创建一个接口
2. 接口包名和接口名与 mapper.xml 中`<mapper>`标签的namespace属性值相同
3. 接口中方法名和 mapper.xml 标签的 id 属性相同
4. 在 mybatis.xml 中使用`<package>`进行扫描接口和 mapper.xml

## 代码实现步骤
1. 在 mybatis.xml 中`<mappers>`下使用`<package>`
```xml
<mappers>
	<package name="top.ljc.mapper"/>
</mappers>
```
2. 在 top.ljc.mapper 下新建接口
```java
public interface LogMapper {
	List<Log> selAll();
}
```
3. 在 top.ljc.mapper 新建一个 LogMapper.xml
3.1 namespace 必须和接口全限定路径(包名+类名)一致
3.2 id 值必须和接口中方法名相同
3.3 如果接口中方法为多个参数,可以省略 parameterType
```xml
<mapper namespace="top.ljc.mapper.LogMapper">
	<select id="selAll" resultType="log">
		select * from log
	</select>
</mapper>
```
## 多参数实现办法

### #{}中使用 0,1,2 或 param1,param2
1. 在接口中声明方法
```java
List<Log> selByAccInAccout(String accin,String accout);
```
2. 在 mapper.xml 中添加
```xml
<!-- 当多参数时,不需要写 parameterType -->
<select id="selByAccInAccout" resultType="log" >
	select * from log where accin=#{0} and accout=#{1}
</select>
```
### 使用注解方式
1. 在接口中声明方法
```java
/**
* mybatis 把参数转换为 map 了,其中@Param("key") 参数内容就是 map 的 value
* @param accin123
* @param accout3454235
* @return
*/
List<Log> selByAccInAccout(@Param("accin") Stringaccin123,@Param("accout") String accout3454235);
```
2. 在 mapper.xml 中添加
	#{}里面写@Param(“内容”)参数中内容
```xml
<!-- 当多参数时,不需要写 parameterType -->
<select id="selByAccInAccout" resultType="log" >
	select * from log where accin=#{accin} and accout=#{accout}
</select>
```
