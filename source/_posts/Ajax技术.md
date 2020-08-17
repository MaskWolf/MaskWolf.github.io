---
title: Ajax技术
thumbnail:
toc: true
date: 2019-11-21 08:36:20
tags:
	- JavaScript
---

## Ajax相关概念及应用场景
定义：AJAX全称为“Asynchronous JavaScript and XML”（异步JavaScript和XML），是一种创建交互式网页应用的网页开发技术，本质上是一个浏览器端的技术
<!-- more -->
所涉及技术：
1. 基于web标准（standards-based presentation）XHTML+CSS的表示
2. 使用 DOM（Document Object Model）进行动态显示及交互
3. 使用 XML 和 XSLT 进行数据交换及相关操作
4. 使用 XMLHttpRequest 进行异步数据查询、检索
5. 使用 JavaScript 将所有的东西绑定在一起

应用场景：在保留原有页面内容的情况下，显示新的响应内容

## Ajax的使用
### 创建ajax引擎对象
```js
var ajax;
if(window.XMLHttpRequest){//火狐
  ajax=new XMLHttpRequest();
}else if(window.ActiveXObject){//IE
  ajax=new ActiveXObject("Msxml2.XMLHTTP");
}
```
### 复写onreadystatement函数

|readyState值|含义|
|:---:|:---:|
|0|表示XMLHttpRequest已建立，但还未初始化，这时尚未调用open方法|
|1|表示open方法已经调用，但未调用send方法（已创建，未发送）|
|2|表示send方法已经调用，其他数据未知|
|3|表示请求已经成功发送，正在接受数据|
|4|表示数据已经成功接收|

|http状态码|含义|
|:---:|:---:|
|200|资源获取成功|
|404|请求资源未找到|
|500|服务器内部错误|

示例：
```js
ajax.onreadystatechange=function(){
  //Ajax状态码为4时表示成功接收
  if(ajax.readyState == 4){
	//响应状态码为200上时表示获取资源成功
	if(ajax.status == 200){
	  //获取数据并处理数据
	}
  }
}
```
### 发送请求
get请求：请求实体用?与URL隔开，以键值对的形式拼接在URL后面
```js
ajax.open("get","url");
ajax.send(null);
```
post请求：有单独的请求实体
```js
ajax.open("post", "url");
ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
ajax.send("name=张三&pwd=123");
```
### ajax的异步和同步
使用：`ajax.open(method,urL,async)`

async：设置同步代码执行还是异步代码执行，true代表异步，false代表同步,默认是异步

异步：并行处理，程序向服务器发送一个请求后，在结果返回之前，程序还是可以执行其它操作（以前台界面为例，用户依然可以输入其它信息，并且和服务器进行其它交付）

同步：顺序处理，程序向服务器发送一个请求，在结果返回之前，程序要一直等待结果返回才可以执行下一步操作
