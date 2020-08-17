---
title: 在项目中引入Log4j日志支持
toc: true
date: 2020-1-19 16:53:45
tags:
	- JavaEE
---
## 下载jar包
下载地址：[http://mirrors.tuna.tsinghua.edu.cn/apache/logging/log4j/2.13.0/apache-log4j-2.13.0-bin.zip](http://mirrors.tuna.tsinghua.edu.cn/apache/logging/log4j/2.13.0/apache-log4j-2.13.0-bin.zip)
<!-- more -->
将下载文件中的`log4j-api-2.13.0.jar`和`log4j-core-2.13.0.jar`导入项目

## 在src下创建log4j.properties
### 文件内容
```properties
log4j.rootCategory=Info, CONSOLE ,LOGFILE

log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%C %d{YYYY-MM-dd hh:mm:ss}  %m %n

log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=E:/my.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%m %n
```
### 解析
1. 第一行控制输出级别和输出目的地
	- 输出级别：fatal(致命错误) > error (错误) > warn (警告) > info(普通信息) > debug(调试信息)
	- 输出目的地：控制台（CONSOLE）、日志文件（LOGFILE）
2. ConversionPattern与表达式共同控制输出内容
	常用表达式
	- %C	包名+类名
	- %d{YYYY-MM-dd HH:mm:ss}	时间
	- %L	行号
	- %m	信息
	- %n	换行

3. log4j.appender.LOGFILE.File控制输出的日志文件的路径和名称（文件扩展名为.log）
