---
title: 安装Windows10&Ubuntu18.04双系统
thumbnail:
toc: true
date: 2019-11-14 08:45:24
tags:
	- Linux
	- 搞事情
---
## 最终效果
![](http://cdn.liaojincan.top/2019年11月14日093015.png)
<!-- more -->

## 准备工作
### 从磁盘分出分出100G空闲空间
右键点击我的电脑，然后点击管理
![](http://cdn.liaojincan.top/2019年11月14日091021.png)
点击磁盘管理
![](http://cdn.liaojincan.top/2019年11月14日091632.png)
选择一个大容量的分区（不要选择Windows10的系统分区），右键，选择压缩卷，从磁盘分出分出100G空闲空间
![](http://cdn.liaojincan.top/2019年11月14日091934.png)
![](http://cdn.liaojincan.top/2019年11月14日092507.png)
### 准备空白U盘和Ubuntu18.04镜像文件
百度Ubuntu18.04镜像，下载即可，下载完成后，将文件解压缩到空白U盘
![](http://cdn.liaojincan.top/2019年11月14日092710.png)

## 安装过程
1. 插上制作好的U盘，重启电脑
2. 进入BIOS（一般都是刚开机就一直按F2），关闭Security Boot选项，再次重启电脑
3. 进入引导选择界面（一般都是刚开机就一直按F12），选择自己制作的那个U盘
4. 此时就进入了Ubuntu18.04的系统安装引导界面，后续步骤参照虚拟机安装Ubuntu系统，仅需要注意选择安装位置时一定要选择我们之前准备好的那100G空闲空间
