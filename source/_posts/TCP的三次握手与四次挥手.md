---
title: TCP的三次握手与四次挥手
toc: true
date: 2020-5-30 09:51:34
categories:
tags:
	- 计算机网路
---

## 传输控制协议TCP

- 面向连接的、可靠的、基于字节流的传输层通信协议
- 将应用层的数据流分割成报文段并发送给目标节点的TCP层
- 数据包都有序号,对方收到则发送ACK确认,未收到则重传
- 使用校验和来检验数据在传输过程中是否有误
<!-- more -->

### TCP Flags

- URG :紧急指针标志
- **ACK** :确认序号标志
- PSH : push标志
- RST :重置连接标志
- **SYN** :同步序号,用于建立连接过程
- **FIN** : finish标志,用于释放连接

## TCP的三次握手

![](http://cdn.liaojincan.top/20200530111048.png)

1. 第一次握手
建立连接时，客户端发送请求连接报文段到服务器，其中同步位SYN=1，同时选择了一个初始序号seq=x，客户端进入SYN_ SENT状态(同步已发送)，等待服务器确认

2. 第二次握手
服务器收到SYN包，如同意连接必须发送确认，SYN与ACK均为1 ，确认号ACK=x+1 ,同时为自己选择一个初始序号seq=y，服务器进入SYN_RECV状态(同步收到)

3. 第三次握手
客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=y+1),同时seq=x+1，此包发送完毕后，客户端和服务器进入ESTABLISHED(已建立连接)状态，完成三次握手

### 为什么需要三次握手才能建立连接?

- 主要是为了初始化双方Sequence Number的值
- 防止已经失效的连接报文段又传送到B
A发出的连接请求报文丢失（或超时）而未收到确认，A重传了连接请求报文并建立了连接，然后此时A第一次连接请求到达了B，如果没有第三次握手，那么B就会产生错误

### 首次握手的隐患，SYN超时

1. 起因
    - Server收到Client的SYN ,回复SYN- ACK的时候未收到ACK确认
    - Server不断重试直至超时, Linux默认等待63秒才断开连接
2. 针对SYN Flood的防护措施
    - SYN队列满后，通过tcp_ syncookies参数回发SYN Cookie
    - 若为正常连接则Client会回发SYN Cookie，直接建立连接

### 建立连接后, Client出现故障怎么办？

TCP含有保活机制，其作用原理如下

- 向对方发送保活探测报文,如果未收到响应则继续发送
- 尝试次数达到保活探测数仍未收到响应则中断连接

## TCP的四次挥手

![](http://cdn.liaojincan.top/20200530123703.png)

1. 第一次挥手
Client发送-一个FIN,用来关闭Client 到Server的数据传送，Client 进入FIN _WAIT_ 1状态

2. 第二次挥手
Server 收到FIN后，发送-一个ACK给Client,确认序号为收到序号+1 (与SYN相同，一个FIN占用-一个序号)，Server 进入CLOSE _WAIT状态

3. 第三次挥手
Server发送-一个FIN,用来关闭Server到Client的数据传送，Server 进入LAST _ACK状态

4. 第四次挥手
Client 收到FIN后，Client 进入TIME _WAIT状态,接着发送一个ACK给Server,确认序号为收到序号+1,Server进入CLOSED状态，完成四次挥手

### 为什么会有TIME-WAIT状态？

1. 确保有足够的时间让服务器收到ACK包
2. 避免新旧连接混淆

### 为什么需要四次握手才能断开连接

因为TCP连接是全双工通信，发送方和接收方都需要FIN报文和ACK报文

### 服务器出现大量CLOSE_ WAIT状态的原因

对方关闭socket连接,我方忙于读或写,没有及时关闭连接

- 检查代码,特别是释放资源的代码
- 检查配置,特别是处理请求的线程配置
