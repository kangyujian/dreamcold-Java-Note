# UDP网络编程

## UDP网络通信

- 类DatagramSocket和DatagramPacket实现了基于UDP协议网络程序。
- UDP数据报通过数据报套接字DatagramSocket发送和接收，系统不保证
- UDP数据报不一定能够安全送到目的地，也不能确定什么时候可以抵达。
- DatagramPacket对象封装了UDP数据报，在数据报中包含了发送端的IP
- 地址和端口号以及接收端的IP地址和端口号。
- UDP协议中每个数据报都给出了完整的地址信息，因此无须建立发送方和接收方的连接。如同发快递包裹一样。



## 例子

发送端

```java
package com.dreamcold.network;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.UnknownHostException;

public class UDPSender {
    //发送端
    public static void main(String[] args) throws IOException {
        DatagramSocket socket=new DatagramSocket();
        String str="我是UDP方式发送的导弹";
        byte[] data=str.getBytes();
        InetAddress inet=InetAddress.getLocalHost();
        DatagramPacket packet=new DatagramPacket(data,data.length,inet,9090);
        socket.send(packet);
        socket.close();
    }
}
```

接收端

```java
package com.dreamcold.network;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPReceiver {
    public static void main(String[] args) throws IOException {
        DatagramSocket socket=new DatagramSocket(9090);
        byte[] buffer=new byte[100];
        DatagramPacket packet=new DatagramPacket(buffer,0,buffer.length);
        socket.receive(packet);
        System.out.println(new String(packet.getData(),packet.getLength()));
        socket.close();
    }
}
```

