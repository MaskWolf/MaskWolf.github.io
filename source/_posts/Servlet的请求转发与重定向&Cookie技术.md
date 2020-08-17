---
title: Servlet的请求转发与重定向&Cookie技术
thumbnail:
toc: true
date: 2019-11-20 09:40:39
tags:
  - Servlet&Jsp
---

## 请求转发
作用：实现多个servlet联动操作处理请求，这样能避免代码冗余，让servlet的职责更加明确
<!-- more -->
使用：`req.getRequestDispatcher("要转发的地址").forward(req, resp);`
地址：相对路径，直接书写servlet的别名即可

特点：
1. 一次请求，浏览器地址栏信息不改变
2. 请求转发后直接`return`结束即可

## 重定向
作用：解决了表单重复提交的问题，以及当前servlet无法处理的请求的问题

使用：`resp.sendRedirect(String uri);`

时机：
1. 如果请求中有表单数据，而数据又比较重要，不能重复提交，建议使用重定向
2. 如果请求被Servlet接收后，无法进行处理，建议使用重定向定位到可以处理的资源

特点：
1. 两次请求，两个request对象
2. 浏览器地址栏信息改变

## Cookie技术
作用：解决了发送的不同请求的数据共享问题

Cookie的创建和存储：
1. 创建Cookie对象
`Cookie c=new Cookie(String name, String value);`
2. 设置cookie(可选)
设置有效期 `c.setMaxAge(int seconds);`
设置有效路径 `c.setPath(String uri);`
3. 响应Cookie信息给客户端
`resp.addCookie(c);`

Cookie的获取
1. 获取Cookie信息数组
`Cookie[] cks=req.getCookies();`
2. 遍历数组获取Cookie信息，使用for循环遍历即可，示例：
    ```java
    if(cks!=null){
       for(Cookie c:cks){
         String name=c.getName();
         String value=c.getValue();
         System.out.println(name+":"+value);
       }
    }
    ```

特点:
1. 一个Cookie对象存储一条数据。多条数据，可以多创建几个Cookie对象进行存储。
2. 浏览器端的数据存储技术。
3. 存储的数据声明在服务器端。
4. 临时存储:存储在浏览器的运行内存中，浏览器关闭即失效。
5. 定时存储:设置了Cookie的有效期，存储在客户端的硬盘中，在有效期内符合路径要求的请求都会附带该信息。
6. 默认cookie信息存储好之后，每次请求都会附带，除非设置有效路径
