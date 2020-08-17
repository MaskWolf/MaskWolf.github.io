---
title: MyBatis实现多表查询
toc: true
date: 2020-2-7 11:55:08
tags:
	- MyBatis
---

1. Mybatis 实现多表查询方式
	1.1 业务装配.对两个表编写单表查询语句,在业务(Service)把查询的两个结果进行关联
	1.2 使用 Auto Mapping 特性,在实现两表联合查询时通过别名完成映射
	1.3 使用 MyBatis 的`<resultMap>`标签进行实现
2. 多表查询时,类中包含另一个类的对象的分类
	2.1 单个对象
	2.2 集合对象
<!-- more -->

## resultMap标签
1. `<resultMap>`标签写在 mapper.xml 中,由程序员控制 SQL 查询结果与实体类的映射关系
2. 使用`<resultMap>`标签时,`<select>`标签不写 resultType 属性,而是使用 resultMap 属性引用`<resultMap>`标签
3. MyBatis默认使用 Auto Mapping 特性

### 使用 resultMap 实现单表映射关系
1. 数据库设计
	```sql
	create table teacher(
		id1 int(10) primary key auto_increment,
		name1 varchar(20)
	);
	```
2. 实体类设计
	```java
	public class Teacher {
	  private int id;
	  private String name;
	}
	```
3. mapper.xml 代码
	```xml
	<resultMap type="teacher" id="mymap">
		<!-- 主键使用 id 标签配置映射关系 -->
		<id column="id" property="id1" />
		<!-- 其他列使用 result 标签配置映射关系 -->
		<result column="name" property="name1"/>
	</resultMap>

	<select id="selAll" resultMap="mymap">
		select * from teacher
	</select>
	```
### 使用 resultMap 实现关联单个对象(N+1 方式)
1. N+1 查询方式,先查询出某个表的全部信息,根据这个表的信息查询另一个表的信息
2. 与业务装配的区别: 把在 service 里面写的代码,变成了由 mybatis 完成装配
3. 实现步骤
	3.1 在 Student 实现类中包含了一个 Teacher 对象
	```java
	public class Student {
		private int id;
		private String name;
		private int age;
		private int tid;
		private Teacher teacher;
	}
	```
	3.2 在 TeacherMapper 中提供一个查询
	```xml
	<select id="selById" resultType="teacher" parameterType="int">
		select * from teacher where id=#{0}
	</select>
	```
	3.3 在 StudentMapper 中配置映射关系
	- `<association>` 标签在装配一个对象时使用
	- property: 对象在类中的属性名
	- select: 通过哪个查询查询出这个对象的信息
	- column: 把当前表的哪个列的值做为参数传递给另一个查询
	- 使用 N+1 方式时，如果列名和属性名相同可以不配置,直接使用 Auto mapping 特性
	- 但是 mybatis 默认只会给列装配一次，需要装配多次的列必须配置
	```xml
	<resultMap type="student" id="stuMap">
		<id property="id" column="id"/>
		<result property="name" column="name"/>
		<result property="age" column="age"/>
		<result property="tid" column="tid"/>
		<!-- 如果关联一个对象 -->
		<association property="teacher" select="top.ljc.mapper.TeacherMapper.selById" column="tid"></association>
	</resultMap>
	<select id="selAll" resultMap="stuMap">
		select * from student
	</select>
	```
	把上面代码简化成
	```xml
	<resultMap type="student" id="stuMap">
		<result column="tid" property="tid"/>
		<!-- 如果关联一个对象 -->
		<association property="teacher" select="top.ljc.mapper.TeacherMapper.selById" column="tid"></association>
	</resultMap>
	<select id="selAll" resultMap="stuMap">
		select * from student
	</select>
	```
### 使用 resultMap 实现关联单个对象(联合查询方式)
只需要编写一个 SQL,在 StudentMapper 中添加下面效果
1. `<association/>`只要装配一个对象时就用这个标签
2. 此时把`<association/>`当成小的`<resultMap>`看待
3. javaType 属性: 指定`<association/>`装配完成后返回对象的类型，取值是一个类(或类的别名)
```xml
<resultMap type="Student" id="stuMap1">
	<id column="sid" property="id"/>
	<result column="sname" property="name"/>
	<result column="age" property="age"/>
	<result column="tid" property="tid"/>
	<association property="teacher" javaType="Teacher" >
		<id column="tid" property="id"/>
		<result column="tname" property="name"/>
	</association>
</resultMap>
<select id="selAll1" resultMap="stuMap1">
	select s.id sid,s.name sname,age age,t.id tid,t.name tname FROM student s left outer join teacher t on s.tid=t.id
</select>
```
### N+1方式和联合查询方式对比
N+1: 需求不确定时
联合查询: 需求中确定查询时两个表一定都查询

### N+1 名称由来
1. 举例:学生中有 3 条数据
2. 需求:查询所有学生信息及授课老师信息
3. 需要执行的 SQL 命令
3.1 查询全部学生信息: `select * from 学生`
3.2 执行 3 遍 `select * from 老师 where id=学生的外键`
4. 使用多条 SQl 命令查询两表数据时,如果希望把需要的数据都查询出来,需要执行 N+1 条 SQl 才能把所有数据库查询出来
5. 缺点: 效率低
6. 优点: 如果有的时候不需要查询学生的同时查询老师,只需要执行一个 `select * from student;`
7. 适用场景: 有的时候需要查询学生同时查询老师,有的时候只需要查询学生
8. 如果解决 N+1 查询带来的效率低的问题
8.1 默认带的前提: 每次都是两个都查询
8.2 使用两表联合查询

## 使用`<resultMap>`查询关联集合对象(N+1)
1. 在 Teacher 中添加 List<Student>
	```java
	public class Teacher {
		private int id;
		private String name;
		private List<Student> list;
	}
	```
2. 在 StudentMapper.xml 中添加通过 tid 查询
	```xml
	<select id="selByTid" parameterType="int" resultType="student">
		select * from student where tid=#{0}
	</select>
	```
3. 在 TeacherMapper.xml 中添加查询全部
 `<collection/>` 是当属性是集合类型时使用的标签
	```xml
	<resultMap type="teacher" id="mymap">
		<id column="id" property="id"/>
		<result column="name" property="name"/>
		<collection property="list" select="top.ljc.mapper.StudentMapper.selByTid" column="id"></collection>
	</resultMap>
	<select id="selAll" resultMap="mymap">
		select * from teacher
	</select>
	```
## 使用<resultMap>实现加载集合数据(联合查询方式)
在 teacherMapper.xml 中添加
- MyBatis 可以通过主键判断对象是否被加载过
- 不需要担心创建重复 Teacher
```xml
<resultMap type="teacher" id="mymap1">
	<id column="tid" property="id"/>
	<result column="tname" property="name"/>
	<collection property="list" ofType="student" >
	<id column="sid" property="id"/>
		<result column="sname" property="name"/>
		<result column="age" property="age"/>
		<result column="tid" property="tid"/>
	</collection>
</resultMap>
<select id="selAll1" resultMap="mymap1">
	select t.id tid,t.name tname,s.id sid,s.name sname,age,tid from teacher t LEFT JOIN student s on t.id=s.tid;
</select>
```
## 使用 Auto Mapping 结合别名实现多表查询
5.1 只能使用多表联合查询方式
5.2 要求: 查询出的列名和属性名相同
5.3 实现方式
`.`在 SQL 是关键字符,两侧添加反单引号
```xml
<select id="selAll" resultType="student">
	select t.id `teacher.id`,t.name `teacher.name`,s.id id,s.name name,age,tid from student s LEFT JOIN teacher t on t.id=s.tid
</select>
```
