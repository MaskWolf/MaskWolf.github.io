---
title: MyBatis注解
toc: true
date: 2020-2-8 15:30:51
tags:
	- MyBatis
---

1. 注解: 为了简化配置文件
2. Mybatis 的注解简化 mapper.xml 文件
2.1 如果涉及动态 SQL 依然使用 mapper.xml
<!-- more -->
3. mapper.xml 和注解可以共存
4. 使用注解时 mybatis.xml 中`<mappers>`使用
4.1 `<package/>`
4.2 `<mapper class=""/>`
5. 实现查询
`@Select("select * from teacher") List<Teacher> selAll();`
6. 实现新增
`@Insert("insert into teacher values(default,#{name})") int insTeacher(Teacher teacher);`
7. 实现修改
`@Update("update teacher set name=#{name} where id=#{id}") int updTeacher(Teacher teacher);`
8. 实现删除
`@Delete("delete from teacher where id=#{0}") int delById(int id);`
9. 使用注解实现`<resultMap>`功能
9.1 以 N+1 举例
9.2 在 StudentMapper 接口添加查询
`@Select("select * from student where tid=#{0}") List<Student> selByTid(int tid);`
9.3 在 TeacherMapper 接口添加
	- @Results() 相当于`<resultMap>`
		- @Result() 相当于`<id/>`或`<result/>`
		- @Result(id=true) 相当于`<id/>`
			- @Many() 相当于`<collection/>`
			- @One() 相当于`<association/>`
```java
@Results(value={
	@Result(id=true,property="id",column="id"),
	@Result(property="name",column="name"),
	@Result(property="list",column="id",many=@Many(select="top.ljc.mapper.StudentMapper.selByTid"))
})
@Select("select * from teacher")
List<Teacher> selTeacher();
```
