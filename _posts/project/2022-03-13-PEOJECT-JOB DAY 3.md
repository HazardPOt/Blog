# PEOJECT-JOB DAY 3
## JAVA基础
复习：https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/toc/JAVA_BASE.md
复习：https://github.com/rbmonster/learning-note/blob/master/src/main/java/com/toc/COLLECTION.md
## 计网基础
### 什么是三次握手 (three-way handshake)？
![](https://github.com/wolverinn/Waking-Up/raw/master/_v_images/20191129101827556_21212.png)
* 第一次握手：Client将SYN置1，随机产生一个初始序列号seq发送给Server，进入SYN_SENT状态；
* 第二次握手：Server收到Client的SYN=1之后，知道客户端请求建立连接，将自己的SYN置1，ACK置1，产生一个acknowledge number=sequence number+1，并随机产生一个自己的初始序列号，发送给客户端；进入SYN_RCVD状态；
* 第三次握手：客户端检查acknowledge number是否为序列号+1，ACK是否为1，检查正确之后将自己的ACK置为1，产生一个acknowledge number=服务器发的序列号+1，发送给服务器；进入ESTABLISHED状态；服务器检查ACK为1和acknowledge number为序列号+1之后，也进入ESTABLISHED状态；完成三次握手，连接建立。

#### Q：TCP建立连接可以两次握手吗？为什么?
A: **首先**，可能会出现已失效的连接请求报文段又传到了服务器端。（如果通过两次握手建立连接，Client端发送SYN，但是由于网络波动在网络中阻塞。Client端的计时器超时，再次发送SYN，Server端接收到第二次发送的SYN，建立连接。过了一段时间后Server端接收到第一次发送的失效SYN，建立连接，可是建立连接后由于第一次的SYN已失效，Client端并不会通过第一次的连接来发送数据，导致Server端的资源浪费）**其次**，两次握手无法保证Client正确接收第二次握手的报文（Server无法确认Client是否收到），也无法保证Client和Server之间成功互换初始序列号。

#### Q：可以采用四次握手吗？为什么？
A：可以，在Server端，ACK和Sequance number分开发送，将一次握手拆分成两次，但是会造成资源的浪费。

#### Q：第三次握手中，如果客户端的ACK未送达服务器，会怎样？
A：**Server端**会重复发送SYN+ACK到Client端（默认发送五次，五次后还没有得到ACK便进入Close状态），使Client端再次发送ACK到Server。**Client端**

#### Q：如果已经建立了连接，但客户端出现了故障怎么办？
A：Server每次发送数据都会重置一个计时器（2hours），若定时器超时，则会向Client发送探测报文探测Server的状态，若发送10次后没有回应，则Server进入Close状态。

#### Q：初始序列号是什么
A：TCP连接的一方A，随机选择一个32位的序列号（Sequence Number）作为发送数据的初始序列号（Initial Sequence Number，ISN）

### 四次挥手
![](https://github.com/wolverinn/Waking-Up/raw/master/_v_images/20191129112652915_15481.png)
* 第一次挥手：Client将FIN置为1，发送一个序列号seq给Server；进入FIN_WAIT_1状态；
* 第二次挥手：Server收到FIN之后，发送一个ACK=1，acknowledge number=收到的序列号+1；进入CLOSE_WAIT状态。此时客户端已经没有要发送的数据了，但仍可以接受服务器发来的数据。
* 第三次挥手：Server将FIN置1，发送一个序列号给Client；进入LAST_ACK状态；
* 第四次挥手：Client收到服务器的FIN后，进入TIME_WAIT状态；接着将ACK置1，发送一个acknowledge number=序列号+1给服务器；服务器收到后，确认acknowledge number后，变为CLOSED状态，不再向客户端发送数据。客户端等待2*MSL（报文段最长寿命）时间后，也进入CLOSED状态。完成四次挥手。

#### Q：为什么不能把服务器发送的ACK和FIN合并起来，变成三次挥手（CLOSE_WAIT状态意义是什么）？
A：应为在第二次挥手后，**可能Server端还有数据没有发送完成，先回复ACK表示收到的断开连接的请求，等数据发送完成后再将FIN置为1**，发送SequenceNumber。

#### Q：如果第二次挥手时服务器的ACK没有送达客户端，会怎样？
A：Client端计时器超时，**Client端重复发送FIN = 1，等待Server端重新发送ACK**。


#### Q：客户端TIME_WAIT状态的意义是什么？
A：**第四次挥手时，客户端发送给服务器的ACK有可能丢失**，TIME_WAIT状态就是用来重发可能丢失的ACK报文。如果Server没有收到ACK，就会重发FIN，如果Client在2*MSL的时间内收到了FIN，就会重新发送ACK并再次等待2MSL，防止Server没有收到ACK而不断重发FIN。

### TCP如何实现流量控制？
使用滑动窗口进行流量控制，设发送端为A，接收端为B，**A负责发送数据，B负责流量控制。接收端维护一个接收窗口，窗口大小是视网络情况而定，在接收端返回确认报文时在TCP报文表头窗口字段告知发送端窗口大小，发送窗口不得大于接收窗口，只有当发送方发送并收到确认后，才能将发送窗口右移。**
发送窗口的上限为接受窗口和拥塞窗口中的较小值。接受窗口表明了接收方的接收能力，拥塞窗口表明了网络的传送能力。
![](https://github.com/wolverinn/Waking-Up/raw/master/_v_images/1615897397.gif)
### TCP的拥塞控制是怎么实现的？
![](https://github.com/wolverinn/Waking-Up/raw/master/_v_images/20191129153624025_28293.png)
拥塞控制主要由四个算法组成：**慢启动（Slow Start）**、**拥塞避免（Congestion voidance）**、**快重传 （Fast Retransmit）**、**快恢复（Fast Recovery）**
* 慢启动：刚开始发送数据时，先把拥塞窗口（congestion window）设置为一个最大报文段MSS的数值，每收到一个新的确认报文之后，就把拥塞窗口加1个MSS。这样每经过一个传输轮次（或者说是每经过一个往返时间RTT），拥塞窗口的大小就会加倍。（**人话：拥塞窗口指数增长**）
* 拥塞避免：当拥塞窗口的大小达到慢开始门限(slow start threshold)时，开始执行拥塞避免算法，拥塞窗口大小不再指数增加，而是线性增加，即每经过一个传输轮次只增加1MSS.（**人话：当慢启动增长到门限值时，拥塞窗口改为线性增长，每次拥塞窗口+1**）
* 快重传：快重传要求接收方在收到一个失序的报文段后就立即发出重复确认（为的是使发送方及早知道有报文段没有到达对方）而不要等到自己发送数据时捎带确认。快重传算法规定，发送方只要一连收到三个重复确认就应当立即重传对方尚未收到的报文段，而不必继续等待设置的重传计时器时间到期。（**人话：若此时网络不稳定，发送端持续发送数据包，接收端确认接收到的最后一个连续数据包，连续三次后，发送端重新发送丢失数据包**）
![](https://github.com/wolverinn/Waking-Up/raw/master/_v_images/20191129161026032_32431.png)

* 快恢复：收到丢失的数据包后，门限值设置为当前拥塞窗口的一半，并继续进行拥塞控制。

### TCP如何最大利用带宽？
* **窗口**：即滑动窗口大小
* **带宽**：这里带宽是指单位时间内从发送端到接收端所能通过的“最高数据率”，是一种硬件限制。
* **RTT**：即Round Trip Time，表示从发送端到接收端的一去一回需要的时间，TCP在数据传输过程中会对RTT进行采样（即对发送的数据包及其ACK的时间差进行测量，并根据测量值更新RTT值），TCP根据得到的RTT值更新RTO值，即Retransmission TimeOut，就是重传间隔，发送端对每个发出的数据包进行计时，如果在RTO时间内没有收到所发出的数据包的对应ACK，则任务数据包丢失，将重传数据。一般RTO值都比采样得到的RTT值要大。

###  TCP与UDP的区别（**重要，必考**）
1. TCP是面向连接，UDP是无连接
2. TCP是可靠连接，UDP是不可靠连接
3. TCP只支持点对点通信，UDP支持一对一、一对多、多对一、多对多通信
4. TCP是面向字节流的，UDP是面向报文的
5. TCP有拥塞控制，UDP没有
6. TCP的报文头部为20字节，UDP为8字节
7. UDP不用维护连接状态表

### TCP如何保证传输的可靠性
1. 数据包校验
2. 对失序数据包重新排序（TCP报文具有序列号）
3. 丢弃重复数据
4. 应答机制：接收方收到数据之后，会发送一个确认（通常延迟几分之一秒）；
5. 超时重发：发送方发出数据之后，启动一个定时器，超时未收到接收方的确认，则重新发送这个数据；
6. 流量控制：确保接收端能够接收发送方的数据而不会缓冲区溢出

#### HTTP和HTTPS有什么区别？
1. 端口不同：HTTP使用的是80端口，HTTPS使用443端口；
2. HTTP（超文本传输协议）信息是明文传输，HTTPS运行在SSL(Secure Socket Layer)之上，添加了加密和认证机制，更加安全；
3. HTTPS由于加密解密会带来更大的CPU和内存开销；
4. HTTPS通信需要证书，一般需要向证书颁发机构（CA）购买

## Java爬虫豆瓣 DAY1

## 剑指