---
title: Socket
toc: true
date: 2020-6-1 16:44:27
categories:
tags:
	- 计算机网路
---

Scoket是对TCP/IP协议的抽象，是操作系统对外开放的接口

![](http://cdn.liaojincan.top/20200601164549.png)
<!-- more -->

## Scoket通信流程
![](http://cdn.liaojincan.top/20200601164829.png)

## 面试真题
编写一个网络应用程序，有客户端与服务器端，客户端向服务器发送一个字符串，服务器收到该字符串后将其打印到命令行上，然后向客户端返回该字符串的长度，最后，客户端输出服务器端返回的该字符串的长度，分别用TCP和UDP种方式去实现

### TCP
TCPServer.java
```java
public class TCPServer {
    public static void main(String[] args) throws Exception{
        //创建socket,并将socket绑定到65001端口
        ServerSocket serverSocket = new ServerSocket(65001);
        //死循环，使得socket一直等待并处理客户端发送过来的请求
        while (true) {
            //监听65000端口，直到客户端返回连接信息后才返回
            Socket socket = serverSocket.accept();
            //获取客户端的请求信息后，执行相关业务逻辑
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        OutputStream os = socket.getOutputStream();
                        InputStream is = socket.getInputStream();
                        int ch = 0;
                        byte[] buffer = new byte[1024];
                        ch = is.read(buffer);
                        String content = new String(buffer, 0, ch);
                        System.out.println(content);
                        os.write(new String(content.length() + "").getBytes());
                        is.close();
                        os.close();
                        socket.close();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }).start();
        }
    }
}
```

TCPClient.java
```java
public class TCPClient {
    public static void main(String[] args) throws Exception{
        Socket socket = new Socket("127.0.0.1", 65001);
        OutputStream os = socket.getOutputStream();
        InputStream is = socket.getInputStream();

        os.write("Hello World!".getBytes());

        byte[] buffer = new byte[1024];
        int ch = is.read(buffer);
        String content = new String(buffer, 0, ch);
        System.out.println(content);
        
        is.close();
        os.close();
        socket.close();
    }
}
```

### UDP
UDPServer.java
```java
public class UDPServer {
    public static void main(String[] args) throws Exception{
        DatagramSocket socket = new DatagramSocket(65001);
        while (true) {
           new Thread(new Runnable() {
               @Override
               public void run() {
                   try {
                       // 服务端接受客户端发送的数据报
                       byte[] buffer = new byte[1024];
                       DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
                       socket.receive(packet);

                       byte[] data = packet.getData();
                       String content = new String(data, 0, packet.getLength());
                       System.out.println(content);

                       byte[] sentContent = String.valueOf(content.length()).getBytes();

                       // 服务端给客户端发送数据报
                       DatagramPacket packetToClient = new DatagramPacket(sentContent, sentContent.length,
                               packet.getAddress(), packet.getPort());
                       socket.send(packetToClient);
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
           }).start();
       }
    }
}

```

UDPClient.java
```java
public class UDPClient {
    public static void main(String[] args) throws Exception{
        DatagramSocket socket = new DatagramSocket();
        byte[] buf = "Hello World!".getBytes();
        InetAddress address = InetAddress.getByName("127.0.0.1");
        DatagramPacket packet = new DatagramPacket(buf, buf.length, address,
                65001);
        socket.send(packet);

        // 客户端接受服务端发送过来的数据报
        byte[] data = new byte[100];
        DatagramPacket receivedPacket = new DatagramPacket(data, data.length);
        socket.receive(receivedPacket);
        String content = new String(receivedPacket.getData(), 0,
                receivedPacket.getLength());
        System.out.println(content);
    }
}
```