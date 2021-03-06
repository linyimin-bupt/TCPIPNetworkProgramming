## Socket类型与协议设置

### 协议

协议就是为了完成数据交换而定好的约定.

### 协议族(Protocal Family)

Socket通信支持多种协议, 我们可以通过`socket`函数的第一个参数指定socket使用的协议.协议的分类信息成为协议族,socket支持的协议族可分成以下几类:


|名称|协议族|
|:---:|:---|
|PF_INET|IPv4互联网协议族|
|PF_INET6|IPv6互联网协议族|
|PF_LOCAL|本地通信的UNIX协议族|
|PF_PACKET|底层套接字的协议族|
|PF_IPX|IPX Novel协议族|

### socket类型

Socket类型就是socket的数据传输方式,我们可以通过`socket`函数的第二个参数指定.

根据这个参数我们可以知道:<font color="#dd0000">一种协议可能存在多种数据传输方式</font>

#### 两种代表性的数据传输方式

##### 面向连接(SOCKET_STREAM)

> 可靠的, 按序传递的, 基于字节的面向连接的数据传输

- 传输过程中的数据不会丢失;
- 按序传输数据;
- 传输的数据不存在数据边界;

收发数据的socket内部有缓冲(buffer), 其实就是字节数组.所以收到数据并不意味着马上调用`read`函数.

##### 面向消息(SOCKET_DGRAM)

> 不可靠的, 不按次序传递的, 以数据的高速传输为目的的套接字

- 强调快速传输数据而非传输顺序;
- 传输的数据可能丢失也可能损毁;
- 传输的数据有数据边界;
- 限制每次传输的数据大小;

由于<font color="#dd0000">同一协议族中可能存在多个相同数据传输方式的协议</font>, 所以还需要第三个参数确定指定具体的协议信息.

### 习题

（1）什么是协议？在收发数据中定义协议有何意义？

协议是通信双方为了完成通信任务（数据交换）而制定的一套规则。在收发中定义协议的意义在于能够让计算机之间能进行准确无误的对话，明确交换数据时各个数据对应的含义，以此来实现数据的交换。

（2）面向连接的TCP套接字传输特性有3点，请分别说明。

1. 基于字节，按序传输数据，传输数据不存在边界。
2. 可靠的，传输过程中数据不会消失（超时重传）
3. 面向连接，双方必须经过三次握手建立连接之后，才能进行数据的传输。

（3）下列哪些是面向消息的套接字的特性？

a. 传输数据可能丢失

b. 没有数据边界（Boundary）

c. 以快速传输为目标

d. 不限制每次传输数据的大小

e. 与面向连接的套接字不同，不存在连接的概念

正确答案： a、c、e

（4）下列数据适合用哪类套接字传输？并给出原因。

a. 演唱会现场直播的多媒体数据

适合使用`面向消息（UDP）`的套接字传输，传输这类数据要求传输速度要快，而且对数据丢失有一定的容忍性。

b. 某人压缩过的文本文件

适合使用`面向连接（TCP）`的套接字传输，这类数据需要可靠传输，无法容忍数据有丢失，否则无法解压。

c. 网上银行用户与银行直接的数据传递

适合使用`面向连接（TCP）`的套接字传输，这类数据需要可靠传输，对数据丢失、错误零容忍。

（5）何种类型的套接字不存在数据边界？这类套接字接收数据时需要注意什么？

`面向连接（TCP）`的套接字不存在数据边界，而是基于字节的按序传输。需要注意的是：在接收数据时，需要保证在接收套接字缓冲填满时要从buffer中读取数据。否则写操作将阻塞，也就是在接收套接字内部，写入Buffer缓冲的速度要小于读出buffer的速度。
