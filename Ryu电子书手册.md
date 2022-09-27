# Ryu电子书手册

### 2.2 Switching Hub by OpenFlow

OpenFlow交换机可以通过接收来自OpenFlow控制器(如Ryu)的指令来执行以下操作。
1.重写接收到的数据包的地址或从指定端口传输数据包。
2.将接收到的数据包传输到控制器(数据包输入)。
3.从指定端口传输控制器转发的数据包(数据包输出)。
上述的功能所组合起来的就是一台交换器的实现。



首先，控制器需要使用Packet-In函数来学习MAC地址。控制器可以使用（Packet-In）分组输入函数从交换机接收数据包。交换机分析收到的数据包以获取MAC地址 以及有关所连接端口的信息。

 学习后，交换机传输收到的数据包。交换机调查目的数据包的MAC地址是否属于学习到的主机。根据调查结果，交换机执行 后续处理。

 如果是已经存在记录中的 host：使用 Packet-Out功能转送至先前所对应的端口 ．

 如果是尚未存在记录中的 host：使用 Packet-Out 功能来达到泛洪

1.初始状态

![image-20220907150949628](C:\Users\14508\Desktop\ty笔记\assets\image-20220907150949628.png)



2.hostA->hostB

当数据包从主机A发送到主机B时，会触发pack-in函数，hostA的mac地址会被端口1给记录下来。由于 host B 的 MAC 地址尚未被学习，所以数据包 泛洪并被主机B和主机C接收。

说明：交换机具有mac地址表和flow表

![image-20220907151227473](C:\Users\14508\Desktop\ty笔记\assets\image-20220907151227473.png)



![image-20220907151306452](C:\Users\14508\Desktop\ty笔记\assets\image-20220907151306452.png)



eth-dst：目的主机

eth-src:：源主机



3.hostB->hostA

hostB收到数据包并进行响应。当数据包从主机B返回到主机A时，一个流表项(Flow Entry)被添加到流表中，并且把数据包传输到端口1。因此，主机C不会收到数据包。

![image-20220907151901222](C:\Users\14508\Desktop\ty笔记\assets\image-20220907151901222.png)



4.host A → host B

再进行一次，当数据包从主机A发送到主机B时，一个流表项(Flow Entry)被添加到流表中，并且还
数据包被传输到端口4。

![image-20220907152225594](C:\Users\14508\Desktop\ty笔记\assets\image-20220907152225594.png)



### 类的定义和初始化

1.为了实现Ryu应用，因此继承了 ryu.base.app_manager.RyuApp。

接着为了使用 OpenFlow 1.3 ，将 OFP_VERSIONS 指定为 OpenFlow 1.3。

然后，MAC地址表的mac_to_port也已经被定义。

（构造方法中初始化成员变量语法：self.属性）

![image-20220907155339293](C:\Users\14508\Desktop\ty笔记\assets\image-20220907155339293.png)



2.事件管理

对于Ryu而言，当接收到OpenFlow消息时，会生成与该消息对应的事件。Ryu应用程序必须实现对应于期望接收的消息并予以处理。 

事件管理（Event Handler ）是一个拥有事件对象（Event Object ）做为参数，并使用 

Ryu . controller . handler . set _ ev _ cls 装饰(decorator)的函数。



Ryu的一个学习网址：[欢迎使用 RYU 网络操作系统 （NOS） — Ryu 4.34 文档](https://ryu.readthedocs.io/en/latest/)



事件类别名称的规则为 ryu.controller.ofp_event.EventOFP +<OpenFlow 消息名称 > ，例 如：在Packet-In 消息的状态下的事件名称为 ofp_event.EventOFPPacketIn 。对于状态来说，请指定下面列表的其中一项。（以下四个名称为四个状态，而SwitchFeatures则是）

![image-20220907160713825](C:\Users\14508\Desktop\ty笔记\assets\image-20220907160713825.png)

###### 新增 Table-miss Flow Entry

在与OpenFlow交换机的握手完成之后，新增 Table-miss Flow Entry 到 Flow table 中，以准备接收Pack-in消息。 具体地，在接收到到Switchfeatures（Features reply）消息时，就会新增Table-miss FlowEntry

![image-20220907162455554](C:\Users\14508\Desktop\ty笔记\assets\image-20220907162455554.png)

ev是存储事件对应的OpenFlow消息类的实例。在这个例子中,它是 ryu.ofproto.ofproto_v1_3_parser.OFPSwitchFeatures 。

msg.datapath，这个消息用来储存 OpenFlow 交 换 器 的 ryu.controller.controller.Datapath 类对应的实例。

Datapath类是用来处理OpenFlow交换机重要的消息，例如与OpenFlow开关和发布对应于所接收消息的事件。

Ryu应用程序中Datapath类的主要方法如下： **send_msg(msg) 发 送 OpenFlow 消息**。msg 是 发 送 OpenFlow 消息 ryu.ofproto.ofproto_parser.MsgBase 类别的子类别。 **交换机本身不仅仅使用 Switch features 消息，还使用事件处理以取得新增 Table-miss Flow  Entry 的时间点。**

![image-20220907170004722](C:\Users\14508\Desktop\ty笔记\assets\image-20220907170004722.png)

match是一个匹配域

Table-miss Flow Entry 的优先权为 0 即最低的优先权，而且此 Entry 可以 match 所有的分组。 这个 Entry 的 Instruction 通常指定为 output action ，并且输出的端口将指向控制器。因 此 当分组没有match任何一个普通Flow Entry时，则触发Packet-In。

Table-miss Flow Entry 具有最低 (0) 优先级，并且该条目匹配所有数据包。在该条目的指令中，通过指定输出到控制器端口的输出动作，如果接收到的数据包与任何正常流目条目不匹配，则发出Packet-In。

生成一个空匹配来匹配所有数据包。match表示于OFPMatch类别中。

接下来，OUTPUT action 类别（OFPActionOutput ）的一个实例以传输到控制器端口。控制器被指定为输出目的地，OFPCML_NO_BUFFER被指定为中的max_len 命令将所有数据包发送到控制器。

最后，0(最低)被指定为优先级，并执行add_flow()方法来发送流模式 消息。add_flow()方法的内容将在后面的章节中解释



### Packet-in消息

创建数据包传入事件处理程序的处理程序，以便接受接收到的目的地未知的数据包。

![image-20220907181810263](C:\Users\14508\Desktop\ty笔记\assets\image-20220907181810263.png)

match:Ryu . of proto . of proto _ v1 _ 3 _ parser.OFPMatch类实例，其中设置存储的分组的元信息

data:表示收到的数据包本身的二进制数据。

total_len : 接收数据包的数据长度

buffer_id：当收到的数据包在OpenFlow交换机上缓冲时，指示其ID。如果在没有buffer 的状况下，则设定 ryu.ofproto.ofproto_v1_3.OFP_NO_BUFFER

### 更新mac地址表

![image-20220907182708913](C:\Users\14508\Desktop\ty笔记\assets\image-20220907182708913.png)

从OFPPacketIn匹配中获取接收端口(in_port)。目的MAC地址和发送方MAC 使用Ryu的数据包库从收到的数据包的以太网报头中获取地址。 基于获取的发送者MAC地址和接收的端口号，更新MAC地址表。 为了支持与多个OpenFlow交换机的连接，MAC地址表被设计为 针对每个OpenFlow交换机进行管理。数据路径ID用于识别OpenFlow交换机。

### 判断中转目的端口

目的MAC地址若存在于 MAC地址表，则判断该端口的号码为输出。反之若不存在于 MAC  地址表则 OUTPUT action 类别的实体并生成 flooding（OFPP_FLOOD ）给目的端口使用。

![image-20220907184441952](C:\Users\14508\Desktop\ty笔记\assets\image-20220907184441952.png)

若是找到了目的MAC地址，则在交换机的 Flow table 中新增。 Table-miss Flow Entry包含 match 和 action，并通过 add_flow() 来新增。

不同于平常的Table-miss Flow Entry，这次将加上设定match条件。实现交换集线器这一次，接收端口(in_port)和目的MAC地址(eth_dst)已经指定。

例如，数据包 由端口1接收的寻址到主机B的是目标。 对于这次的流条目，优先级被指定为1。值越大，优先级越高，因此， 此处添加的流条目将在表缺失流条目之前进行评估。 根据包括上述操作的摘要，将以下条目添加到流表中。

## 增加流入口的处理

Packet-In 处理程序的处理尚未完成，但这里看一下添加流条目的方法

![image-20220927145237920](assets/image-20220927145237920.png)

对于流表项，设置表示目标报文条件的match，以及表示对报文的操作、表项优先级和有效时间的指令。

在交换集线器实现中，Apply Actions 用于指令设置，以便立即使用指定的动作。

最后，通过发出 Flow Mod 消息将条目添加到流表中。

Flow Mod消息对应的类是OFPFlowMod类。生成 OFPFlowMod 类的实例，并使用 Datapath.send_msg() 方法将消息发送到 OpenFlow 交换机。



**OFPFlowMod 类的构造函数有很多参数。它们中的许多通常可以按原样默认。括号内是默认值。**

**datapath**:这是支持流表操作的 OpenFlow 交换机的 Datapath 类实例。通常，指定从传递给处理程序的事件中获取的一个，例如 Packet-In 消息。

**table_id (0)**:指定操作目标流表的表ID。

## Packet Transfer(数据包中转)

现在我们回到 Packet-In 处理程序并解释最终处理。

无论是否从 MAC 地址表中找到目的 MAC 地址，最后都会发出 Packet-Out 消息并传输接收到的数据包。

![image-20220927151631906](assets/image-20220927151631906.png)

Packet-Out消息对应的类是OFPPacketOut类。

OFPPacketOut 的构造函数的参数如下：

datapath
指定 OpenFlow 交换机对应的 Datapath 类的实例。
buffer_id
指定 OpenFlow 上缓存的数据包的缓冲区 ID。如果没有缓冲，则指定 OFP_NO_BUFFER。
in_port
指定接收数据包的端口。如果不是接收到的数据包，则指定 OFPP_CONTROLLER。
action
指定操作列表。
data
指定数据包的二进制数据。这在为 buffer_id 指定 OFP_NO_BUFFER 时使用。当使用 OpenFlow 交换机的缓冲区时，将省略此项。



在交换中心的实施中，Packet-In消息的buffer_id已经被指定为buffer_id。如果Packet-In消息的buffer-id已被禁用，那么Packet-In的接收数据包被指定为发送数据包的数据。