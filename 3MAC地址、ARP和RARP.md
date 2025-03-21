 

一、MAC地址
-------

#### 1、认识MAC地址结构

*   **每个网卡都有6字节（48bit）的MAC地址**。
*   全球唯一，固化在ROM中，由IEEE802制定
    *   前3字节：OUI，组织唯⼀标识符，由IEEE的注册管理机构分配给厂商
    *   后3字节：网络接口标识符，由厂商自行分配
*   MAC地址的表示格式
    *   Windows： 40-55-82-0A-8C-6D
    *   Linux、Android、Mac、iOS： 40:55:82:0A:8C:6D
    *   Packet Tracer： 4055.820A.8C6D

> 当48位全为1时，代表⼴播地址： FF-FF-FF-FF-FF-FF

#### 2、如何获取MAC地址

> **ARP（地址解析协议）**

*   当不知道对方主机的MAC地址时，可以通过发送**ARP广播**获取对方的MAC地址
    *   获取成功后，会缓存IP地址、MAC地址的映射信息，俗称：**ARP缓存**
    *   通过ARP广播获取的MAC地址，属于`动态（dynamic）缓存`
    *   存储时间比较短（默认是2分钟），过期就自动删除

二、ARP是什么
--------

#### 1、简单理解

[ARP 协议](https://so.csdn.net/so/search?q=ARP%20%E5%8D%8F%E8%AE%AE&spm=1001.2101.3001.7020)的全称是 Address Resolution Protocol(`地址解析协议`)，**它是⼀个通过用于实现从 IP 地址到 MAC 地址的映射，即询问目标 IP 对应的 MAC 地址 的⼀种协议**。ARP 协议在 IPv4 中极其重要。

#### 2、ARP 的工作机制

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/97aa571929704eb985c4eb0b97ac7199.png#pic_center)

1.  **ARP请求**（广播）：
    
    *   **设备A**检查ARP缓存，未找到192.168.1.3对应的MAC地址。
    *   **设备A**广播ARP请求，询问“谁的IP地址是192.168.1.3？”。
2.  **ARP响应**（单播）：
    
    *   局域网内所有设备收到ARP请求，只有**设备B**发现请求的IP地址与自己的匹配。
    *   **设备B**向**设备A**发送ARP响应，包含自己的MAC地址66:77:88:99:AA:BB 。
3.  **更新ARP缓存**：
    
    *   **设备A**收到ARP响应后，将192.168.1.3和66:77:88:99:AA:BB 的映射存入ARP缓存。
4.  **数据传输**：
    
    *   **设备A**使用**设备B**的MAC地址封装数据帧并发送。

#### 3、ARP缓存

##### 1、**ARP缓存的作用**

*   **加速通信**：避免每次通信都发送ARP请求。
*   **减少网络流量**：通过缓存减少ARP广播请求。

##### 2、**ARP缓存的内容**

*   **IP地址**：目标设备的IP地址。
*   **MAC地址**：与IP地址对应的MAC地址。
*   **缓存时间**：每个条目有生存时间（TTL），超时后删除。

##### 3、**ARP缓存的管理**

*   **动态条目**：通过ARP协议自动添加，TTL通常为几分钟到几小时。
*   **静态条目**：手动添加，永久有效，需手动删除。

##### 4、ARP缓存过程

⼀般来说，发送过⼀次 ARP 请求后，再次发送相同请求的几率比较大，因此使用 ARP 缓存能够减 少 ARP 包的发送，除此之外，不仅仅 ARP 请求的发送方能够缓存 ARP 接收方的 MAC 地址，接收方也 能够缓存 ARP 请求方的 IP 和 MAC 地址。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d33d009b629f4383bf539b5fcda17405.png#pic_center)  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/91bd31cb019b4227a043ec94baef48cc.png#pic_center)

不过，**MAC 地址的缓存有一定期限，超过这个期限后，缓存的内容会被清除**。

##### 5、 **查看ARP缓存**

*   **Windows**：使用`arp -a`命令。
*   **Linux/Unix**：使用`arp -n`或`ip neigh show`命令。
*   **Cisco设备**：使用`show arp`命令。

三、RARP（了解）
----------

RARP是将ARP反过来，**从MAC地址定位IP地址的一种协议**，将打印机服务器等小型嵌入式设备接入网络时会使用到。

本文转自 <https://blog.csdn.net/2401_82911768/article/details/145692907?spm=1001.2014.3001.5502>，如有侵权，请联系删除。