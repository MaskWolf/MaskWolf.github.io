---
title: 超时重传时间的选择与TCP的滑动窗口
toc: true
date: 2020-5-31 10:00:36
categories:
tags:
	- 计算机网路
---

## 超时重传时间的选择
RTT : 报文段的往返时间
RTT<sub>S</sub> : 加权平均往返时间，也称平滑的往返时间

- 第一次测量到RTT样本时，RTT<sub>S</sub> 值就取该样本
- 此后按下列式子进行计算
**新的RTT<sub>S</sub> = (1 - α) * (旧的RTT<sub>S</sub>) + α * (新的RTT样本)**
- 建议标准 RFC 6289 推荐的α值为 1/8
<!-- more -->
RTO : 超时重传时间
RTT<sub>D</sub> : RTT的偏差的加权平均值
- 第一次测量时，RTT<sub>D</sub> 取RTT样本值的一半
- 此后按下列式子进行计算
**新的RTT<sub>D</sub> = (1 - β) * (旧的RTT<sub>D</sub>) + β * |RTT<sub>S</sub> - 新的RTT样本|**
- 建议标准 RFC 6289 推荐的β值为 1/4
- **RTO = RTT<sub>S</sub> + 4 * RTT<sub>D</sub>**

## TCP的滑动窗口
基于确认重传机制，主要功能是流量控制和乱序重排

![](http://cdn.liaojincan.top/20200531105352.png)

AdvertisedWindow = MaxRcvBuffer - (LastByteRcvd - LastByteRead)

EffectiveWindow = AdvertisedWindow - (LastByteSent - LastByteAcked)