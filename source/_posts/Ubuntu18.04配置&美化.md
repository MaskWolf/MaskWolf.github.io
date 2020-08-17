---
title: Ubuntu18.04配置&美化
thumbnail:
toc: true
date: 2019-11-14 16:48:59
tags:
	- Linux
	- 搞事情
---

## Ubuntu18.04配置常用软件

### 将语言改为中文(简体)

1. 首先进入设置
<!-- more -->
![](http://cdn.liaojincan.top/2018052907260532.png)
2. 找到Region&Language，点击Manage Installed Languages
![](http://cdn.liaojincan.top/20180529072708524.png)
3. 点击Install
![](http://cdn.liaojincan.top/20180529072853331.png)
4. 输入用户名密码
![](http://cdn.liaojincan.top/20180529072953504.png)
5. 等待Applying changes
![](http://cdn.liaojincan.top/20180529073437895.png)
6. Install/Remove Languages...
![](http://cdn.liaojincan.top/20180529073137904.png)
7. 找到中文(简体)Chinese(simplified)，点击Apply
![](http://cdn.liaojincan.top/20180529073322406.png)
8. 等待Applying changes(需要一定的时间)
![](http://cdn.liaojincan.top/20180529073051524.png)
9. 将下载好的中文(简体)按住鼠标拖拽到最上面
![](http://cdn.liaojincan.top/20180529074450174.png)
10. 重新启动Ubuntu 18.04
![](http://cdn.liaojincan.top/2018052907461161.png)
11. 登陆，进入到桌面，设置为“保留旧的名称”(勾选以后不再询问我)
![](http://cdn.liaojincan.top/20180529075102559.png)

### Ubuntu18.04使用图形界面换源

![](http://cdn.liaojincan.top/source1.png)
![](http://cdn.liaojincan.top/source2.png)
![](http://cdn.liaojincan.top/source3.png)


### 安装Chrome浏览器

1. 下载好安装文件google-chrome-stable_current_amd64.deb
2. 在文件所在位置打开终端，输入以下命令

```script
sudo dpkg -i google-chrome-stable_current_adm64.deb
```

![](http://cdn.liaojincan.top/Chrome.png)

### 安装搜狗拼音输入法

1. 首先安装fcitx

![](http://cdn.liaojincan.top/fcitx.png)

2. 切换输入法系统为fcitx后重启电脑

![](http://cdn.liaojincan.top/swapfcitx.png)

3. 安装搜狗输入法

- 在搜狗输入法官网下载输入法安装文件sogoupinyin_2.2.0.0108_amd64.deb
![](http://cdn.liaojincan.top/sougou1.png)
- 在文件所在位置打开终端，输入以下命令

```script
sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb
```

4. 配置搜狗输入法为首选输入法

- 点击系统界面有右上角的键盘图标，然后点击配置，将输入法移动到首位即可
![](http://cdn.liaojincan.top/sougou2.png)

### 安装WPS

1. 在WPS官网下载安装文件wps-office_11.1.0.8372_amd64.deb
![](http://cdn.liaojincan.top/wps1.png)

2. 在文件所在位置打开终端，输入以下命令
```script
sudo dpkg -i wps-office_11.1.0.8372_amd64.deb
```
3. 解决字体缺失问题
字体资源下载地址  [https://pan.baidu.com/s/1codGNTwTAYb8JnGy-Kvxig](https://pan.baidu.com/s/1codGNTwTAYb8JnGy-Kvxig)
解压字体资源并进入文件夹处打开终端输入下列命令
```
sudo cp mtextra.ttf  symbol.ttf  WEBDINGS.TTF  wingding.ttf  WINGDNG2.ttf  WINGDNG3.ttf  /usr/share/fonts
```
![](http://cdn.liaojincan.top/wps2.png)

### 安装wine-QQ
**2019-10-30 16:47:34 --- QQ官方出Linux版了，可以直接用官方的不搞这个了**
1. 安装deepin-wine环境
上[https://github.com/wszqkzqk/deepin-wine-ubuntu](https://github.com/wszqkzqk/deepin-wine-ubuntu)页面下载zip包（或用git方式克隆），解压到本地文件夹，在文件夹中打开终端，输入sudo sh ./install.sh一键安装。
2. 安装相关应用容器
在[http://mirrors.aliyun.com/deepin/pool/non-free/d/](http://mirrors.aliyun.com/deepin/pool/non-free/d/)中下载想要的容器，点击deb安装即可。以下为推荐容器，任选其一即可:
TIM：http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.office/
QQ：http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im/
QQ轻聊版：http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.im.light/

3. 解决QQ最小化托盘图标问题
在软件商店中搜索并安装 Gnome Shell 插件：TopIcons Plus
![](http://cdn.liaojincan.top/qq1.png)
![](http://cdn.liaojincan.top/qq0.png)

## Ubuntu18.04界面美化
### 安装Gnome-tweak-tool统一管理各种美化操作
```
sudo apt-get install gnome-tweak-tool
```

### 在Ubuntu软件商店里面搜索安装各种插件
- **Dash to dock**
	设置让图标从界面左边变成界面底部居中或者其他位置
	可以通过设置使图标栏背景透明
	智能隐藏显示图标
- **User theme**
	使shell主题可以使用桌面主题(shell即为顶部栏，shell主题和桌面主题不一样，是个单独的模块)
- **Hide top bar**
	可以设置自动隐藏顶部栏，因为18.04应用上框不会和顶部栏合并，比较占面积
- **TopIcons Plus**
	在任务通知栏位置为部分应用形成一个个的图标，例如Wine-QQ
- **Dynamic Top Bars**
	使顶部栏透明，界面整体效果更加舒爽
