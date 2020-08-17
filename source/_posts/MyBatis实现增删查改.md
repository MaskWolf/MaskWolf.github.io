---
title: MyBatis实现增删查改
toc: true
date: 2020-1-21 10:43:05
tags:
	- MyBatis
---
## 实体类
数据库字段名与类属性相同
```java
public class People {
    private int id;
    private String name;
    private int age;
}
```
<!-- more -->
## 增
```xml
<insert id="insOne" parameterType="people">
	insert into people values(default,#{name},#{age})
</insert>
```
```java
@Override
public int addPeople(People people) throws IOException {
    InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    int index = sqlSession.insert("top.ljc.mapper.PeopleMapper.insOne", people);
    sqlSession.commit();
    sqlSession.close();
    return index;
}
```

## 删
```xml
<delete id="delOneById" parameterType="int">
	delete from people where id=#{0}
</delete>
```
```java
@Override
public int delPeopleById(int id) throws IOException {
    InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    int index = sqlSession.insert("top.ljc.mapper.PeopleMapper.delOneById", id);
    sqlSession.commit();
    sqlSession.close();
    return index;
}
```

## 查
```xml
<select id="selAll" resultType="top.ljc.pojo.People">
	select * from people
</select>
```
```java
@Override
public List<People> show() throws IOException {
    InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
    //通过构建者设计模式实例化工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    //通过工厂设计模式生产SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    List<People> list = sqlSession.selectList("top.ljc.mapper.PeopleMapper.selAll");
    sqlSession.close();
    return list;
}
```
## 改
```xml
<update id="updNameById" parameterType="map">
  update people set name=#{name} where id=#{id}
</update>
```
```java
@Override
public int updNameById(int id, String newName) throws IOException {
    InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    Map<String, Object> map = new HashMap<>();
    map.put("id", id);
    map.put("name", newName);
    int index = sqlSession.insert("top.ljc.mapper.PeopleMapper.updNameById", map);
    sqlSession.commit();
    sqlSession.close();
    return index;
}
```

## 总结
1. 在 mybatis 中默认是关闭了 JDBC 的自动提交功能
1.1 每一个 SqlSession 默认都是不自动提交事务
1.2 session.commit() 提交事务
1.3 设置自动提交：`openSession(true)，setAutoCommit(true)`

2. mybatis 底层是对 JDBC 的封装.
2.1 JDBC 中 executeUpdate() 执行新增,删除,修改的 SQL 返回值为 int 类型,表示受影响的行数
2.2 mybatis 中 `<insert> <delete> <update>` 标签没有 resultType 属性,认为返回值都是 int

3. 在 openSession() 时 Mybatis 会创建 SqlSession
3.1 同时会创建一个 Transaction(事务对象),且设置 autoCommit 为 false
3.2 如果出现异常，应该 `session.rollback()` 回滚事务
