---
title: B站---【狂神说Java】网络编程实战讲解
tags:
top_img: false
categories:
  - 网络编程
date: 2021-12-09 00:55:10
abbrlink: NetWorkProgramming
---

# 网络编程

## 概述

> 地球村：我在中国，有一个美国的朋友

### 信息传递

![信息传递](https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/networkprogramming-msg.jpg)

### 计算机网络

> 计算机网络是指将[地理](https://baike.baidu.com/item/地理)位置不同的具有独立功能的多台[计算机](https://baike.baidu.com/item/计算机/140338)及其外部设备，通过通信线路连接起来，在[网络操作系统](https://baike.baidu.com/item/网络操作系统/3997)，[网络管理软件](https://baike.baidu.com/item/网络管理软件/6579078)及[网络通信协议](https://baike.baidu.com/item/网络通信协议/4438611)的管理和协调下，实现[资源共享](https://baike.baidu.com/item/资源共享/233480)和信息传递的计算机系统。

### 网络编程的目的

无线电台...传播交流信息，数据交换，通信

### 想要达到这个效果需要什么：

1. 如何准确定位网络上的一台主机。IP+端口
2. 找到了这个主机，如何传输数据呢？

> 注意！！！这里的网络编程不是JavaWeb(`网页编程`)

## 网络编程的要素

如何实现网络的通信？

### 通信双方的地址

- IP
- 端口号

### 规则：网络通信的协议

**开放式系统互联通信参考模型**（英语：Open System Interconnection Reference Model，缩写为 OSI），简称为**OSI模型**（OSI model），一种[概念模型](https://baike.baidu.com/item/概念模型/3187025)，由[国际标准化组织](https://baike.baidu.com/item/国际标准化组织/779832)提出，一个试图使各种计算机在世界范围内互连为网络的标准框架。

![osi](https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/networkprogramming-osi.jpg)

`小结：`

1. 网络编程中有两个重要的问题
   - 如何准确的定位到网络上的一台主机。
   - 找到主机之后如何进行通信
2. 网络编程中的要素
   - IP和端口号
   - 网络通信协议
3. 万物皆对象

## IP地址

ip地址：InetAddress

- 唯一定位一台网络上的计算机

- 127.0.0.1：本机localhost

- IP地址的分类

  - ipv4/ipv6
    - IPV4 127.0.0.1	4个字节组成     0-255  42亿	30亿在北美	亚洲4亿	2011年就用尽了
    - IPV6  128位。8个无符号整数！

  > fa66.2asd.98a5.q63c.6a6:4665s%1

- 域名：

​		为了IP好记

`测试demo`

```java
package com.pink.demo1;

import java.net.InetAddress;
import java.net.UnknownHostException;

//测试IP
public class TestInetAddress {
    public static void main(String[] args) {
        try {
            //查询本机地址
            InetAddress inetAddress = InetAddress.getByName("127.0.0.1");
            InetAddress inetAddress3 = InetAddress.getByName("localhost");
            System.out.println(inetAddress3);
            InetAddress inetAddress4 = InetAddress.getLocalHost();
            System.out.println(inetAddress4);
            //查询网站IP地址
            InetAddress inetAddress2 = InetAddress.getByName("www.baidu.com");
            System.out.println(inetAddress2);
            //常用方法
            System.out.println(inetAddress2.getCanonicalHostName());//标准名
            System.out.println(inetAddress2.getAddress());
            System.out.println(inetAddress2.getHostAddress());//ip
            System.out.println(inetAddress2.getHostName());//域名
        } catch (UnknownHostException e) {
            e.printStackTrace();
        }
    }
}

```

## 端口Port

端口表示计算机上的一个程序的进程

- 不同的进程有不同的端口号！用来区分软件！
- 被规定0~65535
- TCP，UDP:
  - 这两种协议没一个都有0~65535个端口
  - 单协议下，端口号不能冲突
- 端口分类
  - 公有端口：0~1023
    - HTTP：80
    - HTTPS：443
    - FTP：21
    - Telent：23
  - 程序注册端口：1024~49151
    - Tomcat：8080
    - Mysql：3306
    - Oracle：1521
  - 动态、私有：49152~65535

```bash
netstat -ano #查看所有端口
netstat -ano|findstr "端口号"	#查看指定端口
tasklist|findstr "端口号"	#查看指定端口的进程
```

![](https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/networkprogramming-port.jpg)

当一台电脑发送请求到另一台电脑    另一台电脑需要有处理这种请求的工具MSN

马化腾QICQ 微软

## 通信协议

协议：约定，就好比我们现在说的是普通话

`网络通信协议：`速率，传输码率，代码结构，传输控制.....

`问题：`非常的复杂？

大事化小：`分层`

`TCP/IP协议簇：实际上是一组协议`

- TCP：用户传输协议
- UDP：用户数据报协议
- IP：网络互连协议

### TCP UDP对比

`TCP：打电话`

- 连接，稳定
- `三次握手` `四次挥手`

> 最少需要三次，保证稳定连接！
>
> A：你愁啥？
>
> B：瞅你咋地？
>
> A：干一场！
>
> 
>
> 
>
> A：我要走了！
>
> B：你真的要走了吗？
>
> B：你真的真的要走了吗？
>
> A：我真的要走了！



- 客户端、服务端
- 传输完成、释放连接、效率低

`UDP：发短信`

- 不连接、不稳定
- 客户端、服务端：没有明显界限
- 不管有没有准备好、都可以发给你
- 导弹
- DDOS：洪水攻击！饱和攻击

## TCP实现聊天

`客户端`

- 连接客户端Socket
- 发送消息

```java
package com.pink.demo2;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

//客户端
public class TcpClientDemo01 {
    public static void main(String[] args) {
        Socket socket = null;
        OutputStream os = null;
        //1.要知道服务器的地址
        try {
            InetAddress serverIp = InetAddress.getByName("127.0.0.1");
            int port = 9999;
            //2.创建一个Socket连接
            socket = new Socket(serverIp,port);
            //3.发送消息IO流
            os = socket.getOutputStream();
            os.write("HelloWorld".getBytes());
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭资源	遵循先开后关原则
            if (os!=null){
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

`服务端`

- 建立服务的端口ServerSocket
- 等待用户的连接accept
- 接受用户的消息ByteArrayOutputStream

```java
package com.pink.demo2;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

//服务端
public class TcpServerDemo01 {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {
            //1.我得有一个地址
            serverSocket = new ServerSocket(9999);
            //等待客户端连接
            socket = serverSocket.accept();
            //读取客户端的消息
            is = socket.getInputStream();
            /*byte[] buffer = new byte[1024];
            int len;
            while ((len=is.read(buffer))!=-1){
                String msg = new String(buffer, 0, len);
                System.out.println(msg);
             }*/


            //管道流
            baos = new ByteArrayOutputStream();
            byte[] butter = new byte[1024];
            int len;
            while((len= is.read(butter))!=-1){
                baos.write(butter,0,len);
            }
            System.out.println(baos);

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //关闭资源	遵循先开后关原则
            if (baos!=null){
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (serverSocket!=null){
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## TCP文件上传

`客户端`

```java
package com.pink.demo2;

import java.io.*;
import java.net.Socket;

public class TcpClientDemo02 {
    public static void main(String[] args) {
        Socket socket = null;
        OutputStream os = null;
        FileInputStream fis = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {
            //1.创建一个Socket连接
            socket = new Socket("127.0.0.1", 9000);
            //2.创建一个输出流
            os = socket.getOutputStream();
            //3.读取文件
            fis = new FileInputStream(new File("HelloWorld.jpg"));
            //4.写出文件
            byte[] buffer = new byte[1024];
            int len;
            while ((len=fis.read(buffer))!=-1){
                os.write(buffer,0,len);
            }
            //通知服务器，我已经结束了
            socket.shutdownOutput();//我已经传输完了
            //确定服务器接收完毕，才能够断开连接
            is = socket.getInputStream();
            baos = new ByteArrayOutputStream();
            byte[] buffer2 = new byte[2014];
            int len2;
            while ((len2=is.read(buffer2))!=-1){
                baos.write(buffer2,0,len2);
            }
            System.out.println(baos);
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //关闭资源
            if (baos!=null){
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (fis!=null){
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (os!=null){
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

`服务器端`

```java
package com.pink.demo2;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TcpServerDemo02 {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        FileOutputStream fos = null;
        OutputStream os = null;
        try {
            //1.创建服务
            serverSocket = new ServerSocket(9000);
            //2.监听客户端连接
            socket = serverSocket.accept();
            //3.获取输入流
            is = socket.getInputStream();
            //4.文件输出
            fos = new FileOutputStream(new File("receive.jpg"));
            byte[] buffer = new byte[1024];
            int len;
            while ((len=is.read(buffer))!=-1){
                fos.write(buffer,0,len);
            }
            //通知客户端我接收完毕了
            os = socket.getOutputStream();
            os.write("我接收完毕了，你可以断开通信了".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (os!=null){
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            //关闭资源
            if (fos!=null){
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is!=null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (serverSocket!=null){
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## UDP消息发送

发短信：不用连接，需要知道对方的地址

`发送端`

```java
package com.pink.demo03;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

//不需要连接服务器
public class UdpClientDemo01 {
    public static void main(String[] args) {
        DatagramSocket socket = null;
        DatagramPacket packet;
        InetAddress localhost;
        try {
            //1.建立一个Socket
            socket = new DatagramSocket();
            //2.建个包
            String msg = "你好啊，服务器";
            localhost = InetAddress.getByName("localhost");
            int port = 9090;
            //数据，数据的起始，要发送给谁
            packet = new DatagramPacket(msg.getBytes(),0,msg.getBytes().length,localhost,port);
            //3.发送包
            socket.send(packet);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭资源
            if (socket != null) {
                socket.close();
            }
        }
    }
}
```

`接收端`

```java
package com.pink.demo03;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

//还是要等待客户端的连接！
public class UdpServerDemo01 {
    public static void main(String[] args) {
        DatagramSocket socket = null;
        DatagramPacket packet;
        try {
            //开启端口
            socket = new DatagramSocket(9090);
            //接受数据包
            byte[] buffer = new byte[1024];
            packet = new DatagramPacket(buffer, 0, buffer.length);
            socket.receive(packet);//阻塞接收
            System.out.println(packet.getAddress().getHostAddress());
            System.out.println(new String(packet.getData(),0,packet.getLength()));
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭资源
            if (socket != null) {
                socket.close();
            }
        }
    }
}
```

## UDP聊天实现

`发送方`

```java
package com.pink.chat;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;

public class UdpSenderDemo01 {
    public static void main(String[] args) {

        DatagramSocket socket = null;
        try {
            socket = new DatagramSocket(8888);
            //准备数据：控制台读取System.in
            BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
            while (true){
                String data = reader.readLine();
                byte[] datas = data.getBytes();
                DatagramPacket packet = new DatagramPacket(datas,0,datas.length,new InetSocketAddress("localhost",6666 ));
                socket.send(packet);
                if (data.equals("bye")){
                    break;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if (socket != null) {
                socket.close();
            }
        }
    }
}
```

`接收方`

```
package com.pink.chat;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UdpReceiveDemo01 {
    public static void main(String[] args) {
        DatagramSocket socket = null;

        try {
            socket = new DatagramSocket(6666);
            while (true){
                //准备接收包裹
                byte[] container = new byte[1024];
                DatagramPacket packet = new DatagramPacket(container,0,container.length);
                socket.receive(packet);//阻塞式接收包裹
                //断开连接 Bye
                byte[] data = packet.getData();
                String receiveData = new String(data, 0, packet.getLength());
                System.out.println(receiveData);
                if (receiveData.equals("bye")){
                    break;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if (socket != null) {
                socket.close();
            }
        }
    }
}
```

## UDP多线程在线咨询

`发送端`

```java
package com.pink.chat;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetSocketAddress;
import java.net.SocketException;

public class TalkSender implements Runnable{
    DatagramSocket socket = null;
    BufferedReader reader = null;
    private int fromIp;
    private String toIp;
    private int toPort;

    public TalkSender(int fromIp, String toIp, int toPort) {
        this.fromIp = fromIp;
        this.toIp = toIp;
        this.toPort = toPort;
        try {
            socket = new DatagramSocket(fromIp);
            reader = new BufferedReader(new InputStreamReader(System.in));
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }
    @Override
    public void run(){
        while (true){
            try {
                String data = reader.readLine();
                byte[] datas = data.getBytes();
                DatagramPacket packet = new DatagramPacket(datas,0,datas.length,new InetSocketAddress(toIp,toPort ));
                socket.send(packet);
                if (data.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        socket.close();
    }
}
```

`接收端`

```
package com.pink.chat;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

public class TalkReceive implements Runnable{
    DatagramSocket socket = null;
    private int port;
    private String msgFrom;

    public TalkReceive(int port,String msgFrom) {
        this.port = port;
        this.msgFrom = msgFrom;
        try {
            socket = new DatagramSocket(port);
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run(){
        while (true){
            try {
                //准备接收包裹
                byte[] container = new byte[1024];
                DatagramPacket packet = new DatagramPacket(container,0,container.length);
                socket.receive(packet);//阻塞式接收包裹
                //断开连接 Bye
                byte[] data = packet.getData();
                String receiveData = new String(data, 0, packet.getLength());
                System.out.println(msgFrom+":"+receiveData);
                if (receiveData.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        socket.close();
    }
}
```

`建立两个类模拟发送方和接收方并且分别开启两个线程`

```java
package com.pink.chat;

public class TalkStudent {
    public static void main(String[] args) {
        //开启两个线程
        new Thread(new TalkSender(7777,"localhost",9999)).start();
        new Thread(new TalkReceive(8888,"老师")).start();
    }
}
```

```java
package com.pink.chat;

public class TalkTeacher {
    public static void main(String[] args) {
        new Thread(new TalkSender(5555,"localhost",8888)).start();
        new Thread(new TalkReceive(9999,"学生")).start();
    }
}
```

> 两个人既可以是发送方，又可以是接收方

## URL

https://www.baidu.com/

统一资源定位符：定位资源的，定位互联网上的某一个资源

DNS域名解析  www.baidu.com 220.181.38.148

> 协议：//ip地址:端口/项目名/资源

`从网络上下载资源`

```java
package com.pink.demo04;

import javax.net.ssl.HttpsURLConnection;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.URL;

public class URLDownLoad {
    public static void main(String[] args) {
        HttpsURLConnection urlConnection = null;
        InputStream inputStream = null;
        FileOutputStream fos = null;
        try {
            //1.下载地址
            URL url = new URL("https://sm.ms/image/aKOcLiyPl2JQdFD");
            //2.连接到这个资源
            urlConnection = (HttpsURLConnection) url.openConnection();

            inputStream = urlConnection.getInputStream();
            fos = new FileOutputStream("aKOcLiyPl2JQdFD");
            byte[] buffer = new byte[1024];
            int len;
            while ((len=inputStream.read(buffer))!=-1){
                fos.write(buffer,0,len);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            try {
                if (fos != null) {
                    fos.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (inputStream != null) {
                    inputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            if (urlConnection != null) {
                urlConnection.disconnect();
            }
        }
    }
}
```

