---
title: TomcatJDBCPool数据库连接池
toc: true
date: 2019-12-5 17:32:05
tags:
	- JavaEE
---
## 数据库连接池
1. 所做工作：在内存中开辟了一块空间,存放多个数据库连接对象
2. 数据库连接池中数据库连接对象的状态
  - active态:当前连接对象被应用程序使用中
  - Idle空闲状态:等待应用程序使用
<!-- more -->
3. 使用数据库连接池的目的
在高频率访问数据库时,使用数据库连接池可以降低服务器系 统压力,提升程序运行效率

注意：小型项目不适用数据库连接池

## JDBC Tomcat Pool
这是一个直接由 tomcat 产生数据库连接池
1. 在WEB项目中创建META-INF目录
  META-INF目录与WEB-INF目录同级
![](http://cdn.liaojincan.top/20191205185640.png)
2. 在META-INF目录下创建context.xml
![](http://cdn.liaojincan.top/20191205185903.png)
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <Context>
      <Resource
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC"
              username="root"
              password="199827"
              maxActive="50"
              maxIdle="20"
              name="test"
              auth="Container"
              maxWait="10000"
              type="javax.sql.DataSource"
      />
      <!--
            driverClassName JDBC驱动名
            url 数据库地址
            username 数据库用户名
            password 数据库用户密码
            maxActive 最大活跃数据库对象
            maxIdle 最大等待数据库对象
            name 当前数据库连接的识别名
            auth 认证
            maxWait 新请求等待时间
            type 返回的数据类型
      -->
  </Context>
  ```
3. 获取数据库连接池
  ```java
  @WebServlet(urlPatterns = "/pool", name = "pool")
  public class DemoServlet extends HttpServlet {
      @Override
      protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //设置请求编码格式
          req.setCharacterEncoding("utf-8");
          //设置响应编码格式
          resp.setContentType("text/html;charset=utf-8");
          try {
              //获取上下文
              Context context = new InitialContext();
              //获取数据库连接池
              DataSource dataSource = (DataSource) context.lookup("java:comp/env/test");

              //从数据库连接池获取数据库连接
              Connection connection = dataSource.getConnection();
              PreparedStatement preparedStatement = connection.prepareStatement("select * from flower");
              ResultSet resultSet = preparedStatement.executeQuery();

              PrintWriter printWriter = resp.getWriter();
              while (resultSet.next()){
                  printWriter.print(resultSet.getInt(1)+"&nbsp;&nbsp;&nbsp;&nbsp;"+resultSet.getString(2)+"<br/>");
              }
              printWriter.flush();
              printWriter.close();

              resultSet.close();
          } catch (NamingException e) {
              e.printStackTrace();
          } catch (SQLException e) {
              e.printStackTrace();
          }
      }
  }

  ```

注意：
1. 使用Tomcat JDBC Pool仍然需要在项目下导入数据库的JDBC驱动包
2. 在普通java类无法调用Tomcat JDBC Pool，必须在要发布到Tomcat容器中的java类中使用
