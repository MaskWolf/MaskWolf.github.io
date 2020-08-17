---
title: newifi mini刷Breed与PandoraBox
date: 2019-05-11 00:12:28
tags: [搞事情]
---

## 刷机所用路由器 —— newifi mini
![](http://cdn.liaojincan.top/92c74094-c1ef-11e6-8cbf-c50c298eb06a.png)
<!-- more -->
## 刷机所用固件(注意下载路由器对应的版本)
- Breed下载地址
[https://www.right.com.cn/forum/thread-161906-1-1.html](https://www.right.com.cn/forum/thread-161906-1-1.html)
[https://breed.hackpascal.net/](https://breed.hackpascal.net/)
- PandoraBox下载地址
[http://downloads.openwrt.org.cn/PandoraBox/](http://downloads.openwrt.org.cn/PandoraBox/)

## 刷Breed
**刷了Breed固件，再从Breed固件刷其它第三方固件不容易变砖**
1. 准备一根网线，一端连电脑，一端连Newifi任意一个网口(LAN)
2. 设置电脑IP地址为192.168.1.88，子网掩码为255.255.255.0
3. 按住Newifi的Reset键不放，接通Newifi电源，等6秒后(指示灯闪烁后)再松开Reset键
4. 电脑浏览器访问192.168.1.1，进入Newifi恢复模式界面
![](http://cdn.liaojincan.top/2019-05-07_133558.png)
5. 选择对应Newifi相应的[Breed固件](https://breed.hackpascal.net/ "Breed固件")，点击恢复按钮，等待升级完毕。
注意：升级过程中一定别断开电源，不然大概率变砖(亲测)
![](http://cdn.liaojincan.top/2019-05-07_133654.png)

## 刷第三方固件PandoraBox(OpenWrt的分支)
1. 进入Breed Web恢复控制台界面
a. 拔掉电源
b. 按住Reset键不放
c. 接通电源
d. 6秒后松开Reset键
e. 访问192.168.1.1进入Breed界面
![](http://cdn.liaojincan.top/2019-05-07_134351.png)
Tips:无法进入Breed界面时可尝试先将电脑IP恢复为自动获取
2. 备份原始固件(EEPRON和编程器固件)
![](http://cdn.liaojincan.top/2019-05-07_140752.png)
3. 到固件更新页面选择固件，选择第三方固件文件，点击上传
![](http://cdn.liaojincan.top/2019-05-07_140813.png)
4. 点击上传后会有一个确认页面，核对无误后继续，等待更新完成
![](http://cdn.liaojincan.top/2019-05-07_140852.png)
5. 将电脑IP恢复为自动获取，再次进入192.168.1.1，输入密码admin跳转PandoraBox管理界面
![](http://cdn.liaojincan.top/markdown-img-paste-20190514162422346.png)
![](http://cdn.liaojincan.top/markdown-img-paste-20190514162452493.png)

**到此，路由器刷Breed与PandoraBox成功完成！**
