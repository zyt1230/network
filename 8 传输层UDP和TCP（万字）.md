 

一、传输层概述
-------

### 1、传输层的作用

*   ①提供多台主机**应用进程之间**的**端到端的传输行为**（应用层采用进程号来识别不同的运行的程序）。
*   ②为不可靠的网络层是否提供可靠传输。  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/380f2e9b624647f39af883e2aa8403b8.png)  
    传输层提供了“**端口**”的概念，用来区分不同应用进程的标识符。运输层向应用层实体屏蔽了下面网络核心的细节（例如网络拓扑、所采用的路由选择协议等），**它使应用进程看见的就好像是在两个运输层实体之间有⼀条端到端的逻辑通信信道**。

### 2、传输层的TCP、UDP典型协议

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/402c7dcaca00470a91b209040df2700e.png)

### 3、传输层端口号

*   运行在计算机上的**进程是使用进程标识符**（Process Identification，**PID**）来标识的
    *   **不同操作系统**（Windows、Linux、  
        MacOS）**又使用不同格式的进程标识符**。
    *   为了使运行不同操作系统的计算机的应用进程之间能够基于网络进行通信，就**必须使用统⼀的方法对TCP/IP体系的应用进程进行标识**。
*   TCP/IP体系结构的运输层**使用端口号来标识和区分应用层的不同应用进程**。端口号的长度为16比特，取值范围是0~65535。
*   **TCP和UDP的端口号之间是没有关系的，他们是独立的**。  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6d8daa9697344bcb888a1016b793ba82.png)

### 4、TCP、UDP对比

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ae2a06acebbd493bae13fc9ab9e03594.png)

二、UDP面向数据报的协议
-------------

### 1、介绍

*   **用户数据报协议**（User Datagram Protocol，UDP）为其上层提供的是**无连接**的**不可靠**的数据传输服务。
*   使用UDP通信的双方，在传送数据之前**不需要建立连接**。
*   UDP**不需要实现可靠传输**，因此**不需要使用实现可靠传输的各种机制**。
*   UDP的实现简单，UDP用户数据报的**首部比较小**。
*   UDP是**数据报**服务。**无管道**。**只收**（**缓存区装满后，就丢包，丢哪里不管**）。

### 2、UDP报文段格式

*   因为它不可靠传输，所以它会**尽最大努力传输**，但是不保证可靠。
    *   UDP首部8字节（TCP首部至少20字节）
*   UDP长度
    *   占16位，UDP长度 = 首部的长度+数据的长度
*   校验和的计算内容 = **伪首部 + 首部 + 数据**
    *   **伪首部：仅在计算校验和时起作用，并不会传递给网络层**  
        ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3f7479aa2b7a4d4cae8a88c147272eb2.png)  
        ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7dd5253f578b417bba1415d28fa5568c.png)

三、TCP面向连接的协议
------------

### 1、介绍

*   TCP：**传输控制协议**（Transmission Control  
    Protocol，TCP），是**面向连接**且**可靠**的数据传输协议。
*   使用TCP通信的双方，在传送数据之前必须**首先建立TCP连接（逻辑连接，而非物理连接）**。**数据传输结束**后必须要**释放TCP连接**。
*   TCP**为了实现可靠传输**，就必须使用很多措施，例如**TCP连接管理、确认机制、超时重传、流量控制以及拥塞控制等**。
*   TCP的实现复杂，TCP报文段的**首部比较大**，**占用处理机资源比较多**。
*   TCP是**数据流**服务。应用了**管道的思想**，队列数据结构，FIFO（先进先出），也就是一个 （缓存区）buffer。

### 2、TCP报文段格式

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5f1e80f1d7e24e35adb627108662fa87.png)

*   Source Port (源端口) (16位):
    
    *   发送方的端口号，用于**标识发送数据的应用程序**。
*   Destination Port (目的端口) (16位):
    
    *   接收方的端口号，用于**标识接收数据的应用程序**。
*   Sequence Number (序列号/序号) (32位):
    
    *   用于标识发送的数据字节流中的第一个字节的序列号。在建立连接时，双方会交换初始序列号（ISN）。
*   Acknowledgment Number (确认号) (32位):
    
    *   期望接收的下一个字节的序列号。只有在ACK标志位被设置时，该字段才有效。
    
    [序号和确认号的举例，可以看这个视频理解](https://www.bilibili.com/video/BV1kt4y1K74y/?spm_id_from=333.337.search-card.all.click&vd_source=c6d25b8c148a15b1c45b8da5d613a042)
    
*   Data Offset (数据偏移) (4位):
    
    *   指示**TCP头部长度**，以32位字为单位。该字段的**最小值为5（即20字节），最大值为15（即60字节）**。
*   Reserved (保留) (3位):
    
    *   保留字段，**必须设置为0**。
*   Control Flags (控制标志) (9位):
    
    *   包含多个标志位，用于控制TCP连接的状态和数据传输：
        *   URG (Urgent): 指紧急指针字段有效。
        *   ACK (Acknowledgment): 确认号字段有效。当ACK=1时有效，ACK=0时无效。**注意：TCP连接建定后，所有传送TCP报文段都必须把ACK设置为1**.
        *   PSH (Push): 接收方应**立即**将数据传递给应用程序。**发送方TCP把PSH置1，并立即创建⼀个TCP报文段发送出去，而不需要积累到足够多的数据再发送**。
        *   RST (Reset): 重置连接。**当RST=1时，表明TCP连接中出现严重差错，必须释放连接，然后再重新建立连接**。
        *   SYN (Synchronize): 同步序列号，用于建立连接。
        *   FIN (Finish): 发送方已完成数据发送，请求关闭连接。**当FIN=1时，表明此TCP报文段的发送方已经将全部数据发送完毕，现在要求释放TCP连接**。
*   Window Size (窗口大小) (16位):
    
    *   **接收方当前可接收的数据量，可以用来表示接受方的接收能力，用于流量控制**。
*   [Checksum](https://so.csdn.net/so/search?q=Checksum&spm=1001.2101.3001.7020) (校验和) (16位):
    
    *   用于检测**TCP头部和数据部分**的错误。
*   Urgent Pointer (紧急指针) (16位):
    
    *   指示**紧急数据的位置**，只有在URG标志位被设置时有效。
*   Options (选项) (可变长度):
    
    *   可选字段，用于支持一些高级功能，如**最大段大小（MSS）**、窗口缩放因子等。
        *   例如：
        *   窗口扩大选项：用来扩大窗口，提高吞吐率。
        *   时间戳选项：①用于计算往返时间RTT。② 用于处理序号超范围的情况，⼜称为防止序号绕回PAWS。
        *   选择确认选项：用来实现选择确认功能。
*   Padding (填充) (可变长度):
    
    *   用于**确保TCP头部长度是32位的整数倍**。
*   Data (数据) (可变长度):
    
    *   实际传输的数据部分。如果没有数据，该字段可以为空。

> *   MSS表示 **TCP 段中数据部分的最大长度**。
> *   MSS 的值通常根据网络的 MTU（Maximum Transmission Unit，最大传输单元） 确定。
> *   MSS=MTU−IP 头大小−TCP 头大小
> *   MSS 的值在 TCP 三次握手阶段通过 TCP 选项字段协商确定。
> *   通过合理设置 MSS，可以避免 IP 分片，提高网络传输效率。  
>     MTU：网络链路层支持的最大传输单元。以太网的默认 MTU 为 1500 字节。
>     *   IP 头大小：通常为 20 字节。
>     *   TCP 头大小：通常为 20 字节。
>     *   因此，在以太网中，MSS 的默认值为:MSS=1500−20−20=1460字节

通过限制 TCP 段的大小，确保 TCP 段加上 IP 头和 TCP 头后不超过 [MTU](https://so.csdn.net/so/search?q=MTU&spm=1001.2101.3001.7020)，从而避免 IP 分片。

### 3、TCP可靠传输

#### 3.1、ARQ协议的关键机制

ARQ（Automatic Repeat–reQuest）自动重传请求。

*   确认机制（ACK）：
    *   接收方通过发送ACK来确认已正确接收数据。
    *   可以是累积确认（如Go-Back-N ARQ）或单独确认（如Selective Repeat ARQ）。
*   超时重传机制：
    *   发送方为每个发送的帧设置一个定时器。如果在超时时间内未收到ACK，则重传该帧。
*   滑动窗口：
    *   用于控制发送方和接收方的缓冲区大小，实现流量控制和可靠传输。
*   错误检测：
    *   通过校验和（Checksum）或循环冗余校验（CRC）检测数据帧是否出错。

#### 3.2、停止等待ARQ协议

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/aafe4c254d7b489aa3dc18e5b42dd255.png)

*   停等ARQ是最简单的ARQ协议，其**工作原理**如下：
    
    *   发送方发送**一个数据帧**后，等待接收方的确认（ACK）。【**注意停止等待是一个数据帧**】
    *   接收方收到数据帧后，检查其正确性（通过校验和等方式）。如果数据正确，**接收方发送ACK**；如果数据错误，接收方丢弃该帧并等待发送方重传。
    *   如果发送方在超时时间内未收到ACK，则会重传该数据帧。
    
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e3afa8d7e6ea4531a2c004a9b5403144.png)
    

> **若有个包重传了N次还是失败，会⼀直持续重传到成功为止吗？**  
> 取决于系统的设置，比如重传了4次还未成功就会发送RST报文，断开连接。

#### 3.3、连续ARQ协议 + 滑动窗口协议

【**注意是数据段**】

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/37c78a8753f74a6cb419957ad0c79bfb.png)

*   如果接收窗口最多能接4个包，但是发送方只发送了2个包
    
    *   接收方等待一段时间后，没有收到第3个包，就会确认收到2个包给发送方
*   序号和确认号
    
    *   在传输过程的每一个字节都会有一个编号
    *   在建立连接后，序号表示，这一次传给对方的TCP数据部分的第一个字节的编号
    *   在建立连接后，确认号代表，期望对方下一次传过来的TCP数据部分的第一个字节的编号
    
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b551cb06e84e4da99d5cad18027ced94.png)
    

> 滑动窗口的大小与接收方能力有关。

#### 3.4、选择性确认（SACK）

在TCP通信过程中，如果发送序列中间某个数据包丢失（比如1、2、3、4、5中的3丢失了），TCP会通过重传最后确认的分组后续的分组（最后确认的是2，会重传3、4、5），这样**原先已经正确传输的分组也可能重复发送**（比如4、5），**降低了TCP性能**。

*   所以发展出SACK技术
    *   告诉发送方哪些数据丢失，哪些已经提前收到了
    *   使TCP只重新发送丢失的包（比如：3），不用发送后续的所有分组（比如：4 、5）
*   实现过程：SACK 信息会放在TCP首部的选项部分
    *   Kind：占1字节，SACK选项的**类型**，值为5。
        
    *   Length：占1字节，SACK选项的**长度**，单位为字节。每个SACK块占用8字节（左边缘和右边缘各4字节），因此长度为10（2字节的Kind和Length + 8字节的SACK块）。
        
    *   Left Edge：已接收数据块的**起始序列号**。
        
    *   Right Edge：已接收数据块的**结束序列号（不包含该序列号本身）**。
        

```bash
+--------+--------+--------+--------+------------+
| Kind （1字节）  | Length （1字节）| Left Edge        | Right Edge       |
+--------+--------+--------+--------+------------+
| 5      | 10     | 4字节            | 4字节            |
+--------+--------+--------+--------+------------+
```

*   SACK的工作过程
    
    *   接收方检测到非连续数据：
        *   如果接收方收到非连续的数据段（例如数据段1和3到达，但数据段2丢失），它会生成SACK信息。
        *   SACK信息包含已接收的非连续数据范围（例如数据段1和3）。
    *   接收方发送SACK选项：
        *   接收方在ACK报文中添加SACK选项，告知发送方哪些数据段已经正确接收。
    *   发送方处理SACK信息：
        *   发送方根据SACK信息确定哪些数据段需要重传。
        *   发送方只重传丢失的数据段（例如数据段2），而不需要重传已经正确接收的数据段。
*   SACK的示例
    
    *   假设发送方发送了以下数据段：
        *   段1：序列号=100，长度=100字节
        *   段2：序列号=200，长度=100字节
        *   段3：序列号=300，长度=100字节
        *   如果接收方收到段1和段3，但段2丢失，接收方会发送以下SACK信息：
        *   ACK=200（表示期望接收序列号为200的数据）
        *   SACK块：左边缘=300，右边缘=400（表示已接收序列号300-399的数据）
        *   发送方根据SACK信息，只需重传段2（序列号=200，长度=100字节）。

> 注意：选项最多40字节，所以SACK选项最多携带4组边界信息。最大占用4\*8+2 = 34字节。 2 是指Kind和length。

> **为什么传输层分段，而不是网络层？**  
> 可靠传输在传输层控制的。  
> ① 如果在传输层不分段，一旦出现数据丢失，整个传输层的数据都得重传  
> ②如果传输层分了段，一旦出现数据丢失，只需要重传丢失的那些段即可。

### 4、TCP流量控制（端到端）

#### 4.1、理解流量控制

**应用程序可能正忙于其他任务**，并**不⼀定能够立刻取走数据**。**但是客户端又在持续发送大量数据**，接收端**缓存会溢出，造成数据丢失**。  
所以**TCP为应用程序提供了流量控制（Flow Control）机制**，以解决因发送方发送数据太快而导致接收方来不及接收，造成接收方的接收缓存溢出的问题。

#### 4.2、基本情况

流量控制的方法：接收方根据自己的接收能力（接收缓存的可用空间大小）控制发送方的发送速率。

*   **通过确认报文中窗口字段来控制发送方的发送速率**
*   发送方的发送窗口大小不能超过接收方给出窗口大小
*   当发送方收到接收窗口的大小为0时，发送方就会停止发送数据

rwnd：r是接收的意思，wnd是window的意思。总结为接收窗口的大小。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d80f6ce7d86e4fb8b97950cf8136a46d.png)

#### 4.3、特殊情况

⼀开始，接收方给发送方发送了0窗口的报文段，后面，接收方又有了⼀些存储空间，给发送方发送的非0窗口的报文段丢失了，发送方的发送窗口⼀直为零，双方陷入僵局。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/500c8563c8e9494a82ee03cbdec966b4.png)

*   解决方法：
    *   当发送方收到0窗口通知时，这时发送方停止发送报⽂
    *   并且同时开启⼀个定时器，**隔⼀段时间就发个测试报文**去询问 接收方最新的窗口大小
    *   如果接收的窗口大小还是为0，则发送方**再次刷新启动定时器**

> *   例题
>     *   主机甲和主机乙之间建立了⼀个TCP连接，TCP最大段长度为1000字节。若主机甲的当前拥塞窗口为4000字节，在主机甲向主机乙连续发送两个最⼤段后，成功收到主机乙发送的第⼀个段的确认段，确认段中通告的接收窗口大小为2000字节，则此时主机甲还可以向主机乙发送的最大字节数是（ 1000字节）。
>         *   答：发送方的发送窗口的上限值取接收方窗口和拥塞窗口中的最小值，即min(4000, 2000)=2000B，由于**还未收到第二个最大段的确认**，所以此时主机甲还可以向主机乙发送的最大字节数为2000-1000=1000B。

### 5、拥塞控制（网络集合之间）

#### 5.1、理解拥塞控制

*   拥塞：在某段时间，**若对网络中某⼀资源的需求超过了该资源所能提供的可用部分**，**网络性能就要变坏**，这种情况就叫作拥塞（congestion）。
*   计算机网络中的链路容量（带宽）、交换节点中的缓存和处理机等都是**网络的资源**。  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7fd69498ab7b493a9c8d66be230a749c.png)
*   拥塞控制是全局性的过程（**网络集合之间**）
    *   涉及到所有的主机、路由器，以及与降低网络传输性能有关的所有因素。
    *   是需要靠所有节点共同努力的结果
    *   防止过多的数据注入到网络中，使网络能够承受现有的网络负荷。
*   相比之下，流量控制是点到点通信的控制。

#### 5.2、拥塞控制的常用方法

> 理解窗口：  
> cwnd（congestion window）：拥塞窗口  
> rwnd（receive window）接收窗口  
> swnd（send window）：发送窗口 swnd= min（rwnd，cwnd）

##### 5.2.1、慢开始

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/dec7d62a1e4a4f0a86e249103209f13c.png)

*   慢开始：发送方开始时以较低的速率发送数据，随后逐渐增加发送速率，直到检测到网络拥塞或达到某个阈值，可能开始减少cwnd，让swnd变小。
*   cwnd的初始值比较小，然后随着数据包被接收方确认（收到⼀个ACK）
    *   cwnd后面是指数级增长的速度

##### 5.2.2、拥塞避免

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e49c94ca62194d17bd9a5fbe0d8e803f.png)

*   拥塞避免：在**慢启动达到阈值**后，发送方进入拥塞避免阶段，此时发送速率会以**线性方式增长**，而不是指数增长，从而更谨慎地避免拥塞。
*   ssthresh指慢启动阈值（slow start threshold）：慢开始阈值，cwnd达到阈值后，以线性方式增加
*   拥塞避免（**加法增大**）：**拥塞窗口缓慢增大**，**以防止网络过早出现拥塞**
*   **乘法减小**：只要网络出现拥塞，**把ssthresh减为拥塞峰值的⼀半**，同时执行慢开始算法（cwnd又恢复到初始值）
*   当网络出现频繁拥塞时，ssthresh值就下降的很快

##### 5.2.3、快速重传

*   快速重传：当接收方**检测到数据包丢失时**，会立即发送**重复的确认（ACK）**，发送方在收到多个重复ACK后，**会快速重传丢失的数据包，而不必等待超时**。
*   接收方
    *   每收到一个**失序的分组**后就立即发出重复确认
    *   使发送方**及时**知道有分组没有到达
    *   而不要等待自己发送数据时才进行确认
*   发送方
    *   只要连续收到三个重复确认（总共4个相同的确认），就应当立即重传对方尚未收到的报文段
    *   而不必继续等待重传计时器到期后再重传
*   对于个别丢失的报文段，发送方不会出现超时重传，也就不会误认为出现了拥塞而错误地把拥塞窗口cwnd的值减为1。实践证明，使用快重传可以使整个网络的吞吐量提高约20%。  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b40f63fe7a414cbb93dcbc852bbba821.png)

##### 5.2.4、快速恢复

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7582b1c7977b4a7e9c6c4599dd478b04.png)

*   快速恢复：在快速重传后，发送方进入快速恢复阶段，**调整发送速率以避免进一步拥塞**，同时继续传输数据。
*   当发送方连续收到三个重复确认，说明网络出现拥塞
    *   就执行“乘法减小”算法，把ssthresh减为拥塞峰值的⼀半
    *   与慢开始不同之处是现在**不执行慢开始算法**，即**cwnd现在不恢复到初始值**
        *   而是把cwnd值设置为新的ssthresh值（减小后的值）
        *   然后开始执行拥塞避免算法（“**加法增大**”），使拥塞窗口缓慢地线性增大

### 6、TCP的建立连接和释放连接

#### 6.1、三次握手（建立连接）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f5ef05d4b4d3494d8819c71cc4db0562.png)

```bash
客户端（Client）                            服务器（Server）
    |                                          |
    | ----------- SYN (Seq = x) ------------>  | 第一次握手
    | <--------- SYN+ACK (Seq = y, Ack = x+1) -| 第二次握手
    | ----------- ACK (Ack = y+1) -----------> | 第三次握手
    |                                          |
```

> *   第一次握手：SYN
>     *   客户端向服务器发送一个TCP报文段，其中：
>         *   SYN标志位设置为1，表示请求建立连接。
>         *   **序列号（Seq）为一个随机值**（例如Seq = x），用于初始化客户端的序列号。
>         *   客户端进入**SYN\_SENT状态**，**等待服务器的确认**。

> *   第二次握手：SYN + ACK
>     *   服务器收到客户端的SYN报文后，向客户端发送一个TCP报文段，其中：
>         *   SYN标志位设置为1，表示同意建立连接。
>         *   ACK标志位设置为1，表示确认客户端的SYN报文。
>         *   确认号（Ack）为客户端序列号加1（即Ack = x + 1）。
>         *   序列号（Seq）为一个随机值（例如Seq = y），用于初始化服务器的序列号。
>         *   服务器进入SYN\_RECEIVED状态。

> *   第三次握手：ACK
>     *   客户端收到服务器的SYN+ACK报文后，向服务器发送一个TCP报文段，其中：
>         *   ACK标志位设置为1，表示确认服务器的SYN报文。
>         *   确认号（Ack）为服务器序列号加1（即Ack = y + 1）。
>         *   序列号（Seq）为x + 1（即第一次握手中的序列号加1）。
>         *   客户端和服务器都进入**ESTABLISHED状态**，连接建立完成。

*   那么为什么使用的是三次握手，而不是二次握手？
    *   答 ：如果只有两次握手，服务器无法确认客户端是否收到了自己的SYN+ACK报文。如果客户端的ACK丢失，服务器会一直等待，导致资源浪费。
    *   通过三次握手，双方都能确认连接已建立，避免了资源浪费和数据传输错误。

#### 6.2、4次挥手（释放连接）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/777a892f727543ae94044c97a3e89bbe.png)

```bash
主动关闭方（Client）                          被动关闭方（Server）
    |                                          |
    | ------------ FIN (Seq = u) ------------> | 第一次挥手
    | <--------- ACK (Ack = u + 1) ----------- | 第二次挥手
    |                                          |
    | <--------- FIN (Seq = w) --------------  | 第三次挥手
    | ----------- ACK (Ack = w + 1) --------> | 第四次挥手
    |                                          |
```

**发出挥手不一定是客户端，客户和服务皆有可能**。

> 第一次挥手
> 
> *   序号seq字段的值设置为u，它等于TCP客户进程之前已经传送过的数据的**最后⼀个字节的序号**  
>     加1。
>     *   **主动**关闭方（**如客户端**）发送一个TCP报文段，其中：
>         *   **FIN标志位设置为1**，表示**请求终止连接**。
>         *   **序列号Seq**为当前已发送数据的最后一个字节序号加1（例如Seq = u）。
>         *   **确认号ack**字段的值设置为v，它等于TCP客户进程之前已收到的数据的**最后⼀个字节的序号加1**。
>     *   主动关闭方进入**FIN\_WAIT\_1**状态，等待对方的确认。

> 第二次挥手
> 
> *   **被动**关闭方（如**服务器**）收到FIN报文后，发送一个确认报文段，其中：
>     *   **ACK标志位设置为1**，表示确认收到的FIN报文。
>     *   **确认号**（Ack）为u + 1（即主动关闭方的序列号加1）。
>     *   **序号seq**字段的值设置为v，它等于TCP服务器进程之前已传送过的数据的最后⼀个字节的序号加1。这也与之前收到的TCP连接释放报文段中的确认号v匹配。
> *   被动关闭方进入**CLOSE\_WAIT**状态，此时仍可继续发送未完成的数据。
> *   **主动关闭方收到ACK后，进入FIN\_WAIT\_2状态**，等待被动关闭方的FIN报文。
> *   经过两次挥手后，从**TCP客户进程到TCP服务器进程这个方向的连接就释放了**，但TCP服务器进程如果还有数据要发送，TCP客户进程仍要接收，也就是从**TCP服务器进程到TCP客户进程这个方向的连接并未关闭**。

> 第三次挥手
> 
> *   被动关闭方完成数据发送后，发送一个FIN报文段，其中：
>     *   **FIN标志位**设置为1，表示请求终止自己的数据通道。
>     *   **确认标志位ACK**设置为1。**FIN和ACK**表明这是⼀个TCP连接释放报文段，同时也对之前收到的TCP报文段进行确认。
>     *   **序号seq**字段的值假定被设置为w，这是因为在半关闭状态下TCP服务器进程可能又发送了⼀些数据。
>     *   **确认号ack**字段的值被设置为u+1，这是对之前收到的TCP连接释放报文段的重复确认。
> *   被动关闭方进入LAST\_ACK状态，等待最后的确认。

> 第四次挥手
> 
> *   主动关闭方收到FIN报文后，发送一个确认报文段，其中：
>     *   **序号seq**字段的值设置为u+1，这是因为TCP客户进程之前发送的TCP连接释放报文段虽然不携带数据，但要消耗掉⼀个序号。
>     *   **ACK标志位**设置为1。
>     *   **确认号ack**字段的值设置为w+1，这是对所收到的TCP连接释放报⽂段的确认。
> *   主动关闭方TCP客户端进⼊TIME-WAIT状态，等待2MLS，MSL是最长报文段寿命（Maximum Segment  
>     Lifetime）的英文缩写词，\[RFC793\]建议为2分钟。也就是说，TCP客户进程进入时间等待  
>     （TIME-WAIT）状态后，还要经过4分钟才能进⼊关闭（CLOSED）状态。
> *   被动关闭方收到ACK后，立即关闭连接。

> *   TIME\_WAIT状态
>     *   主动关闭方在发送最后一次ACK后进入TIME\_WAIT状态，等待2MSL时间。这是为了：
>         *   确保对方收到最终的ACK。若ACK丢失，被动关闭方会重传FIN报文，主动关闭方可重新发送ACK。
>         *   防止旧连接的报文干扰新连接（通过2MSL时间使网络中残留的旧报文失效）。
> *   CLOSE\_WAIT状态
>     *   被动关闭方在发送ACK后进入CLOSE\_WAIT状态，表示正在等待应用程序关闭连接。若长期处于此状态，可能表示程序未正确关闭连接（需检查代码逻辑）。

本文转自 <https://blog.csdn.net/2401_82911768/article/details/145864981?spm=1001.2014.3001.5502>，如有侵权，请联系删除。