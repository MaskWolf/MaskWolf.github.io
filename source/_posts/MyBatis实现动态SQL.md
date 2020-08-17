---
title: MyBatis实现动态SQL
toc: true
date: 2020-2-3 12:11:37
tags:
	- MyBatis
---

## 动态 SQL
定义: 根据不同的条件需要执行不同的 SQL 命令

ps:  MyBatis 中动态 SQL 的实现是通过在 mapper.xml 中添加逻辑判断
<!-- more -->
## `<if>`
```xml
<select id="selByAccinAccout" resultType="log">
  select * from log where 1=1
  <!-- OGNL 表达式,直接写 key 或对象的属性.不需要添加任何特字符号 -->
  <if test="accin!=null and accin!=''">
    and accin=#{accin}
  </if>
  <if test="accout!=null and accout!=''">
    and accout=#{accout}
  </if>
</select>
```

## `<where>`
1. 当编写 where 标签时,如果内容中第一个是 and 去掉第一个 and
2. 如果<where>中有内容会生成 where 关键字,如果没有内容不生成 where 关键
3. 代码示例
```xml
<select id="selByAccinAccout" resultType="log">
  select * from log
  <where>
    <if test="accin!=null and accin!=''">
      and accin=#{accin}
    </if>
    <if test="accout!=null and accout!=''">
      and accout=#{accout}
    </if>
  </where>
</select>
```
ps: 比直接使用`<if>`少写 where 1=1

## `<choose> <when> <otherwise>`
1. 只有有一个成立,其他都不执行
2. 代码示例
```xml
<select id="selByAccinAccout" resultType="log">
  select * from log
  <where>
    <choose>
      <when test="accin!=null and accin!=''">
        and accin=#{accin}
      </when>
      <when test="accout!=null and accout!=''">
        and accout=#{accout}
      </when>
    </choose>
  </where>
</select>
```
ps:如果 accin 和 accout 都不是 null 或不是 "" 生成的 sql 中只有 where accin=?，即只有第一个选择分支生效

## `<set>`
1. 用在修改 SQL 中 set 从句中，去掉最后一个逗号
2. 如果`<set>`里面有内容生成 set 关键字,没有就不生成
3. 代码示例
```xml
<update id="upd" parameterType="log" >
  update log
  <set>
    id=#{id},
    <if test="accIn!=null and accIn!=''">
      accin=#{accIn},
    </if>
    <if test="accOut!=null and accOut!=''">
      accout=#{accOut},
    </if>
  </set>
  where id=#{id}
</update>
```
ps: id=#{id} 目的防止`<set>`中没有内容,mybatis 不生成 set 关键字,如果修改中没有 set 从句 SQL 语法错误

## `<trim>`
1. prefix 在前面添加内容
2. prefixOverrides 去掉前面内容
3. suffix 在后面添加内容
4. suffixOverrieds 去掉后面内容
5. 执行顺序去掉内容后添加内容
6. 代码示例
```xml
<update id="upd" parameterType="log">
  update log
  <trim prefix="set" suffixOverrides=",">
    a=a,
  </trim>
  where id=100
</update>
```

## `<bind>`
1. 给参数重新赋值
2. 场景:
  2.1 模糊查询
  2.2 在原内容前或后添加内容
3. 代码示例
```xml
<select id="selByLog" parameterType="log" resultType="log">
  <bind name="accin" value="'%'+accin+'%'"/>
  #{money}
</select>
```

## `<foreach>`
1. 循环参数内容,还具备在内容的前后添加内容,还具备添加分隔符功能
2. 适用场景:in 查询中，批量新增中(mybatis 中 foreach 效率比较低)
  2.1 如果希望批量新增,SQL 命令为
  `insert into log VALUES(default,1,2,3),(default,2,3,4),(default,3,4,5)`
  2.2 openSession()必须指定参数才能使用
      - 底层 JDBC 的 `PreparedStatement.addBatch();`
      - `factory.openSession(ExecutorType.BATCH);`
3. 代码示例
  3.1 `collectino=""` 要遍历的集合
  3.2 `item` 迭代变量, #{迭代变量名}获取内容
  3.3 `open` 循环后左侧添加的内容
  3.4 `close` 循环后右侧添加的内容
  3.5 `separator` 每次循环时,元素之间的分隔符
```xml
<select id="selIn" parameterType="list" resultType="log">
  select * from log where id in
  <foreach collection="list" item="abc" open="(" close=")" separator=",">
    #{abc}
  </foreach>
</select>
```

## `<sql>` 和 `<include>`
某些 SQL 片段如果希望复用,可以使用<sql>定义这个片段
```xml
<sql id="mysql">
  id,accin,accout,money
</sql>
```

在`<select>或<delete>或<update>或<insert>`中使用`<include>`引用
```xml
<select id="">
  select <include refid="mysql"></include> from log
</select>
```
