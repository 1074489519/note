- TCP/IP协议族
	- #### TCP/IP协议族体系结构以及主要协议
	  collapsed:: true
		- TCP/IP协议族是一个四层协议系统，自 底而上分别是数据链路层、网络层、传输层和
		  应用层。每一层完成不同的功能，且通过若干协议来实现，上层 协议使用下层协议提供的服
		- ![image.png](../assets/image_1658849001404_0.png)
		- ##### [[数据链路层]]
			- 数据链路层实现了网卡接口的网络驱动程序，以处理数据在物理媒介(比如以太网、令
			  牌环等).上 的传输。不同的物理网络具有不同的电气特性，网络驱动程序隐藏了这些细节，
			  为上层协议提供一个统一的接口。
			- 数据链路层两个常用的协议是ARP协议(Address Resolve Protocol, 地址解析协议)和RARP协议(Reverse Address Resolve Protocol, 逆地址解析协议)。它们实现了IP地址和机
			  器物理地址(通常是MAC地址，以太网、令牌环和802.11无线网络都使用MAC地址)之
			  间的相互转换。
			- 网络层使用IP地址寻址一台机器，而数据链路层使用物理地址寻址一台机器，因此网
			  络层必须先将目标机器的IP地址转化成其物理地址，才能使用数据链路层提供的服务，这就
			  是ARP协议的用途。RARP 协议仅用于网络上的某些无盘工作站。因为缺乏存储设备，无盘
			  工作站无法记住自己的IP地址，但它们可以利用网卡上的物理地址来向网络管理者(服务器
			  或网络管理软件)查询自身的IP地址。运行RARP服务的网络管理者通常存有该网络上所有机器的物理地址到IP地址的映射。
		- ##### [[网络层]]
		  collapsed:: true
			- 网络层实现数据包的选路和转发。WAN ( Wide Area Network,广域网)通常使用众多
			  分级的路由器来连接分散的主机或LAN (Local Area Network,局域网)，因此，通信的两台
			  主机一般不是直接相连的，而是通过多个中间节点(路由器)连接的。网络层的任务就是选
			  择这些中间节点，以确定两台主机之间的通信路径。同时，网络层对上层协议隐藏了网络拓
			  扑连接的细节，使得在传输层和网络应用程序看来，通信的双方是直接相连的。
			- 网络层最核心的协议是IP协议(Internet Protocol,因特网协议)。IP协议根据数据包的
			  目的IP地址来决定如何投递它。如果数据包不能直接发送给目标主机，那么IP协议就为它
			  寻找一个合适的下一跳(next hop)路由器，并将数据包交付给该路由器来转发。多次重复
			  这一过程， 数据包最终到达目标主机，或者由于发送失败而被丢弃。可见，IP协议使用逐跳
			  (hop by hop)的方式确定通信路径。我们将在第2章详细讨论IP协议。
			- 网络层另外一个重要的协议是ICMP协议(Intermet Control Message Protocol,因特网控
			  制报文协议)。它是IP协议的重要补充，主要用于检测网络连接。ICMP协议使用的报文格
			  式如图1-2所示。
			- ![image.png](../assets/image_1658849192527_0.png)
			- 图1-2中，8位类型字段用于区分报文类型。它将ICMP报文分为两大类:一类是差错
			  报文，这类报文主要用来回应网络错误，比如目标不可到达(类型值为3)和重定向(类型
			  值为5);另一类是查询报文，这类报文用来查询网络信息，比如ping程序就是使用ICMP
			  报文查看目标是否可到达(类型值为8)的。有的ICMP报文还使用8位代码字段来进一步.
			  细分不同的条件。比如重定向报文使用代码值0表示对网络重定向，代码值1表示对主机重定向。ICMP 报文使用16位校验和字段对整个报文(包括头部和内容部分)进行循环冗余校
			  验(Cyclic Redundancy Check, CRC)，以检验报文在传输过程中是否损坏。不同的ICMP报
			  文类型具有不同的正文内容。我们将在第2章详细讨论主机重定向报文，其他ICMP报文格
			  式请参考ICMP协议的标准文档RFC 792.
			- 需要指出的是，ICMP协议并非严格意义上的网络层协议，因为它使用处于同一层的IP
			  协议提供的服务(一般来说，上层协议使用下层协议提供的服务)。
		- ##### [[传输层]]
		  collapsed:: true
			- 传输层为两台主机上的应用程序提供端到端(end to end)的通信。与网络层使用的逐跳
			  通信方式不同，传输层只关心通信的起始端和目的端，而不在乎数据包的中转过程。图1-3
			  展示了传输层和网络层的这种区别。
			- ![image.png](../assets/image_1658849310739_0.png)
			- 图1-3传输层 和网络层的区别
			  图1-3中，垂直的实线箭头表示TCP/IP协议族各层之间的实体通信(数据包确实是沿
			  着这些线路传递的)，而水平的虚线箭头表示逻辑通信线路。该图中还附带描述了不同物理
			  网络的连接方法。可见，数据链路层(驱动程序)封装了物理网络的电气细节;网络层封装
			  了网络连接的细节;传输层则为应用程序封装了一条端到端的逻辑通信链路，它负责数据的
			  收发、链路的超时重连等。
			- 传输层协议主要有三个: TCP 协议、UDP协议和SCTP协议。
			- TCP协议(Transmission Control Protocol,传输控制协议)为应用层提供可靠的、面向
			  连接的和基于流(stream)的服务。TCP协议使用超时重传、数据确认等方式来确保数据
			  包被正确地发送至目的端，因此TCP服务是可靠的。使用TCP协议通信的双方必须先建立
			  TCP连接，并在内核中为该连接维持--些必要的数据结构，比如连接的状态、读写缓冲区,以及诸多定时器等。当通信结束时，双方必须关闭连接以释放这些内核数据。TCP服务是基
			  于流的。基于流的数据没有边界(长度)限制，它源源不断地从通信的一-端流人另一端。发
			  送端可以逐个字节地向数据流中写人数据，接收端也可以逐个字节地将它们读出。
			- UDP协议(User Datagram Protocol,用户数据报协议)则与TCP协议完全相反，它为
			  应用层提供不可靠、无连接和基于数据报的服务。“不可靠”意味着UDP协议无法保证数据
			  从发送端正确地传送到目的端。如果数据在中途丢失，或者目的端通过数据校验发现数据错
			  误而将其丢弃，则UDP协议只是简单地通知应用程序发送失败。因此，使用UDP协议的应
			  用程序通常要自己处理数据确认、超时重传等逻辑。UDP协议是无连接的，即通信双方不
			  保持一个长久的联系，因此应用程序每次发送数据都要明确指定接收端的地址(IP地址等信
			  息)。基于数据报的服务，是相对基于流的服务而言的。每个UDP数据报都有一个长度，接
			  收端必须以该长度为最小单位将其所有内容- -次性读出，否则数据将被截断。
			- SCTP协议( Stream Control Transmission Protocol,流控制传输协议)是一种相对较新的
			  传输层协议，它是为了在因特网上传输电话信号而设计的。本书不讨论SCTP协议，感兴趣
			  的读者可参考其标准文档RFC 2960。
		- ##### [[应用层]]
			- 应用层负责处理应用程序的逻辑。数据链路层、网络层和传输层负责处理网络通信细
			  节，这部分必须既稳定又高效，因此它们都在内核空间中实现，如图1-1所示。而应用层则
			  在用户空间实现，因为它负责处理众多逻辑，比如文件传输、名称查询和网络管理等。如果
			  应用层也在内核中实现，则会使内核变得非常庞大。当然，也有少数服务器程序是在内核中.
			  实现的，这样代码就无须在用户空间和内核空间来回切换(主要是数据的复制)，极大地提
			  高了工作效率。不过这种代码实现起来较复杂，不够灵活，且不便于移植。本书只讨论用户
			  空间的网络编程。
			- 应用层协议很多，图1-1仅列举了其中的几个:
				- ping是应用程序，而不是协议，前面说过它利用ICMP报文检测网络连接，是调试网络
				  环境的必备工具。
				- telnet协议是一种远 程登录协议，它使我们能在本地完成远程任务，本书后续章节将会
				  多次使用telnet客户端登录到其他服务上。
				- OSPF (Open Shortest Path First,开放最短路径优先)协议是- -种动态路由更新协议，用
				  于路由器之间的通信，以告知对方各自的路由信息。
				- DNS ( Domain Name Service,域名服务)协议提供机器域名到IP地址的转换，我们将
				  在后面简要介绍DNS协议。
				  应用层协议(或程序)可能跳过传输层直接使用网络层提供的服务，比如ping程序和OSPF协议。应用层协议(或程序)通常既可以使用TCP服务，又可以使用UDP服务，比
				  如DNS协议。我们可以通过/etc/services文件查看所有知名的应用层协议，以及它们都能使用哪些传输层服务。
	- #### 封装
	  collapsed:: true
		- 上层协议是如何使用下层协议提供的服务的呢?其实这是通过封装(encapsulation) 实
		  现的。应用程序数据在发送到物理网络上之前，将沿着协议栈从上往下依次传递。每层协议
		  都将在上层数据的基础上加上自己的头部信息(有时还包括尾部信息)，以实现该层的功能，
		  这个过程就称为封装，如图1-4所示。
		- ![image.png](../assets/image_1658849573872_0.png)
		- 经过TCP封装后的数据称为TCP报文段(TCP message segment),或者简称TCP段。
		  前文提到，TCP协议为通信双方维持-一个连接，并且在内核中存储相关数据。这部分数据中
		  的TCP头部信息和TCP内核缓冲区(发送缓冲区或接收缓冲区)数据- -起构成了TCP报文
		  段，如图1-5中的虚线框所示。
		- ![image.png](../assets/image_1658849609995_0.png)
		- 当发送端应用程序使用send (或者write)兩数向-一个TCP连接写人数据时，内核中的
		  TCP模块首先把这些数据复制到与该连接对应的TCP内核发送缓冲区中，然后TCP模块调
		  用IP模块提供的服务，传递的参数包括TCP头部信息和TCP发送缓冲区中的数据，即TCP
		  报文段。关于TCP报文段头部的细节，我们将在第3章讨论。
		  经过UDP封装后的数据称为UDP数据报(UDP datagram)。UDP对应用程序数据的封
		  装与TCP类似。不同的是，UDP无须为应用层数据保存副本，因为它提供的服务是不可靠
		  的。当一个UDP数据报被成功发送之后，UDP内核缓冲区中的该数据报就被丢弃了。如果
		  应用程序检测到该数据报未能被接收端正确接收，并打算重发这个数据报，则应用程序需要
		  重新从用户空间将该数据报拷贝到UDP内核发送缓冲区中。
		  经过IP封装后的数据称为IP数据报(IP datagram)。IP 数据报也包括头部信息和数据部
		  分，其中数据部分就是一个TCP报文段、UDP数据报或者ICMP报文。我们将在第2章详
		  细讨论IP数据报的头部信息。
		  经过数据链路层封装的数据称为帧(frame)。传输媒介不同，帧的类型也不同。比如，
		  以太网上传输的是以太网帧(ethernet frame)，而令牌环网络上传输的则是令牌环帧(token
		  ring frame)。以以太网帧为例，其封装格式如图1-6所示。
		  以太网帧使用6字节的目的物理地址和6字节的源物理地址来表示通信的双方。关
		  于类型(type)字段，我们将在后面讨论。4字节CRC字段对帧的其他部分提供循环冗
		  余校验。
		  帧的最大传输单元(Max Transmit Unit, MTU)，即帧最多能搒带多少上层协议数据(比
		  如IP数据报)，通常受到网络类型的限制。图1-6所示的以太网帧的MTU是1500字节。正
		  因为如此，过长的IP数据报可能需要被分片(fragment) 传输。
		- ![image.png](../assets/image_1658849754702_0.png)
		- 帧才是最终在物理网络上传送的字节序列。至此，封装过程完成。
		-
		-
	- #### 分用
	  collapsed:: true
		- 当帧到达目的主机时，将沿着协议栈自底向上依次传递。各层协议依次处理帧中本层负
		  责的头部数据，以获取所需的信息，并最终将处理后的帧交给目标应用程序。这个过程称为
		  分用(demultiplexing)。 分用是依靠头部信息中的类型字段实现的。标准文档RFC 1700定义
		  了所有标识上层协议的类型字段以及每个上层协议对应的数值。图1-7显示了以太网帧的分
		  用过程。
		- ![image.png](../assets/image_1658850491811_0.png)
		- 因为IP协议、ARP协议和RARP协议都使用帧传输数据，所以帧的头部需要提供某个
		  字段(具体情况取决于帧的类型)来区分它们。以以太网帧为例，它使用2字节的类型字段
		  来标识上层协议(见图1-6)。如果主机接收到的以太网帧类型字段的值为0x800，则帧的数
		  据部分为IP数据报(见图1-4)，以太网驱动程序就将帧交付给IP模块;若类型字段的值为
		  0x806，则帧的数据部分为ARP请求或应答报文，以太网驱动程序就将帧交付给ARP模块;
		  若类型字段的值为0x835，则帧的数据部分为RARP请求或应答报文，以太网驱动程序就将
		  帧交付给RARP模块。
		- 同样，因为ICMP协议、TCP协议和UDP协议都使用IP协议，所以IP数据报的头部采
		  用16位的协议(protocol) 字段来区分它们。
		  TCP报文段和UDP数据报则通过其头部中的16位的端口号(portnumber)字段来
		  区分上层应用程序。比如DNS协议对应的端口号是53, HTTP协议( Hyper-Text Transfer :
		  Protocol,超文本传送协议)对应的端口号是80。所有知名应用层协议使用的端口号都可在
		  /etc/services文件中找到。
		- 帧通过上述分用步骤后，最终将封装前的原始数据送至目标服务(图1-7中的ARP服，
		  务、RARP服务、ICMP 服务或者应用程序)。这样，在顶层目标服务看来，封装和分用似乎
		  没有发生过。
	- #### [[ARP]]协议工作原理
	  collapsed:: true
		- ARP协议能实现任意网络层地址到任意物理地址的转换，不过本书仅讨论从IP地址到以太网地址（MAC地址）的转换。其工作原理是:主机向自己所在的网络广播一个ARP请求，该请求包含目标机器的网络地址。此网络上的其他机器都将收到这个请求，但只有被请求的目标机器会回应一个ARP应答，其中包含自己的物理地址。
		- ##### 以太网ARP请求 / 应答报文详解
			- 以太网ARP请求/应答报文的格式：
			- ![image.png](../assets/image_1658886923731_0.png)
			- 以太网ARP请求/应答报文各字段具体介绍如下。
				- 硬件类型字段定义物理地址的类型，它的值为1表示MAC地址。
				- 协议类型字段表示要映射的协议地址类型，它的值为0x800，表示IP地址。
				- 硬件地址长度字段和协议地址长度字段，顾名思义，其单位是字节。对MAC地址来说，其长度为6;对IP (v4）地址来说，其长度为4。
				- 操作字段指出4种操作类型:ARP请求（值为1)、ARP应答（值为2)、RARP请求(值为3）和RARP应答（值为4)。
				- 最后4个字段指定通信双方的以太网地址和IP地址。发送端填充除目的端以太网地址外的其他3个字段，以构建ARP请求并发送之。接收端发现该请求的目的端IP地址是自己，就把自己的以太网地址填进去，然后交换两个目的端地址和两个发送端地址，以构建ARP应答并返回之（当然，如前所述，操作字段需要设置为2)。
			- 由图1-9可知，ARP请求/应答报文的长度为28字节。如果再加上以太网帧头部和尾部的18字节（见图1-6)，则一个携带ARP请求/应答报文的以太网帧长度为46字节。不过有的实现要求以太网帧数据部分长度至少为46字节（见图1-4)，此时ARP请求/应答报文将增加一些填充字节，以满足这个要求。在这种情况下，一个携带ARP请求/应答报文的以太网帧长度为64字节。
		-
		-
	- #### [[DNS]]工作原理
	  collapsed:: true
		- 我们通常使用机器的域名来访问这台机器，而不直接使用其IP地址，比如访问因特网上的各种网站。那么如何将机器的域名转换成IP地址呢?这就需要使用域名查询服务。域名查询服务有很多种实现方式，比如NIS (Network Information Service，网络信息服务)、DNS和本地静态文件等。
		-
		- ##### DNS查询和应答报文详解
			- DNS是一套分布式的域名服务系统。每个DNS服务器上都存放着大量的机器名和IP地址的映射，并且是动态更新的。众多网络客户端程序都使用DNS协议来向DNS服务器查询目标主机的IP地址。
			- ![image.png](../assets/image_1658888935279_0.png)
			- 16位标识字段用于标记一对DNS查询和应答，以此区分一个DNS应答是哪个DNS查询的回应。
			- 16位标志字段用于协商具体的通信方式和反馈通信状态。DNS报文头部的16位标志字段的细节如图1-12所示。
			- ![image.png](../assets/image_1658888992134_0.png)
			- 图1-12中各标志的含义分别是:
				- QR，查询/应答标志。0表示这是一个查询报文，1表示这是一个应答报文。
				- opcode，定义查询和应答的类型。0表示标准查询，1表示反向查询（由IP地址获得
				  主机域名)，2表示请求服务器状态。
				- AA，授权应答标志，仅由应答报文使用。1表示域名服务器是授权服务器。
				- TC，截断标志，仅当DNS报文使用UDP服务时使用。因为UDP数据报有长度限制，所以过长的 DNS报文将被截断。1表示 DNS报文超过512字节，并被截断。
				- RD，递归查询标志。1表示执行递归查询，即如果目标DNS服务器无法解析某个主机名，则它将向其他DNS服务器继续查询，如此递归，直到获得结果并把该结果返回给客户端。0表示执行迭代查询，即如果目标DNS服务器无法解析某个主机名,则它将自己知道的其他DNS服务器的IP地址返回给客户端，以供客户端参考。
				- RA，允许递归标志。仅由应答报文使用，1表示DNS服务器支持递归查询。
				- zero，这3位未用，必须都设置为0。
				- rcode，4位返回码，表示应答的状态。常用值有0（无错误）和3(域名不存在)。
				- 接下来的4个字段则分别指出DNS报文的最后4个字段的资源记录数目。对查询报文而言，它一般包含1个查询问题，而应答资源记录数、授权资源记录数和额外资源记录数则为0。应答报文的应答资源记录数则至少为1，而授权资源记录数和额外资源记录数可为0或非0。
			- 查询问题的格式如下：
				- ![image.png](../assets/image_1658889247982_0.png)
				- 图1-13中，查询名以一定的格式封装了要查询的主机域名。16位查询类型表示如何执行查询操作，常见的类型有如下几种:
					- 类型A，值是1，表示获取目标主机的P地址。
					- 类型CNAME，值是5，表示获得目标主机的别名。
					- 类型PTR，值是12，表示反向查询。
					- 16位查询类通常为1，表示获取因特网地址（IP地址)。
			- 应答字段、授权字段和额外信息字段都使用资源记录(Resource Record，RR）格式。资源记录格式如图1-14所示。
				- ![image.png](../assets/image_1658889324910_0.png)
				- 图1-14中，32位域名是该记录中与资源对应的名字，其格式和查询问题中的查询名字段相同。16位类型和16位类字段的含义也与DNS查询问题的对应字段相同。
				- 32位生存时间表示该查询记录结果可被本地客户端程序缓存多长时间，单位是秒。
				- 16位资源数据长度字段和资源数据字段的内容取决于类型字段。对类型A而言，资源数据是32位的IPv4地址，而资源数据长度则为4(以字节为单位)。
				- 至此，我们简要地介绍了DNS协议。我们将在后面给出一个DNS通信的具体例子。DNS协议的更多细节请参考其RFC文档(DNS协议存在诸多RFC文档，每个RFC文档介绍其一个侧面，比如RFC 1035介绍的是域名的实现和规范，RFC 1886则描述DNS协议对IPv6的扩展支持)。
- IP协议
	- #### IP服务的特点
	  collapsed:: true
		- IP协议是TCP/IP协议族的动力，它为上层协议提供无状态、无连接、不可靠的服务。无状态〈stateless）是指IⅣ通信双方不同步传输数据的状态信息，因此所有IP数据报的发送、传输和接收都是相互独立、没有上下文关系的。这种服务最大的缺点是无法处理乱序和重复的IP数据报。比如发送端发送出的第N个IP数据报可能比第N+1个IP数据报后到达接收端，而同一个IP数据报也可能经过不同的路径多次到达接收端。在这两种情况下，接收端的IP模块无法检测到乱序和重复，因为这些数据报之间没有任何上下文关系。接收端的IP模块只要收到了完整的IP数据报（如果是IP分片的话，IP模块将先执行重组)，就将其数据部分（TCP报文段、UDP数据报或者ICMP报文）上交给上层协议。那么从上层协议来看，这些数据就可能是乱序的、重复的。面向连接的协议，比如TCP协议，则能够自己处理乱序的、重复的报文段，它递交给上层协议的内容绝对是有序的、正确的。
		- 虽然IP数据报头部提供了一个标识字段（见后文）用以唯一标识一个P数据报，但它是被用来处理IP分片和重组的，而不是用来指示接收顺序的。
		  无状态服务的优点也很明显﹔简单、高效。我们无须为保持通信的状态而分配一些内核资源，也无须每次传输数据时都携带状态信息。在网络协议中，无状态是很常见的，比如UDP协议和HTTP协议都是无状态协议。以HTTP协议为例，一个浏览器的连续两次网页请求之间没有任何关联，它们将被Web 服务器独立地处理。
		- 无连接(connectionless）是指IP通信双方都不长久地维持对方的任何信息。这样，上层协议每次发送数据的时候，都必须明确指定对方的IP地址。
		- 不可靠是指P协议不能保证IP数据报准确地到达接收端，它只是承诺尽最大努力( best effort)。很多种情况都能导致IP数据报发送失败。比如，某个中转路由器发现IP数据报在网络上存活的时间太长（根据IP数据报头部字段TTL判断，见后文)，那么它将丢弃之，并返回一个ICMP错误消息（超时错误）给发送端。又比如，接收端发现收到的IP数据报不正确（通过校验机制)，它也将丢弃之，并返回一个ICMP错误消息（IP头部参数错误)给发送端。无论哪种情况，发送端的IP模块一旦检测到IP数据报发送失败，就通知上层协议发送失败，而不会试图重传。因此，使用IP服务的上层协议（比如TCP协议）需要自己实现数据确认、超时重传等机制以达到可靠传输的目的。
		-
	- #### IPv4头部结构
	  collapsed:: true
		- IPv4的头部结构如下。其长度通常为20字节，除非含有可变长的选项部分。
		- ![image.png](../assets/image_1658892143366_0.png)
		- 4位版本号( version）指定P协议的版本。对IPv4来说，其值是4。其他IPv4协议的扩展版本（如SIP协议和PIP协议)，则具有不同的版本号（它们的头部结构也和图2-1不同)。
		- 4位头部长度(header length）标识该IP头部有多少个32 bit字（(4字节)。因为4位最大能表示15，所以IP头部最长是60字节。
		- 8位服务类型（Type Of Service，TOS）包括一个3位的优先权字段（现在已经被忽略),4位的TOS字段和1位保留字段（必须置0)。4位的TOS字段分别表示:最小延时，最大吞吐量，最高可靠性和最小费用。其中最多有一个能置为1，应用程序应该根据实际需要来设置它。比如像ssh和 telnet这样的登录程序需要的是最小延时的服务，而文件传输程序 ftp则需要最大吞吐量的服务。
		- 16位总长度( total length）是指整个IP数据报的长度，以字节为单位，因此IP数据报的最大长度为65535(2^16-1)字节。但由于MTU的限制，长度超过MTU的数据报都将被分片传输，所以实际传输的IP数据报（或分片）的长度都远远没有达到最大值。接下来的3个字段则描述了如何实现分片。
		- 16位标识（identification）唯一地标识主机发送的每一个数据报。其初始值由系统随机生成﹔每发送一个数据报，其值就加1。该值在数据报分片时被复制到每个分片中，因此同一个数据报的所有分片都具有相同的标识值。
		  3位标志字段的第一位保留。第二位（Don't Fragment，DF）表示“禁止分片”。如果设置了这个位，IP模块将不对数据报进行分片。在这种情况下，如果IP数据报长度超过MTU的话，IP模块将丢弃该数据报并返回一个ICMP差错报文。第三位(More Fragment，MF)表示“更多分片”。除了数据报的最后一个分片外，其他分片都要把它置1。
		- 13位分片偏移(fragmentation offset）是分片相对原始IP数据报开始处（仅指数据部分）的偏移。实际的偏移值是该值左移3位（乘8）后得到的。由于这个原因，除了最后一个IP分片外，每个IP分片的数据部分的长度必须是8的整数倍（这样才能保证后面的IP分片拥有一个合适的偏移值)。
		- 8位生存时间〈Time To Live，TTL)是数据报到达目的地之前允许经过的路由器跳数。TTL值被发送端设置（常见的值是64)。数据报在转发过程中每经过一个路由，该值就被路由器减1。当TTL值减为0时，路由器将丢弃数据报，并向源端发送一个ICMP差错报文。TTL值可以防止数据报陷入路由循环。
		- 8位协议（ protocol）用来区分上层协议，我们在第1章讨论过。letc/protocols文件定义了所有上层协议对应的protocol字段的数值。其中，ICMP是1，TCP是6，UDP是17。letclprotocols文件是RFC 1700的一个子集。
		- 16位头部校验和（header checksum）由发送端填充，接收端对其使用CRC算法以检验IP数据报头部（注意，仅检验头部）在传输过程中是否损坏。S
		- 32位的源端IP地址和目的端IP地址用来标识数据报的发送端和接收端。一般情况下，这两个地址在整个数据报的传递过程中保持不变，而不论它中间经过多少个中转路由器。关于这一点，我们将在第4章进一步讨论。
		- IPv4最后一个选项字段( option）是可变长的可选信息。这部分最多包含40字节，因为IP头部最长是60字节(其中还包含前面讨论的20字节的固定部分)。可用的IP选项包括:
			- 记录路由（record route)，告诉数据报途经的所有路由器都将自己的IP地址填人IP头部的选项部分，这样我们就可以跟踪数据报的传递路径。
			- 时间截（timestamp)，告诉每个路由器都将数据报被转发的时间（或时间与IP地址对〉填人IP头部的选项部分，这样就可以测量途经路由之间数据报传输的时间。
			- 松散源路由选择（loose source routing)，指定一个路由器IP地址列表，数据报发送过程中必须经过其中所有的路由器。
			- 严格源路由选择(strict source routing)，和松散源路由选择类似，不过数据报只能经过被指定的路由器。
	- #### IP分片
	  collapsed:: true
		- 当IP数据报的长度超过帧的MTU时，它将被分片传输。分片可能发生在发送端，也可能发生在中转路由器上，而且可能在传输过程中被多次分片，但只有在最终的目标机器上，这些分片才会被内核中的IP模块重新组装。
		  IP头部中的如下三个字段给IP的分片和重组提供了足够的信息﹔数据报标识、标志和片偏移。一个IP数据报的每个分片都具有自己的IP头部，它们具有相同的标识值，但具有不同的片偏移。并且除了最后一个分片外，其他分片都将设置MF标志。此外，每个分片的IP头部的总长度字段将被设置为该分片的长度。
		- 以太网帧的MTU是1500字节（可以通过ifconfig命令或者netstat命令查看)，因此它携带的IP数据报的数据部分最多是1480字节(IP头部占用20字节)。考虑用IP数据报封装一个长度为1481字节的ICMP报文（包括8字节的ICMP头部，所以其数据部分长度为1473字节)，则该数据报在使用以太网帧传输时必须被分片。
		- 长度为1501字节的IP数据报被拆分成两个IP分片，第一个P分片长度为1500字节，第二个IP分片的长度为21字节。每个IP分片都包含自己的I头部（20字节),且第一个IP分片的IP头部设置了MF标志，而第二个IP分片的IP头部则没有设置该标志，因为它已经是最后一个分片了。原始P数据报中的ICMP头部内容被完整地复制到了第一个IP分片中。第二个IP分片不包含ICMP头部信息，因为IP模块重组该ICMP报文的时候只需要一份ICMP头部信息，重复传送这个信息没有任何益处。1473字节的ICMP报文数据的前1472字节被IP模块复制到第一个IP分片中，使其总长度为1500字节，从而满足MTU的要求;而多出的最后1字节则被复制到第二个IP分片中。
		- ![image.png](../assets/image_1658892837784_0.png)
		- 需要指出的是，ICMP报文的头部长度取决于报文的类型，其变化范围很大。图2-2以8字节为例，因为后面的例子用到了ping程序，而ping程序使用的ICMP回显和应答报文的头部长度是8字节。
	- #### IP路由
	  collapsed:: true
		- IP协议的一个核心任务是数据报的路由，即决定发送数据报到目标机器的路径。为了理解IP路由过程，我们先简要分析IP模块的基本工作流程。
		- ##### IP模块工作流程
		  collapsed:: true
			- ![image.png](../assets/image_1658902875975_0.png)
			- 我们从右往左来分析图2-3。当IP模块接收到来自数据链路层的IP数据报时，它首先对该数据报的头部做CRC校验，确认无误之后就分析其头部的具体信息。
			  如果该IP数据报的头部设置了源站选路选项（松散源路由选择或严格源路由选择)，则IP模块调用数据报转发子模块来处理该数据报。如果该P数据报的头部中目标P地址是本机的某个IP地址，或者是广播地址，即该数据报是发送给本机的，则IP模块就根据数据报头部中的协议字段来决定将它派发给哪个上层应用（分用)。如果IP模块发现这个数据报不是发送给本机的，则也调用数据报转发子模块来处理该数据报。
			  数据报转发子模块将首先检测系统是否允许转发，如果不允许,IP模块就将数据报丢弃。如果允许，数据报转发子模块将对该数据报执行一些操作，然后将它交给IP数据报输出子模块。我们将在后面讨论数据报转发的具体过程。
			  IP数据报应该发送至哪个下一跳路由（或者目标机器)，以及经过哪个网卡来发送，就是P路由过程，即图2-3中“计算下一跳路由”子模块。IP模块实现数据报路由的核心数据结构是路由表。这个表按照数据报的目标IP地址分类，同一类型的IP数据报将被发往相同的下一跳路由器（或者目标机器)。我们将在后面讨论P路由过程。
			  IP输出队列中存放的是所有等待发送的IP数据报，其中除了需要转发的IP数据报外，还包括封装了本机上层数据（ICMP报文、TCP报文段和UDP数据报）的IP数据报。
			  图2-3中的虚线箭头显示了路由表更新的过程。这一过程是指通过路由协议或者route命令调整路由表，使之更适应最新的网络拓扑结构，称为IP路由策略。
			-
		-
	- #### IP转发
	  collapsed:: true
		- 前文提到，不是发送给本机的I数据报将由数据报转发子模块来处理。路由器都能执行数据报的转发操作，而主机一般只发送和接收数据报，这是因为主机上/proc/sys/net/ipv4/ip_forward 内核参数默认被设置为0。我们可以通过修改它来使能主机的数据报转发功能(在测试机器Kongming20上以root身份执行):
		  ![image.png](../assets/image_1658903740058_0.png)
		- 对于允许IP数据报转发的系统（主机或路由器)，数据报转发子模块将对期望转发的数据报执行如下操作:
			- 1) 检查数据报头部的TTL值。如果TTL值已经是0，则丢弃该数据报。
			- 2) 查看数据报头部的严格源路由选择选项。如果该选项被设置，则检测数据报的目标IP地址是否是本机的某个IP地址。如果不是，则发送一个ICMP源站选路失败报文给发送端。
			- 3) 如果有必要，则给源端发送一个ICMP重定向报文，以告诉它一个更合理的下一跳路由器。
			- 4) 将TTL值减1。
			- 5) 处理IP头部选项。
			- 6) 如果有必要，则执行IP分片操作。
	- #### 重定向
	  collapsed:: true
		- ![image.png](../assets/image_1658906093635_0.png)
		- 我们在1.1节讨论过ICMP报文头部的3个固定字段:8位类型、8位代码和16位校验和。ICMP重定向报文的类型值是5，代码字段有4个可选值，用来区分不同的重定向类型。本书仅讨论主机重定向，其代码值为1。
		  ICMP重定向报文的数据部分含义很明确，它给接收方提供了如下两个信息:引起重定向的IP数据报（即图2-4中的原始IP数据报）的源端IP地址。应该使用的路由器的IP地址。
		  接收主机根据这两个信息就可以断定引起重定向的IP数据报应该使用哪个路由器来转发，并且以此来更新路由表（通常是更新路由表缓冲，而不是直接更改路由表)。
		  /proc/sys/net/ipv4/conf/all/send_redirects内核参数指定是否允许发送ICMP重定向报文，而/proc/sys/net/ipv4/conf/all/accept_redirects 内核参数则指定是否允许接收ICMP重定向报文。一般来说，主机只能接收ICMP重定向报文，而路由器只能发送ICMP重定向报文。
	- #### IPv6头部结构
	  collapsed:: true
		- IPv6固定头部结构
		  collapsed:: true
			- IPv6头部由40字节的固定头部和可变长的扩展头部组成。图2-6所示是IPv6的固定头
			  部结构。
			- ![image.png](../assets/image_1658931685982_0.png)
			- 4位版本号(version) 指定IP协议的版本。对IPv6来说，其值是6。
			- 8位通信类型(raffic class)指示数据流通信类型或优先级，和IPv4中的Tos类似。
			- 20位流标签(flow label)是IPv6新增加的字段，用于某些对连接的服务质量有特殊要
			  求的通信，比如音频或视频等实时数据传输。
			- 16位净荷长度(payload length) 指的是IPv6扩展头部和应用程序数据长度之和，不包
			  括固定头部长度。
			- 8位下一一个包头(next header)指出紧跟IPv6固定头部后的包头类型，如扩展头(如果
			  有的话)或某个上层协议头( 比如TCP，UDP或ICMP)。它类似于IPv4头部中的协议字段，
			  且相同的取值有相同的含义。
			- 8位跳数限制(hop limit)和IPv4中的TTL含义相同。
			- IPv6用128位(16字节)来表示IP地址，使得IP地址的总量达到了2128个。所以有人
			  说，“IPv6使得地球上的每粒沙子都有---个IP地址”。
			- 32位表示的IPv4地址一般用点分十进制来表示，而IPv6地址则用十六进制字符串
			  表示，比如“E0000000000:124:57800000012”。可见，IPv6地 址用“:”分
			  割成8组，每组包含2字节。但这种表示方法过于麻烦，通常可以使用所谓的零压缩法
			  来将其简写，也就是省略连续的、全零的组。比如，上面的例子使用零压缩法可表示为
			  “FE80::1234:5678.000:0012”。不过零压缩法对一一个IPv6地址只能使用- -次，比如上面的例
			  子中，字节组“5678"后面的全零组就不能再省略，否则我们就无法计算每个“::” 之间省
			  略了多少个全零组。
		- IPv6扩展头部
		  collapsed:: true
			- 可变长的扩展头部使得IPv6能支持更多的选项，并且很便于将来的扩展需要。它的长
			  度可以是0,表示数据报没使用任何扩展头部。-一个数据报可以包含多个扩展头部，每个扩
			  展头部的类型由前一个头部(固定头部或扩展头部)中的下一个报头字段指定。目前可以使
			  用的扩展头部如表2-3所示。
			- ![image.png](../assets/image_1658931744969_0.png)
			-
- TCP协议
  collapsed:: true
	- #### TCP服务的特点
	  collapsed:: true
		- 传输层协议主要有两个: TCP协议和UDP协议。TCP 协议相对于UDP协议的特点是:
		  面向连接、字节流和可靠传输。,
		- 使用TCP协议通信的双方必须先建立连接，然后才能开始数据的读写。双方都必须为
		  该连接分配必要的内核资源，以管理连接的状态和连接上数据的传输。TCP连接是全双工的，即双方的数据读写可以通过-一个连接进行。完成数据交换之后，通信双方都必须断开连接以释放系统资源。
		- TCP协议的这种连接是- -对- -的，所以基于广播和多播( 目标是多个主机地址)的应用
		  程序不能使用TCP服务。而无连接协议UDP则非常适合于广播和多播。
		  我们在1.1节中简单介绍过字节流服务和数据报服务的区别。这种区别对应到实际编程
		  中，则体现为通信双方是否必须执行相同次数的读、写操作(当然，这只是表现形式)。当
		  发送端应用程序连续执行多次写操作时，TCP 模块先将这些数据放入TCP发送缓冲区中。当
		  TCP模块真正开始发送数据时，发送缓冲区中这些等待发送的数据可能被封装成- -个或多个TCP报文段发出。因此，TCP模块发送出的TCP报文段的个数和应用程序执行的写操作次
		  数之间没有固定的数量关系。
		- 当接收端收到一个或多个TCP报文段后，TCP模块将它们携带的应用程序数据按照
		  TCP报文段的序号(见后文)依次放人TCP接收缓冲区中，并通知应用程序读取数据。接.
		  收端应用程序可以一次性将TCP接收缓冲区中的数据全部读出，也可以分多次读取，这取决
		  于用户指定的应用程序读缓冲区的大小。因此，应用程序执行的读操作次数和TCP模块接收
		  到的TCP报文段个数之间也没有固定的数量关系。
		- 综上所述，发送端执行的写操作次数和接收端执行的读操作次数之间没有任何数量关
		  系，这就是字节流的概念:应用程序对数据的发送和接收是没有边界限制的。UDP 则不然。
		  发送端应用程序每执行一次写操作，UDP模块就将其封装成-一个 UDP数据报并发送之。接
		  收端必须及时针对每-一个UDP数据报执行读操作(通过recvfrom系统调用)，否则就会丢包
		  (这经常发生在较慢的服务器上)。并且，如果用户没有指定足够的应用程序缓冲区来读取
		  UDP数据，则UDP数据将被截断。
		- ![image.png](../assets/image_1658931859556_0.png)
		- TCP传输是可靠的。首先，TCP协议采用发送应答机制，即发送端发送的每个TCP报
		  文段都必须得到接收方的应答，才认为这个TCP报文段传输成功。其次，TCP协议采用超时重传机制，发送端在发送出一个TCP报文段之后启动定时器，如果在定时时间内未收到应
		  答，它将重发该报文段。最后，因为TCP报文段最终是以IP数据报发送的，而IP数据报到
		  达接收端可能乱序、重复，所以TCP协议还会对接收到的TCP报文段重排、整理，再交付
		  给应用层。
		- UDP协议则和IP协议- -样，提供不可靠服务。它们都需要上层协议来处理数据确认和
		  超时重传。
	- #### TCP头部结构
	  collapsed:: true
		- TCP固定头部结构
		  collapsed:: true
			- TCP头部结构如图3-3所示，其中的诸多字段为管理TCP连接和控制数据流提供了足够
			  的信息。
			- ![image.png](../assets/image_1658931942365_0.png)
			- 16位端口号(port number):告知主机该报文段是来自哪里(源端口)以及传给哪个上
			  层协议或应用程序(目的端口)的。进行TCP通信时，客户端通常使用系统自动选择的临时
			  端口号，而服务器则使用知名服务端口号。1.3 节中提到过，所有知名服务使用的端口号都
			  定义在/etc/services文件中。
			- 32位序号(sequence number): - -次TCP通信(从TCP连接建立到断开)过程中某
			  一个传输方向上的字节流的每个字节的编号。假设主机A和主机B进行TCP通信，A发
			  送给B的第一个TCP报文段中，序号值被系统初始化为某个随机值ISN ( Initial Sequence
			  Number,初始序号值)。 那么在该传输方向上(从A到B)，后续的TCP报文段中序号值
			  将被系统设置成ISN加上该报文段所携带数据的第一个字节在整个字节流中的偏移。例如，某个TCP报文段传送的数据是字节流中的第1025 ~ 2048字节，那么该报文段的序
			  号值就是ISN+1025.另外一个传输方向(从B到A)的TCP报文段的序号值也具有相同
			  的含义。.
			- 32位确认号(acknowledgement number):用作对另-方发送来的TCP报文段的响应。
			  其值是收到的TCP报文段的序号值加1.假设主机A和主机B进行TCP通信，那么A发送.
			  出的TCP报文段不仅携带自己的序号，而且包含对B发送来的TCP报文段的确认号。反之，
			  B发送出的TCP报文段也同时携带自己的序号和对A发送来的报文段的确认号。
			- 4位头部长度(header length):标识该TCP头部有多少个32bit字(4 字节)。因为4位
			  最大能表示15，所以TCP头部最长是60字节。
			- 6位标志位包含如下几项:
				- URG标志，表示紧急指针(urgent pointer)是否有效。
				- ACK标志，表示确认号是否有效。我们称携带ACK标志的TCP报文段为确认报文段。
				- PSH标志，提示接收端应用程序应该立即从TCP接收缓冲区中读走数据，为接收后
				  续数据腾出空间(如果应用程序不将接收到的数据读走，它们就会--直停留在TCP
				  接收缓冲区中)。
				- RST标志，表示要求对方重新建立连接。我们称携带RST标志的TCP报文段为复位
				  报文段。
				- SYN标志，表示请求建立一个连接。我们称携带SYN标志的TCP报文段为同步报
				  文段。
				- FIN标志，表示通知对方本端要关闭连接了。我们称携带FIN标志的TCP报文段为
				  结束报文段。
			- 16位窗口大小( window size):是TCP流量控制的-一个手段。这里说的窗口，指的是接
			  收通告窗口(Receiver Window, RWND)。它告诉对方本端的TCP接收缓冲区还能容纳多少
			  字节的数据，这样对方就可以控制发送数据的速度。
			- 16位校验和(TCP checksum):由发送端填充，接收端对TCP报文段执行CRC算法以
			  检验TCP报文段在传输过程中是否损坏。注意，这个校验不仅包括TCP头部，也包括数据
			  部分。这也是TCP可靠传输的-一个重要保障。
			- 16位紧急指针(urgent pointer):是-一个正的偏移量。它和序号字段的值相加表示最后
			  一个紧急数据的下一 -字节的序号。因此，确切地说，这个字段是紧急指针相对当前序号的偏移，不妨称之为紧急偏移。TCP的紧急指针是发送端向接收端发送紧急数据的方法。
				-
		- TCP头部选项
			- TCP头部的最后-一个选项字段(options) 是可变长的可选信息。这部分最多包含40字
			  节，因为TCP头部最长是60字节(其中还包含前面讨论的20字节的固定部分)。典型的
			  TCP头部选项结构如图3-4所示。
			- ![image.png](../assets/image_1658932104853_0.png)
			- 选项的第一个字段kind说明选项的类型。有的TCP选项没有后面两个字段，仅包含1
			  字节的kind字段。第二个字段length ( 如果有的话)指定该选项的总长度，该长度包括kind
			  字段和length字段占据的2字节。第三个字段info (如果有的话)是选项的具体信息。常见
			  的TCP选项有7种，如图3-5所示。
			- ![image.png](../assets/image_1658932122179_0.png)
			- kind=0是选项表结束选项。
			- kind=1是空操作(nop) 选项，没有特殊含义，一般用于将TCP选项的总长度填充为4
			  字节的整数倍。
			- kind=2是最大报文段长度选项。TCP连接初始化时，通信双方使用该选项来协商最大报
			  文段长度(Max Segment Size, MSS)。 TCP模块通常将MSS设置为(MTU-40)字节(减
			  掉的这40字节包括20字节的TCP头部和20字节的IP头部)。这样携带TCP报文段的IP
			  数据报的长度就不会超过MtU (假设TCP头部和IP头部都不包含选项字段，并且这也是一
			  般情况)，从而避免本机发生IP分片。对以太网而言，MSS 值是1460 (1500 -40)字节。
			- kind=3是窗口扩“大因子选项。TCP连接初始化时，通信双方使用该选项来协商接收通告
			  窗口的扩大因子。在TCP的头部中，接收通告窗口大小是用16位表示的，故最大为65 535
			  字节，但实际上TCP模块允许的接收通告窗口大小远不止这个数(为了提高TCP通信的吞
			  吐量)。窗口扩大因子解决了这个问题。假设TCP头部中的接收通告窗口大小是N,窗口扩
			  大因子(移位数)是M,那么TCP报文段的实际接收通告窗口大小是N乘2"，或者说N左
			  移M位。注意，M的取值范围是0 ~ 14。我们可以通过修改/proc/sys/net/ipv4/tcp_ window_
			  scaling内核变量来启用或关闭窗口扩大因子选项。
			- 和MSS选项- -样，窗口扩大因子选项只能出现在同步报文段中，否则将被忽略。但同
			  步报文段本身不执行窗口扩大操作，即同步报文段头部的接收通告窗口大小就是该TCP报文
			  段的实际接收通告窗口大小。当连接建立好之后，每个数据传输方向的窗口扩大因子就固定
			  不变了。关于窗口扩大因子选项的细节，可参考标准文档RFC 1323。
			- kind=4是选择性确认(Selective Acknowledgment, SACK)选项。TCP通信时，如果某
			  个TCP报文段丢失，则TCP模块会重传最后被确认的TCP报文段后续的所有报文段，这样
			  原先已经正确传输的TCP报文段也可能重复发送，从而降低了TCP性能。SACK技术正是
			  为改善这种情况而产生的，它使TCP模块只重新发送丢失的TCP报文段，不用发送所有未
			  被确认的TCP报文段。选择性确认选项用在连接初始化时，表示是否支持SACK技术。我
			  们可以通过修改/proc/sys/net/ipv4/tcp_ sack 内核变量来启用或关闭选择性确认选项。
			- kind=5是SACK实际工作的选项。该选项的参数告诉发送方本端已经收到并缓存的不连
			  续的数据块，从而让发送端可以据此检查并重发丢失的数据块。每个块边沿(edge of block)
			  参数包含一个4字节的序号。其中块左边沿表示不连续块的第一个 数据的序号，而块右边沿
			  则表示不连续块的最后-一个数据的序号的下一个序号。这样--对参数(块左边沿和块右边
			  沿)之间的数据是没有收到的。因为- -个块信息占用8字节，所以TCP头部选项中实际上最
			  多可以包含4个这样的不连续数据块(考虑选项类型和长度占用的2字节)。
			- kind=8是时间戳选项。该选项提供了较为准确的计算通信双方之间的回路时间(Round
			  Trip Time, RTT) 的方法，从而为TCP流量控制提供重要信息。我们可以通过修改/proc/sys/
			  net/ipv4/tcp_ timestamps 内核变量来启用或关闭时间戳选项。
		-
- Linux网络编程基础API
  collapsed:: true
	- #### socket地址API
	  collapsed:: true
		- 主机字节序和网络字节序
		  collapsed:: true
			- 现代CPU的累加器一次都能装载（至少)4字节（这里考虑32位机，下同)，即一个整数。那么这4字节在内存中排列的顺序将影响它被累加器装载成的整数的值。这就是字节序问题。字节序分为大端字节序(big endian）和小端字节序(little endian)。大端字节序是指一个整数的高位字节(23～31 bit)存储在内存的低地址处，低位字节(0～7 bit）存储在内存的高地址处。小端字节序则是指整数的高位字节存储在内存的高地址处，而低位字节则存储在内存的低地址处。代码清单5-1可用于检查机器的字节序。
			- ```c
			  // 判断机器字节序
			  #include <stdio.h>
			  void byteorder() {
			    union 
			    {
			      short value;
			      char union_bytes { sizeof(short) };
			    } test;
			    test.value = 0x0102;
			    if( (test.union_bytes[0] == 1) && (test.union_bytes[1] == 2) )
			    {
			      printf("big endian\n");
			    }
			    else if( (test.union_bytes[0] == 2) && (test.union_bytes[1] == 1) ) 
			    {
			      printf("little endian\n");
			    }
			    else 
			    {
			      printf("unknown...\n");
			    }
			  }
			  ```
			- 现代PC大多采用小端字节序，因此小端字节序又被称为主机字节序。
			- 当格式化的数据（比如32 bit整型数和16 bit短整型数）在两台使用不同字节序的主机之间直接传递时，接收端必然错误地解释之。解决问题的方法是:发送端总是把要发送的数据转化成大端字节序数据后再发送，而接收端知道对方传送过来的数据总是采用大端字节序，所以接收端可以根据自身采用的字节序决定是否对接收到的数据进行转换（小端机转换，大端机不转换)。因此大端字节序也称为网络字节序，它给所有接收数据的主机提供了一个正确解释收到的格式化数据的保证。
			- 需要指出的是，即使是同一台机器上的两个进程（比如一个由C语言编写，另一个由JAVA编写）通信，也要考虑字节序的问题（JAVA虚拟机采用大端字节序)。
			  Linux提供了如下4个函数来完成主机字节序和网络字节序之间的转换:
			  ```c
			  #include <netinet/in.h>
			  unsigned long int htonl(unsigned long int hostlong);
			  unsigned short int htons(unsigned short int hostshort);
			  unsigned long int ntohl(unsigned long int netlong);
			  unsigned short int ntohs(unsigned short int netshort);
			  ```
			- 它们的含义很明确，比如 htonl表示“host to network long"，即将长整型（32 bit）的主机字节序数据转化为网络字节序数据。这4个函数中，长整型函数通常用来转换IP地址，短整型函数用来转换端口号(当然不限于此。任何格式化的数据通过网络传输时，都应该使用这些函数来转换字节序)。
		- 通用socket地址
		  collapsed:: true
			- socket网络编程接口中表示socket地址的是结构体sockaddr，定义如下：
			- ```c
			  #include <bits/socket.h>
			  struct sockaddr
			  {
			    sa_family sa_family;
			    char sa_data[14];
			  }
			  ```
			- sa_family成员是地址族类型(sa_family_t)）的变量。地址族类型通常与协议族类型对应。常见的协议族（protocol family，也称domain，见后文）和对应的地址族如表5-1所示。
			  ![image.png](../assets/image_1658976814502_0.png)
			- 宏PF_*和AF_*都定义在bits/socket.h头文件中，且后者与前者有完全相同的值，所以二者通常混用。
			- sa_data成员用于存放socket地址值。但是，不同的协议族的地址值具有不同的含义和长度，如表5-2所示。
			- ![image.png](../assets/image_1658976841874_0.png)
			- 由表5-2可见，14字节的sa_data根本无法完全容纳多数协议族的地址值。因此，Linux定义了下面这个新的通用socket地址结构体:
			- ```c
			  #include <bits/socket.h>
			  struct sockaddr_storage
			  {
			    sa_family_t sa_family;
			    unsigned long int __ss_align;
			    char __ss_padding[128-sizeof(__ss_align)];
			  }
			  
			  ```
			- 这个结构体不仅提供了足够大的空间用于存放地址值，而且是内存对齐的（这是__ss_align成员的作用)。
		- 专用socket地址
		  collapsed:: true
			- 上面这两个通用socket地址结构体显然很不好用，比如设置与获取IP地址和端口号就需要执行烦琐的位操作。所以Linux为各个协议族提供了专门的socket地址结构体。
			  UNIX本地域协议族使用如下专用socket地址结构体:
			- ```c
			  #include <sys/un.h>
			  struct sockaddr_un
			  {
			    sa_family_t sin_family; 	// 地址族：AF_UNIX
			    char sun_path[108]; 		// 文件路径名
			  }
			  ```
			- TCP/IP协议族有sockaddr_in和 sockaddr_in6两个专用socket地址结构体，它们分别用于IPv4和IPv6:
			- ```c
			  struct sockaddr_in 
			  {
			    sa_family_t sin_family; 	// 地址族：AF_INET
			    u_int16_t sin_port;		// 端口号：要用网络字节序表示
			    struct in_addr sin_addr;	// IPv4地址族结构体，见下面
			  };
			  struct in_addr
			  {
			    u_int32_t s_addr;			// IPv4地址，要用网络字节序表示
			  };
			  
			  struct sockaddr_in6
			  {
			    sa_family_t sin6_family; 	// 地址族：AF_INET6
			    u_int16_t sin6_port;		// 端口号，要用网络字节序表示
			    u_int32_t sin6_flowinfo;	// 流信息，应设置为0
			    struct in6_addr sin6_addr;// IPv6地址结构体，见下面
			    u_int32_t sin6_scope_id;	// scope ID, 尚处于实验阶段
			  };
			  struct in6_addr
			  {
			    unsigned char sa_addr[16]; // IPv6地址，要用网络字节序表示
			  };
			  ```
			- 所有专用socket地址（以及sockaddr_storage）类型的变量在实际使用时都需要转化为通用socket地址类型sockaddr（强制转换即可)，因为所有socket编程接口使用的地址参数的类型都是sockaddr。
		- IP地址转换函数
		  collapsed:: true
			- 通常，人们习惯用可读性好的字符串来表示IP地址，比如用点分十进制字符串表示IPv4地址，以及用十六进制字符串表示IPv6地址。但编程中我们需要先把它们转化为整数(二进制数）方能使用。而记录日志时则相反，我们要把整数表示的IP地址转化为可读的字符串。下面3个函数可用于用点分十进制字符串表示的IPv4地址和用网络字节序整数表示的IPv4地址之间的转换:
			- ```c
			  #include <arpa/inet.h>
			  in_addr_t inet_addr( const char* strptr );
			  int inet_aton( const char* cp, struct in_addr* inp );
			  char* inet_ntoa( struct in_addr in );
			  ```
			- inet_addr函数将用点分十进制字符串表示的IPv4地址转化为用网络字节序整数表示的IPv4地址。它失败时返回INADDR_NONE。
			- inet_aton函数完成和 inet_addr同样的功能，但是将转化结果存储于参数inp指向的地址结构中。它成功时返回1，失败则返回0。
			- inet_ntoa函数将用网络字节序整数表示的IPv4地址转化为用点分十进制字符串表示的IPv4地址。但需要注意的是，该函数内部用一个静态变量存储转化结果，函数的返回值指向该静态内存，因此 inet_ntoa是不可重人的。代码清单5-2揭示了其不可重入性。
			- ![image.png](../assets/image_1658979067760_0.png)
			-
			- 下面这对更新的函数也能完成和前面3个函数同样的功能，并且它们同时适用于IPv4地址和IPv6地址:
			- ```c
			  #include <arpa/inet.h>
			  int inet_pton( int af, const char* src, void* dst );
			  const char* inet_ntop( int af, const void* src, char* dst, socklen_t cut );
			  ```
			- inet_pton函数将用字符串表示的P地址src（用点分十进制字符串表示的IPv4地址或用十六进制字符串表示的IPv6地址）转换成用网络字节序整数表示的IP地址，并把转换结果存储于dst指向的内存中。其中，af参数指定地址族，可以是AF_INET或者AF_INET6.inet_pton成功时返回1，失败则返回О并设置errno。
			- inet_ntop函数进行相反的转换，前三个参数的含义与inet_pton的参数相同，最后一个参数cnt指定目标存储单元的大小。下面的两个宏能帮助我们指定这个大小(分别用于IPv4和IPv6):
			- ```c
			  #include <netinet/in.h>
			  #define INET_ADDRSTRLEN 16
			  #define INET6_ADDRSTRLEN 46
			  ```
			- inet_ntop成功时返回目标存储单元的地址，失败则返回NULL并设置errno。
			-
	- #### 创建[[socket]]
		- UNIX/Linux 的一个哲学是:所有东西都是文件。socket也不例外，它就是可读、可写、可控制、可关闭的文件描述符。下面的socket系统调用可创建一个socket:
		- ```C
		  #include <sys/types.h>
		  #include <sys/socket.h>
		  int socket( int domain, int type, int protocol );
		  ```
		- domain参数告诉系统使用哪个底层协议族。对TCP/IP协议族而言，该参数应该设置为PF_INET (Protocol Family of Internet，用于IPv4）或PF_INET6(用于IPv6);对于UNIX本地域协议族而言，该参数应该设置为PF_UNIX。关于socket系统调用支持的所有协议族，请读者自己参考其man手册。
		- type参数指定服务类型。服务类型主要有SOCK_STREAM服务（流服务）和SOCK_UGRAM（数据报）服务。对TCP/IP协议族而言，其值取SOCK_STREAM表示传输层使用TCP协议，取SOCK_DGRAM表示传输层使用UDP协议。
		- 值得指出的是，自Linux内核版本2.6.17起，type参数可以接受上述服务类型与下面两个重要的标志相与的值:SOCK_NONBLOCK和 SOCK_CLOEXEC。它们分别表示将新创建的socket设为非阻塞的，以及用fork调用创建子进程时在子进程中关闭该socket。在内核版本2.6.17之前的Linux中，文件描述符的这两个属性都需要使用额外的系统调用（比如fcntl)）来设置。
		- protocol参数是在前两个参数构成的协议集合下，再选择一个具体的协议。不过这个值通常都是唯一的（前两个参数已经完全决定了它的值)。几乎在所有情况下，我们都应该把它设置为0，表示使用默认协议。
		- socket系统调用成功时返回一个socket文件描述符，失败则返回-1并设置errno.
	- #### 命名socket
	  collapsed:: true
		- 创建socket时，我们给它指定了地址族，但是并未指定使用该地址族中的哪个具体socket地址。将一个socket与 socket地址绑定称为给socket命名。在服务器程序中，我们通常要命名socket，因为只有命名后客户端才能知道该如何连接它。客户端则通常不需要命名socket，而是采用匿名方式，即使用操作系统自动分配的 socket地址。命名socket的系统调用是bind，其定义如下:
		- ```c
		  #include <sys/types.h>
		  #include <sys/socket.h>
		  int bind( int sockfd, const struct sockaddr* my_addr, socklen_t addrlen );
		  ```
		- bind将my_addr所指的socket地址分配给未命名的sockfd文件描述符，addrlen参数指出该socket地址的长度。
		- bind成功时返回0，失败则返回-1并设置errno。其中两种常见的errno是EACCES和EADDRINUSE，它们的含义分别是:
			- EACCES，被绑定的地址是受保护的地址，仅超级用户能够访问。比如普通用户将
			  socket绑定到知名服务端口（端口号为0~1023）上时，bind将返回EACCES错误。
			- EADDRINUSE，被绑定的地址正在使用中。比如将socket绑定到一个处于TIME_
			  WAIT状态的socket地址。
		-
	- #### 监听socket
	  collapsed:: true
		- socket被命名之后，还不能马上接受客户连接，我们需要使用如下系统调用来创建一个监听队列以存放待处理的客户连接:
		- ```c
		  #include <sys/socket.h>
		  int listen( int sockfd, int backlog );
		  ```
		- sockfd参数指定被监听的socket。backlog参数提示内核监听队列的最大长度。监听队列的长度如果超过backlog，服务器将不受理新的客户连接，客户端也将收到ECONNREFUSED错误信息。在内核版本2.2之前的Linux 中，backlog参数是指所有处于半连接状态(SYN_RCVD）和完全连接状态（ESTABLISHED）的 socket 的上限。但自内核版本2.2之后，它只表示处于完全连接状态的socket的上限，处于半连接状态的socket 的上限则由/proc/sys/net/ipv4/tcp_max_syn_backlog 内核参数定义。backlog参数的典型值是5。
		- listen成功时返回0，失败则返回-1并设置errno。
		-
		-
	- #### 接受连接
	  collapsed:: true
		- 下面的系统调用从listen监听队列中接受一个连接：
		- ```c
		  #include <sys/types.h>
		  #include <sys/socket.h>
		  int accept( int sockfd, struct sockaddr* addr, socklen_t* addrlen );
		  ```
		- sockfd参数是执行过listen系统调用的监听socket。addr参数用来获取被接受连接的远端socket地址，该socket 地址的长度由addrlen参数指出。accept成功时返回一个新的连接socket，该socket唯一地标识了被接受的这个连接，服务器可通过读写该socket来与被接受连接对应的客户端通信。accept失败时返回-1并设置errno.
		-
	- #### 发起连接
	  collapsed:: true
		- 如果说服务器通过listen调用来被动接受连接，那么客户端需要通过如下系统调用来主动与服务器建立连接:
		- ```c
		  #include <sys/types.h>
		  #include <sys/socket.h>
		  int connect( int sockfd, const struct sockaddr *serv_addr, socklen_t addlen );
		  ```
		- sockfd参数由socket系统调用返回一个socket。serv_addr参数是服务器监听的socket地址，addrlen参数则指定这个地址的长度。
		- connect成功时返回0。一旦成功建立连接，sockfd就唯一地标识了这个连接，客户端就可以通过读写sockfd来与服务器通信。connect 失败则返回-1并设置errno。其中两种常见的crrno是 ECONNREFUSED和 ETIMEDOUT，它们的含义如下:
			- ECONNREFUSED，目标端口不存在，连接被拒绝。我们在3.5.1小节讨论过这种
			  情况。
			- ETIMEDOUT，连接超时。我们在3.3.3小节讨论过这种情况。
	- #### 关闭连接
	  collapsed:: true
		- 关闭一个连接实际上就是关闭该连接对应的socket，这可以通过如下关闭普通文件描述符的系统调用来完成:
		- ```c
		  #include <unistd.h>
		  int close( int fd );
		  ```
		- fd参数是待关闭的socket。不过，close系统调用并非总是立即关闭一个连接，而是将fl的引用计数减1。只有当fd的引用计数为0时，才真正关闭连接。多进程程序中，一次fork系统调用默认将使父进程中打开的socket 的引用计数加1，因此我们必须在父进程和子进程中都对该socket执行closc调用才能将连接关闭。
		- 如果无论如何都要立即终止连接（而不是将socket 的引用计数减1)，可以使用如下的shutdown系统调用（相对于close来说，它是专门为网络编程设计的):
		- ```c
		  #include <sys/socket.h>
		  int shutdown( int sockfd, int howto );
		  ```
		- sockfd参数是待关闭的socket。howto参数决定了shutdown 的行为，它可取表5-3中的某个值。
		- ![image.png](../assets/image_1658992331164_0.png)
		- 由此可见，shutdown 能够分别关闭socket上的读或写，或者都关闭。而close在关闭连接时只能将socket 上的读和写同时关闭。
		- shutdown成功时返回0，失败则返回-1并设置errno.
		-
	- #### 数据读写
	  collapsed:: true
		- TCP数据读写
		  collapsed:: true
			- 对文件的读写操作read和 write同样适用于socket。但是 socket编程接口提供了几个专门用于socket数据读写的系统调用，它们增加了对数据读写的控制。其中用于TCP流数据读写的系统调用是:
			- ```c
			  #include <sys/types.h>
			  #include <sys/socket.h>
			  ssize_t recv( int sockfd, void *buf, size_t len, int flags );
			  ssize_t send( int sockfd, const void *buf, size_t len, int flags );
			  ```
			- recv读取sockfd 上的数据，buf和 len参数分别指定读缓冲区的位置和大小，flags参数的含义见后文，通常设置为0即可。recv成功时返回实际读取到的数据的长度，它可能小于我们期望的长度len。因此我们可能要多次调用recv，才能读取到完整的数据。recv可能返回0，这意味着通信对方已经关闭连接了。recv出错时返回-1并设置errno。
			- send往sockfd 上写入数据，buf和 len参数分别指定写缓冲区的位置和大小。send成功时返回实际写入的数据的长度，失败则返回-1并设置errno。
			- flags参数为数据收发提供了额外的控制，它可以取表5-4所示选项中的一个或几个的逻辑或。
			- ![image.png](../assets/image_1658992781948_0.png)
			-
		- UDP数据读写
		  collapsed:: true
			- socket编程接口中用于UDP数据报读写的系统调用是：
			- ```c
			  #include <sys/types.h>
			  #include <sys/socket.h>
			  ssize_t recvfrom( int sockfd, void* buf, size_t len, int flags, 
			                  struct sockaddr *src_addr, socklen_t * addrlen );
			  ssize_t sendto( int sockfd, const void* buf, size_t len, int flags, 
			                const struct sockaddr *dest_addr, socklen_t addrlen );
			  ```
			- recvfrom读取sockfd上的数据，buf和 len参数分别指定读缓冲区的位置和大小。因为UDP通信没有连接的概念，所以我们每次读取数据都需要获取发送端的socket地址，即参数src_addr所指的内容，addrlcn参数则指定该地址的长度。
			- sendto往sockfd上写人数据，buf和len参数分别指定写缓冲区的位置和大小。dest_addr参数指定接收端的socket地址，addrlen参数则指定该地址的长度。
			- 这两个系统调用的flags参数以及返回值的含义均与send/recv系统调用的flags参数及返回值相同。
			- 值得一提的是，recvfrom/sendto系统调用也可以用于面向连接（STREAM）的 socket的数据读写，只需要把最后两个参数都设置为NULL以忽略发送端/接收端的socket地址（因为我们已经和对方建立了连接，所以已经知道其socket地址了)。
			-
		- 通用数据读写函数
		  collapsed:: true
			- socket编程接口还提供了一对通用的数据读写系统调用。它们不仅能用于TCP流数据，也能用于UDP数据报:
			- ```c
			  #include <sys/socket.h>
			  ssize_t recvmsg( int sockfd, struct msghdr* msg, int flags );
			  ssize_t sendmsg( int sockfd, struct msghdr* msg, int flags );
			  ```
			- sockfd参数指定被操作的目标socket。msg参数是msghdr结构体类型的指针，msghdr结构体的定义如下:
			- ```c
			  struct msghdr 
			  {
			    void* msg_name;			// socket 地址
			    socklen_t msg_namelen;	// socket地址的长度
			    struct iovec* msg_iov;	// 分散的内存块，见后文
			    int msg_iovlen;			// 分散内存块的数量
			    void* msg_control;		// 指向辅助数据的起始位置
			    socklen_t msg_controllen;	// 辅助数据大小
			    int msg_flags;			// 复制函数中的flags参数，并在调用过程中更新
			  }
			  ```
			- msg_name成员指向一个socket地址结构变量。它指定通信对方的socket地址。对于面向连接的TCP协议，该成员没有意义，必须被设置为NULL。这是因为对数据流socket而言，对方的地址已经知道。msg_namelen成员则指定了msg_name所指socket地址的长度。
			- msg_iov成员是iovec结构体类型的指针，iovec结构体的定义如下:
			- ```c
			  struct iovec
			  {
			    void* iov_base;		// 内存起始地址
			    size_t iov_len;		// 这块内存的长度
			  }
			  ```
			- 由上可见，iovec结构体封装了一块内存的起始位置和长度。msg_iovlen指定这样的iovec结构对象有多少个。对于recvmsg而言，数据将被读取并存放在msg_iovlen块分散的内存中，这些内存的位置和长度则由msg_iov指向的数组指定，这称为分散读〈（scatterread);对于sendmsg而言，msg_iovlen块分散内存中的数据将被一并发送，这称为集中写( gather write)。
			- msg_control和 msg_controllen 成员用于辅助数据的传送。我们不详细讨论它们，仅在第13章介绍如何使用它们来实现在进程间传递文件描述符。
			- msg_flags成员无须设定，它会复制recvmsg/sendmsg的 flags参数的内容以影响数据读写过程。recvmsg还会在调用结束前，将某些更新后的标志设置到msg_flags中。
			- recvmsg/sendmsg的 flags参数以及返回值的含义均与send/recv的flags参数及返回值相同。
	- #### 带外标记
	  collapsed:: true
		- ####
		- ####
		- 代码清单5-7演示了TCP带外数据的接收方法。但在实际应用中，我们通常无法预期带外数据何时到来。好在Linux内核检测到TCP紧急标志时，将通知应用程序有带外数据需要接收。内核通知应用程序带外数据到达的两种常见方式是:I/O复用产生的异常事件和SIGURG信号。但是，即使应用程序得到了有带外数据需要接收的通知，还需要知道带外数据在数据流中的具体位置，才能准确接收带外数据。这一点可通过如下系统调用实现:
		- ```c
		  #include <sys/socket.h>
		  int sockatmark( int sockfd );
		  ```
		- sockatmark 判断sockfd是否处于带外标记，即下一个被读取到的数据是否是带外数据。如果是，sockatmark返回1，此时我们就可以利用带MSG_OOB标志的recv调用来接收带外数据。如果不是，则sockatmark返回0。
		-
	- #### 地址信息函数
	  collapsed:: true
		- ####
		- 在某些情况下，我们想知道一个连接socket的本端socket地址，以及远端的socket地址。下面这两个函数正是用于解决这个问题:
		- ```c
		  #include <sys/socket.h>
		  int getsockname( int sockfd, struct sockaddr* address, socklen_t* address_len );
		  int getpeername( int sockfd, struct sockaddr* address, socklen_t* address_len );
		  ```
		- getsockname获取sockfd对应的本端socket地址，并将其存储于address参数指定的内存中，该socket地址的长度则存储于address_len参数指向的变量中。如果实际socket地址的长度大于address所指内存区的大小，那么该socket地址将被截断。getsockname成功时返回0，失败返回-1并设置errno。
		- getpeername获取sockfd对应的远端socket地址，其参数及返回值的含义与getsockname的参数及返回值相同。
	- #### socket选项
	  collapsed:: true
		- 如果说fcntl系统调用是控制文件描述符属性的通用POSIX方法，那么下面两个系统调用则是专门用来读取和设置socket 文件描述符属性的方法:
		- ```c
		  #include <sys/socket.h>
		  int getsockopt( int sockfd, int level, int option_name, void* option_value, 
		                socklen_t* restrict option_len );
		  int setsockopt( int sockfd, int level, int option_name, 
		                const void* option_value, socklen_t option_len );
		  ```
		- sockfd参数指定被操作的目标 socket。level参数指定要操作哪个协议的选项（即属性),比如IPv4、IPv6、TCP等。option_name参数则指定选项的名字。我们在表5-5中列举了socket通信中几个比较常用的socket选项。option_value和 option_len参数分别是被操作选项的值和长度。不同的选项具有不同类型的值，如表5-5中“数据类型”一列所示。
		- ![image.png](../assets/image_1658995471451_0.png){:height 633, :width 673}
		- getsockopt 和 setsockopt这两个函数成功时返回0，失败时返回-1并设置errno。
		- 值得指出的是，对服务器而言，有部分socket选项只能在调用listen系统调用前针对监听socket设置才有效。这是因为连接socket只能由accept调用返回，而accept 从 listen 监听队列中接受的连接至少已经完成了TCP三次握手的前两个步骤（因为listen监听队列中的连接至少已进入SYN_RCVD状态，参见图3-8和代码清单5-4)，这说明服务器已经往被接受连接上发送出了TCP同步报文段。但有的socket选项却应该在TCP同步报文段中设置，比如TCP最大报文段选项（回忆3.2.2小节，该选项只能由同步报文段来发送)。对这种情况，Linux给开发人员提供的解决方案是:对监听socket 设置这些socket选项，那么accept返回的连接socket将自动继承这些选项。这些socket选项包括:SO_DEBUG、SO_DONTROUTE、SO_KEEPALIVE、SO_LINGER、SO_OOBINLINE、SO_RCVBUF、SO_RCVLOWAT、SO_SNDBUF、SO_SNDLOWAT、TCP_MAXSEG和TCP_NODELAY。而对客户端而言，这些socket选项则应该在调用connect函数之前设置，因为connect调用成功返回之后，TCP三次握手已完成。
		-
		- ##### SO_REUSEADDR 选项
			- 我们在3.4.2小节讨论过TCP连接的TIME_WAIT状态，并提到服务器程序可以通过设置socket选项SO_REUSEADDR来强制使用被处于TIME_WAIT状态的连接占用的socket地址。具体实现方法如代码清单5-9所示。
			- ![image.png](../assets/image_1658995579625_0.png)
			- 经过setsockopt的设置之后，即使sock处于TIME_WAIT状态，与之绑定的socket地址也可以立即被重用。此外，我们也可以通过修改内核参数/proc/sys/net/ipv4/tcp_tw_recycle来快速回收被关闭的socket，从而使得TCP连接根本就不进入TIME_WAIT状态，进而允许应用程序立即重用本地的socket地址。
		- ##### SO_RCVBUF 和 SO_SNDBUF 选项
		  collapsed:: true
			- so_RCVBUF 和 SO_SNDBUF选项分别表示TCP接收缓冲区和发送缓冲区的大小。不过，当我们用setsockopt 来设置TCP的接收缓冲区和发送缓冲区的大小时，系统都会将其值加倍，并且不得小于某个最小值。TCP接收缓冲区的最小值是256字节，而发送缓冲区的最小值是2048字节(不过，不同的系统可能有不同的默认最小值)。系统这样做的目的，主要是确保一个TCP连接拥有足够的空闲缓冲区来处理拥塞（比如快速重传算法就期望TCP接收缓冲区能至少容纳4个大小为SMSs的TCP报文段)。此外，我们可以直接修改内核参数/proc/sys/net/ipv4/tcp_rmem和/proc/sys/net/ipv4/tcp_wmem来强制TCP接收缓冲区和发送缓冲区的大小没有最小值限制。我们将在第16章讨论这两个内核参数。
		- ##### SO_LINGER 选项
			- SO_LINGER选项用于控制close系统调用在关闭TCP连接时的行为。默认情况下，当我们使用close系统调用来关闭一个socket时，close将立即返回，TCP模块负责把该socket对应的TCP发送缓冲区中残留的数据发送给对方。
			  如表5-5所示，设置（获取)SO_LINGER选项的值时，我们需要给setsockopt( getsockopt）系统调用传递一个linger类型的结构体，其定义如下:
			- ```c
			  #include <sys/socket.h>
			  struct linger
			  {
			    int l_onoff; 		// 开启（非0）还是关闭（0）该选项
			    int l_linger;		// 滞留时间
			  };
			  ```
			- 根据linger结构体中两个成员变量的不同值, close系统调用可能产生如下3种行为之一;l_onoff等于0。此时SO_LINGER选项不起作用，close用默认行为来关闭socket。l_onoff不为0，l_linger 等于0。此时close系统调用立即返回，TCP模块将丢弃被关闭的socket对应的TCP发送缓冲区中残留的数据，同时给对方发送一个复位报文段(见3.5.2小节)。因此，这种情况给服务器提供了异常终止一个连接的方法。
			  l_onoff不为0，l_linger大于0。此时close的行为取决于两个条件:一是被关闭的 socket对应的TCP发送缓冲区中是否还有残留的数据﹔二是该socket是阻塞的，还是非阻塞的。对于阻塞的socket，close将等待一段长为l_linger的时间，直到TCP模块发送完所有残留数据并得到对方的确认。如果这段时间内TCP模块没有发送完残留数据并得到对方的确认，那么close系统调用将返回-1并设置errno为EWOULDBLOCK。如果socket是非阻塞的，close将立即返回，此时我们需要根据其返回值和errno来判断残留数据是否已经发送完毕。
		-
	- #### 网络信息API
	  collapsed:: true
		- socket地址的两个要素，即IP地址和端口号，都是用数值表示的。这不便于记忆，也不便于扩展（比如从IPv4转移到IPv6)。因此在前面的章节中，我们用主机名来访问一台机器，而避免直接使用其IP地址。同样，我们用服务名称来代替端口号。比如，下面两条telnet命令具有完全相同的作用:
		  telnet 127.0.0.1 80
		  telnet localhost www
		  上面的例子中，telnet客户端程序是通过调用某些网络信息API来实现主机名到IP地址的转换，以及服务名称到端口号的转换的。下面我们将讨论网络信息API中比较重要的几个。
		-
		- #### gethostbyname 和 gethostbyaddr
		  collapsed:: true
			- gethostbyname函数根据主机名称获取主机的完整信息，gethostbyaddr函数根据IP地址获取主机的完整信息。gethostbyname函数通常先在本地的letc/hosts 配置文件中查找主机，如果没有找到，再去访问 DNS服务器。这些在前面章节中都讨论过。这两个函数的定义如下:
			- ```c
			  #include <netdb.h>
			  struct hostent* gethostbyname( const char* name );
			  struct hostent* gethostbyaddr( const void* addr, size_t len, int type );
			  ```
			- name参数指定目标主机的主机名，addr参数指定目标主机的IP地址，len参数指定addr所指IP地址的长度，type参数指定addr所指IP地址的类型，其合法取值包括AF_INET（用于IPv4地址）和AF_INET6(用于IPv6地址)。
			- 这两个函数返回的都是hostent结构体类型的指针，hostent结构体的定义如下:
			- ```c
			  #include <netdb.h>
			  struct hostent
			  {
			    char* h_name;			// 主机名
			    char** h_aliases;		// 主机别名列表，可能有多个
			    int h_addrtype;		// 地址类型（地址族）
			    int h_length;			// 地址长度
			    char** h_addr_list;	// 按网络字节序列出的主机IP地址列表
			  }
			  ```
		- #### getservbyname 和 getservbyport
		  collapsed:: true
			- getservbyname 函数根据名称获取某个服务的完整信息，getservbyport 函数根据端口号获取某个服务的完整信息。它们实际上都是通过读取letc/services文件来获取服务的信息的。这两个函数的定义如下:
			- ```c
			  #include <netdb.h>
			  struct servent* getservbyname( const char* name, const char* proto );
			  struct servent* getservbyport( int port, const char* proto );
			  ```
			- name参数指定目标服务的名字，port参数指定目标服务对应的端口号。proto参数指定服务类型，给它传递“tcp”表示获取流服务，给它传递“udp”表示获取数据报服务，给它传递NULL则表示获取所有类型的服务。
			- 这两个函数返回的都是servent结构体类型的指针，结构体servent 的定义如下:
			- ```c
			  #include <netdb.h>
			  struct servent
			  {
			    char* s_name;			// 服务名称
			    char** s_aliases;		// 服务的别名列表，可能有多个
			    int s_port;			// 端口号
			    char* s_proto;		// 服务类型，通常是tcp或者udp
			  }
			  ```
		- #### getaddrinfo
		  collapsed:: true
			- getaddrinfo函数既能通过主机名获得IP地址（内部使用的是gethostbyname函数)，也能通过服务名获得端口号（内部使用的是getservbyname 函数)。它是否可重入取决于其内部调用的gethostbyname和 getservbyname函数是否是它们的可重入版本。该函数的定义如下:
			- ```c
			  #include <netdb.h>
			  int getaddrinfo( const char* hostname, const char* service, 
			                 const struct addrinfo* hints, struct addrinfo** result );
			  ```
			- hostname参数可以接收主机名，也可以接收字符串表示的IP地址（IPv4采用点分十进制字符串，IPv6则采用十六进制字符串)。同样，service参数可以接收服务名，也可以接收字符串表示的十进制端口号。hints参数是应用程序给getaddrinfo的一个提示，以对getaddrinfo 的输出进行更精确的控制。hints参数可以被设置为NULL，表示允许getaddrinfo反馈任何可用的结果。result参数指向一个链表，该链表用于存储getaddrinfo反馈的结果。
			- getaddrinfo反馈的每一条结果都是addrinfo结构体类型的对象，结构体addrinfo的定义如下:
			- ```c
			  struct addrinfo 
			  {
			    int ai_flags;				// 见后文
			    int ai_family;			// 地址族
			    int ai_socktype;			// 服务类型，SOCK_STREAM 或 SOCK_DGRAM
			    int ai_protocol;			// 见后文
			    socklen_t ai_addrlen;		// socket地址ai_addr的长度
			    char* ai_canotname;		// 主机的别名
			    struct sockaddr* ai_addr;	// 指向socket地址
			    struct addrinfo* ai_next;	// 指向下一个sockinfo结构的对象
			  }
			  ```
			- 该结构体中，ai_protocol成员是指具体的网络协议，其含义和socket系统调用的第三个参数相同，它通常被设置为0。ai_flags成员可以取表5-6中的标志的按位或。
			  ![image.png](../assets/image_1658997761935_0.png)
			- 当我们使用hints参数的时候，可以设置其ai_flags，ai_family，ai_socktype和 ai_protocol四个字段，其他字段则必须被设置为NULL。例如，代码清单5-13利用了hints参数获取主机 ernest-laptop 上的“daytime”流服务信息。
			  ![image.png](../assets/image_1658997814506_0.png)
			- 从代码清单5-13中我们能分析出，getaddrinfo将隐式地分配堆内存（可以通过valgrind等工具查看)，因为res指针原本是没有指向一块合法内存的，所以，getaddrinfo调用结束后，我们必须使用如下配对函数来释放这块内存:
			- ```c
			  #include <netdb.h>
			  void freeaddrinfo( struct addrinfo* res );
			  ```
			-
		- #### getnameinfo
		  collapsed:: true
			- getnameinfo 函数能通过socket地址同时获得以字符串表示的主机名（内部使用的是gethostbyaddr函数）和服务名（内部使用的是getservbyport函数)。它是否可重入取决于其内部调用的gethostbyaddr和 getservbyport 函数是否是它们的可重入版本。该函数的定义如下:
			- ```c
			  #include <netdb.h>
			  int getnameinfo( const struct sockaddr* sockaddr, socklen_t addrlen, char* host,
			                 socklen_t hostlen, char* serv, socklen_t servlen, int flags );
			  ```
			- getnameinfo将返回的主机名存储在host参数指向的缓存中，将服务名存储在serv参数指向的缓存中，hostlen和 servlen参数分别指定这两块缓存的长度。flags参数控制getnameinfo 的行为，它可以接收表5-7中的选项。
			- ![image.png](../assets/image_1658998390510_0.png)
			- getaddrinfo和 getnameinfo函数成功时返回0，失败则返回错误码，可能的错误码如表5-8所示。
			- ![image.png](../assets/image_1658998449298_0.png)
			- Linux 下strerror函数能将数值错误码errno转换成易读的字符串形式。同样，下面的函数可将表5-8中的错误码转换成其字符串形式:
			- ```c
			  #include <netdb.h>
			  const char* gai_strerror( int error );
			  ```
		-
- 高级I/O函数
  collapsed:: true
	- #### [[pipe]] 函数
	  collapsed:: true
		- pipe 函数可用于创建一-个管道，以实现进程间通信。我们将在13.4节讨论如何使用管道来实现进程间通信，本章只介绍其基本使用方式。pipe函数的定义如下:
		- ```c
		  #include <unistd.h>
		  int pipe( int fd[2] );
		  ```
		- pipe函数的参数是一个包含两个int型整数的数组指针。该函数成功时返回0，并将一对打开的文件描述符值填入其参数指向的数组。如果失败，则返回-1并设置errno。
		  通过pipe函数创建的这两个文件描述符fd[0]和fd[1]分别构成管道的两端，往fd[1]写人的数据可以从fd[0]读出。并且，fd[0]只能用于从管道读出数据，fd[1]则只能用于往管道写入数据，而不能反过来使用。如果要实现双向的数据传输，就应该使用两个管道。默认情况下，这一对文件描述符都是阻塞的。此时如果我们用read系统调用来读取一个空的管道,则read将被阻塞，直到管道内有数据可读;如果我们用writc系统调用来往一个满的管道（见后文〉中写入数据，则write亦将被阻塞，直到管道有足够多的空闲空间可用。但如果应用程序将fd[0]和fd[1]都设置为非阻塞的，则read和 write会有不同的行为。关于阻塞和非阻塞的讨论，见第8章。如果管道的写端文件描述符fd[1]的引用计数（见5.7节)减少至0,即没有任何进程需要往管道中写人数据，则针对该管道的读端文件描述符fd[0]的read操作将返回0，即读取到了文件结束标记（End Of File，EOF);反之，如果管道的读端文件描述符fd[0]的引用计数减少至0，即没有任何进程需要从管道读取数据，则针对该管道的写端文件描述符fd[1]的write操作将失败，并引发SIGPIPE信号。
		- 管道内部传输的数据是字节流，这和TCP字节流的概念相同。但二者又有细微的区别。应用层程序能往一个TCP连接中写人多少字节的数据，取决于对方的接收通告窗口的大小和本端的拥塞窗口的大小。而管道本身拥有一个容量限制，它规定如果应用程序不将数据从管道读走的话，该管道最多能被写人多少字节的数据。自Linux 2.6.11内核起，管道容量的大小默认是65536字节。我们可以使用fcntl函数来修改管道容量（见后文)。
		  此外，socket的基础API中有一个socketpair函数。它能够方便地创建双向管道。其定义如下:
		- ```c
		  #include <sys/types.h>
		  #include <sys/socket.h>
		  int socketpair(int domain, int type, int protocol, int fd[2]);
		  ```
		- socketpair前三个参数的含义与socket系统调用的三个参数完全相同，但domain只能使用UNIX本地域协议族AF_UNIX，因为我们仅能在本地使用这个双向管道。最后一个参数则和pipe系统调用的参数一样，只不过socketpair创建的这对文件描述符都是既可读又可写的。socketpair 成功时返回0，失败时返回-1并设置errno。
	- #### [[dup]] 函数和 [[dup2]] 函数
	  collapsed:: true
		- 有时我们希望把标准输入重定向到一个文件，或者把标准输出重定向到一个网络连接(比如CGI编程)。这可以通过下面的用于复制文件描述符的dup或dup2函数来实现:
		- ```c
		  #include <unistd.h>
		  int dup(int file_descriptor);
		  int dup2(int file_descriptor_one, int file_descriptor_tow);
		  ```
		- dup函数创建一个新的文件描述符，该新文件描述符和原有文件描述符file_descriptor指向相同的文件、管道或者网络连接。并且 dup返回的文件描述符总是取系统当前可用的最小整数值。dup2和 dup类似，不过它将返回第一个不小于file_descriptor_two的整数值。dup和dup2系统调用失败时返回-1并设置errno.
		- > 注意: 通过dup和dup2创建的文件描述符并不继承原文件描述符的属性，比如close-on-exec
		  和non-blocking 等。
		-
	- #### [[readv]]函数和 [[writev]] 函数
	  collapsed:: true
		- readv函数将数据从文件描述符读到分散的内存块中，即分散读: writev函数则将多块分散的内存数据一并写人文件描述符中，即集中写。它们的定义如下:
		- ```c
		  #include <sys/uio.h>
		  ssize_t readv( int fd, const struct iovec* vector, int count );
		  ssize_t writev( int fd, const struct iovec* vector, int count );
		  ```
		- fd参数是被操作的目标文件描述符。vector参数的类型是 iovec结构数组。我们在第5章讨论过结构体iovec，该结构体描述一块内存区。count参数是vector数组的长度，即有多少块内存数据需要从fd读出或写到fd。readv和 writev在成功时返回读出/写人fd的字节数，失败则返回-1并设置errno。它们相当于简化版的recvmsg 和 sendmsg函数。
		- 案例：简单web服务器
		- ```c
		  #include <sys/socket.h>
		  #include <netinet/in.h>
		  #include <arpa/inet.h>
		  #include <assert.h>
		  #include <stdio.h>
		  #include <unistd.h>
		  #include <stdlib.h>
		  #include <errno.h>
		  #include <string.h>
		  #include <sys/stat.h>
		  #include <sys/types.h>
		  #include <fcntl.h>
		  #include <sys/uio.h>
		  
		  #define BUFFER_SIZE 1024
		  
		  // 定义两种HTTP状态码和状态信息
		  static const char *status_line[2] = {"200 OK", "500 Internal server error"};
		  
		  int main(int argc, char *argv[])
		  {
		      if (argc <= 3)
		      {
		          printf("usage: %s ip_address port_number filename\n", basename(argv[0]));
		          return 1;
		      }
		      const char *ip = argv[1];
		      int port = atoi(argv[2]);
		      // 将目标文件作为程序的第三个参数传入
		      const char *file_name = argv[3];
		  
		      struct sockaddr_in address;
		      bzero(&address, sizeof(address));
		      address.sin_family = AF_INET;
		      inet_pton(AF_INET, ip, &address.sin_addr);
		      address.sin_port = htons(port);
		  
		      int sock = socket(PF_INET, SOCK_STREAM, 0);
		      assert(sock >= 0);
		  
		      int reuse = 1;
		      setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, &reuse, sizeof(reuse));
		  
		      int ret = bind(sock, (struct sockaddr *)&address, sizeof(address));
		      assert(ret != -1);
		  
		      ret = listen(sock, 5);
		      assert(ret != -1);
		  
		      struct sockaddr_in client;
		      socklen_t client_addrlen = sizeof(client);
		      int connfd = accept(sock, (struct sockaddr *)&client, &client_addrlen);
		      if (connfd < 0)
		      {
		          printf("errno is : %d\n", errno);
		      }
		      else
		      {
		          // 用于保存HTTP应答的状态栏、头部字段和一个空行的缓存区
		          char header_buf[BUFFER_SIZE];
		          memset(header_buf, '\0', BUFFER_SIZE);
		          // 用于存放目标文件内容的应用程序缓存
		          char *file_buf;
		          // 用于获取目标文件的属性，比如是否为目录，文件大小等
		          struct stat file_stat;
		          // 记录目标文件是否有效文件
		          bool valid = true;
		          // 缓存区 header_buf 目前已经使用了多少字节的空间
		          int len = 0;
		          if (stat(file_name, &file_stat) < 0) // 目标文件不存在
		          {
		              valid = false;
		          }
		          else
		          {
		              if (S_ISDIR(file_stat.st_mode)) // 目标文件是一个目录
		              {
		                  valid = false;
		              }
		              else if (file_stat.st_mode & S_IROTH) // 当前用户有读取目录文件权限
		              {
		                  // 动态分配缓存区file_buf，并指定其大小为目标文件的大小 file_stat.st_size加1，然后将目标文件读入缓存区file_buf中
		                  int fd = open(file_name, O_RDONLY);
		                  file_buf = new char[file_stat.st_size + 1];
		                  memset(file_buf, '\0', file_stat.st_size + 1);
		                  if (read(fd, file_buf, file_stat.st_size) < 0)
		                  {
		                      valid = false;
		                  }
		              }
		              else
		              {
		                  valid = false;
		              }
		          }
		          // 如果目标文件有效，则发送正常的HTTP应答
		          if (valid)
		          {
		              // 下面这部分内容将HTTP应答的状态行、"Content-Length"头部字段和一个空行以此加入header_buf中
		              ret = snprintf(header_buf, BUFFER_SIZE - 1, "%s %s\r\n", "HTTP/1.1", status_line[0]);
		              len += ret;
		              ret = snprintf(header_buf + len, BUFFER_SIZE - 1 - len, "Content-Length: %d\r\n", file_stat.st_size);
		              len += ret;
		              ret = snprintf(header_buf + len, BUFFER_SIZE - 1 - len, "%s", "\r\n");
		              // 利用writev将header_buf和file_buf的内容一并写出
		              struct iovec iv[2];
		              iv[0].iov_base = header_buf;
		              iv[0].iov_len = strlen(header_buf);
		              iv[1].iov_base = file_buf;
		              iv[1].iov_len = file_stat.st_size;
		              ret = writev(connfd, iv, 2);
		          }
		          else
		          {
		              // 目标文件无效，则溶质客户端服务器发生了”内部错误“
		              ret = snprintf(header_buf, BUFFER_SIZE - 1, "%s %s\r\n", "HTTP/1.1", status_line[1]);
		              len += ret;
		              ret = snprintf(header_buf + len, BUFFER_SIZE - 1 - len, "%s", "\r\n");
		              send(connfd, header_buf, strlen(header_buf), 0);
		          }
		          close(connfd);
		          delete[] file_buf;
		      }
		      close(sock);
		      return 0;
		  }
		  ```
	- #### [[sendfile]] 函数
	  collapsed:: true
		- sendfile函数在两个文件描述符之间直接传递数据（完全在内核中操作)，从而避免了内核缓冲区和用户缓冲区之间的数据拷贝，效率很高，这被称为零拷贝。sendfile 函数的定义如下:
		- ```c
		  #include <sys/sendfile.h>
		  ssize_t sendfile( int out_fd, int in_fd, off_t* offset, size_t count );
		  ```
		- in_fd参数是待读出内容的文件描述符，out_fd参数是待写入内容的文件描述符。offset参数指定从读入文件流的哪个位置开始读，如果为空，则使用读人文件流默认的起始位置。count参数指定在文件描述符in_fd和 out_fd之间传输的字节数。sendfile成功时返回传输的字节数，失败则返回-1并设置errno。该函数的man手册明确指出，in_fd必须是一个支持类似mmap函数的文件描述符，即它必须指向真实的文件，不能是socket和管道﹔而out_fd则必须是一个socket。由此可见，sendfile几乎是专门为在网络上传输文件而设计的。下面的代码清单6-3利用sendfile函数将服务器上的一个文件传送给客户端。
		- ```c
		  #include <sys/socket.h>
		  #include <netinet/in.h>
		  #include <arpa/inet.h>
		  #include <assert.h>
		  #include <stdio.h>
		  #include <unistd.h>
		  #include <stdlib.h>
		  #include <errno.h>
		  #include <string.h>
		  #include <sys/stat.h>
		  #include <sys/types.h>
		  #include <fcntl.h>
		  #include <sys/uio.h>
		  #include <sys/sendfile.h>
		  
		  int main(int argc, char *argv[])
		  {
		      if (argc <= 3)
		      {
		          printf("usage: %s ip_address port_number filename\n", basename(argv[0]));
		          return 1;
		      }
		      const char *ip = argv[1];
		      int port = atoi(argv[2]);
		      // 将目标文件作为程序的第三个参数传入
		      const char *file_name = argv[3];
		  
		      int filefd = open(file_name, O_RDONLY);
		      printf("open: %s\n", strerror(errno)); 
		      assert(filefd > 0);
		  
		      struct stat stat_buf;
		      fstat(filefd, &stat_buf);
		  
		      struct sockaddr_in address;
		      bzero(&address, sizeof(address));
		      address.sin_family = AF_INET;
		      inet_pton(AF_INET, ip, &address.sin_addr);
		      address.sin_port = htons(port);
		  
		      int sock = socket(PF_INET, SOCK_STREAM, 0);
		      assert(sock >= 0);
		  
		      int reuse = 1;
		      setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, &reuse, sizeof(reuse));
		  
		      int ret = bind(sock, (struct sockaddr *)&address, sizeof(address));
		      assert(ret != -1);
		  
		      ret = listen(sock, 5);
		      assert(ret != -1);
		  
		      struct sockaddr_in client;
		      socklen_t client_addrlen = sizeof(client);
		      int connfd = accept(sock, (struct sockaddr *)&client, &client_addrlen);
		      if (connfd < 0)
		      {
		          printf("errno is : %d\n", errno);
		      }
		      else
		      {
		          sendfile(connfd, filefd, NULL, stat_buf.st_size);
		          close(connfd);
		      }
		      close(sock);
		      return 0;
		  }
		  
		  ```
	- #### [[mmap]] 函数和 [[munmap]] 函数
	  collapsed:: true
		- mmap函数用于申请一段内存空间。我们可以将这段内存作为进程间通信的共享内存，也可以将文件直接映射到其中。munmap函数则释放由mmap创建的这段内存空间。它们的定义如下:
		- ```c
		  #include <sys/mman.h>
		  void* mmap( void *start, size_t length, int prot, int flags, int fd, 
		            off_t offset );
		  int munmap( void *start, size_t length );
		  ```
		- start参数允许用户使用某个特定的地址作为这段内存的起始地址。如果它被设置成NULL，则系统自动分配一个地址。length参数指定内存段的长度。prot参数用来设置内存段的访问权限。它可以取以下几个值的按位或:
			- PROT_READ，内存段可读。
			- PROT_WRITE，内存段可写。PROT_EXEC，内存段可执行。
			- PROT_NONE，内存段不能被访问。
		- flags参数控制内存段内容被修改后程序的行为。它可以被设置为表6-1中的某些值（这里仅列出了常用的值）的按位或(其中MAP_SHARED和MAP_PRIVATE是互斥的，不能同时指定)。
		- ![image.png](../assets/image_1659080818894_0.png)
		- fd参数是被映射文件对应的文件描述符。它一般通过open系统调用获得。offset参数设置从文件的何处开始映射（对于不需要读入整个文件的情况)。
		- mmap函数成功时返回指向目标内存区域的指针，失败则返回MAP_FAILED((void*)-1)并设置errno。munmap函数成功时返回0，失败则返回-1并设置errno。
		-
	- #### [[splice]] 函数
	  collapsed:: true
		- splice函数用于在两个文件描述符之间移动数据，也是零拷贝操作。splice函数的定义如下:
		- ```c
		  #include <fcntl.h>
		  ssize_t splice( int fd_in, loff_t* off_in, int fd_out, loff_t* off_out,
		                size_t len, unsigned int flags );
		  ```
		- fd_in参数是待输人数据的文件描述符。如果fd_in是一个管道文件描述符，那么off_in参数必须被设置为NULL。如果fd_in不是一个管道文件描述符（比如socket)，那么 off_in表示从输入数据流的何处开始读取数据。此时，若off_in被设置为NULL，则表示从输入数据流的当前偏移位置读入﹔若off_in不为NULL，则它将指出具体的偏移位置。fd_outoff_out参数的含义与fd_in/off_in相同，不过用于输出数据流。len参数指定移动数据的长度﹔flags参数则控制数据如何移动，它可以被设置为表6-2中的某些值的按位或。
		  ![image.png](../assets/image_1659080937861_0.png)
		- 使用splice函数时，fd_in和fd_out必须至少有一个是管道文件描述符。splice函数调用成功时返回移动字节的数量。它可能返回0，表示没有数据需要移动，这发生在从管道中读取数据(fd_in是管道文件描述符）而该管道没有被写入任何数据时。splice函数失败时返回-1并设置errno。常见的errno如表6-3所示。
		- ![image.png](../assets/image_1659080956364_0.png)
		-
	- #### [[tee]] 函数
	  collapsed:: true
		- tee函数在两个管道文件描述符之间复制数据，也是零拷贝操作。它不消耗数据，因此源文件描述符上的数据仍然可以用于后续的读操作。tee函数的原型如下:
		- ```c
		  #include <fcntl.h>
		  ssize_t tee( int fd_in, int fd_out, size_t len, unsigned int flags );
		  ```
		- 该函数的参数的含义与splice相同（但fd_in和 fd_out必须都是管道文件描述符)。tee函数成功时返回在两个文件描述符之间复制的数据数量（字节数)。返回0表示没有复制任何数据。tee失败时返回-1并设置errno。
	- #### [[fcntl]] 函数
	  collapsed:: true
		- fcntl函数，正如其名字(file control〉描述的那样，提供了对文件描述符的各种控制操作。另外一个常见的控制文件描述符属性和行为的系统调用是ioctl，而且 ioctl 比 fentl能够执行更多的控制。但是，对于控制文件描述符常用的属性和行为，fcntl函数是由POSIX规范指定的首选方法。所以本书仅讨论fcntl函数。fcntl函数的定义如下:
		- ```c
		  #include <fcntl.h>
		  int fcntl( int fd, int cmd, ... );
		  ```
		- fd参数是被操作的文件描述符，cmd参数指定执行何种类型的操作。根据操作类型的不同，该函数可能还需要第三个可选参数 arg。fcntl函数支持的常用操作及其参数如表6-4所示。
		- ![image.png](../assets/image_1659081846899_0.png)
		  ![image.png](../assets/image_1659081879154_0.png)
		- fcntl函数成功时的返回值如表6-4最后一列所示，失败则返回-1并设置errno。
		  在网络编程中，fentl函数通常用来将一-个文件描述符设置为非阻塞的，如代码清单6-6所示。
		- ```c
		  int setnonblocking( int fd )
		  {
		    int old_option = fcntl( fd, F_GETFL ); 		// 获取文件描述符旧的状态标志
		    int new_option = old_option | O_NONBLOCK;		// 设置非阻塞标志
		    fcntl( fd, F_SETFL, new_option );
		    return old_option;							// 返回文件描述符旧的状态标志，以便日后恢复该状态标志
		  }
		  ```
		- 此外，SIGIO和SIGURG这两个信号与其他Linux信号不同，它们必须与某个文件描述符相关联方可使用:当被关联的文件描述符可读或可写时，系统将触发SIGIO信号﹔当被关联的文件描述符（而且必须是一个socket)上有带外数据可读时，系统将触发SIGURG信号。将信号和文件描述符关联的方法，就是使用fcntl 函数为目标文件描述符指定宿主进程或进程组，那么被指定的宿主进程或进程组将捕获这两个信号。使用SIGIO时，还需要利用fcntl 设置其О_ASYNC标志（异步I/O标志，不过SIGIO信号模型并非真正意义上的异步VО模型，见第8章)。
- Linux服务器程序规范
  collapsed:: true
	- #### 日志
	  collapsed:: true
		- ##### Linux系统日志
		  collapsed:: true
			- 工欲善其事，必先利其器。服务器的调试和维护都需要一个专业的日志系统。Linux提供一个守护进程来处理系统日志——syslogd，不过现在的Linux系统上使用的都是它的升级版——rsyslogd。
			  rsyslogd守护进程既能接收用户进程输出的日志，又能接收内核日志。用户进程是通过调用syslog函数生成系统日志的。该函数将日志输出到一个UNIX本地域socket类型（AF_UNIX)的文件l/dev/log 中，rsyslogd则监听该文件以获取用户进程的输出。内核日志在老的系统上是通过另外一个守护进程rklogd来管理的，rsyslogd利用额外的模块实现了相同的功能。内核日志由printk等函数打印至内核的环状缓存（ring buffer）中。环状缓存的内容直接映射到/proc/kmsg 文件中。rsyslogd则通过读取该文件获得内核日志。
			  rsyslogd守护进程在接收到用户进程或内核输入的日志后，会把它们输出至某些特定的日志文件。默认情况下，调试信息会保存至/var/log/debug文件，普通信息保存至/var/log/messages文件，内核消息则保存至/var/log/kern.log 文件。不过，日志信息具体如何分发，可以在 rsyslogd的配置文件中设置。rsyslogd 的主配置文件是letc/rsyslog.conf，其中主要可以设置的项包括:内核日志输入路径，是否接收UDP日志及其监听端口（默认是514，见letclservices文件)，是否接收TCP日志及其监听端口，日志文件的权限，包含哪些子配置文件(比如/etc/rsyslog.d/*.conf)。rsyslogd的子配置文件则指定各类日志的目标存储文件。
			- ![image.png](../assets/image_1659082496059_0.png)
			-
		- ##### `syslog` 函数
		  collapsed:: true
			- 应用程序使用syslog 函数与rsyslogd守护进程通信。syslog 函数的定义如下:
			- ```c
			  #include <syslog.h>
			  void syslog( int priority, const char* message, ... );
			  ```
			- 该函数采用可变参数（第二个参数message和第三个参数…〉来结构化输出。priority参数是所谓的设施值与日志级别的按位或。设施值的默认值是LOG_USER，我们下面的讨论也只限于这一种设施值。日志级别有如下几个:
			- ```c
			  #include <syslog.h>
			  #define LOG_EMERG		0	// 系统不可用
			  #define LOG_ALERT		1	// 报警，需要立即采取动作
			  #define LOG_CRIT		2	// 非常严重的情况
			  #define LOG_ERR			3	// 错误
			  #define LOG_WARNING		4	// 警告
			  #define LOG_NOTICE		5	// 通知
			  #define LOG_INFO		6	// 信息
			  #define LOG_DEBUG		7	// 调试
			  ```
			- 下面这个函数可以改变syslog 的默认输出方式，进一步结构化日志内容:
			- ```c
			  #include <syslog.h>
			  void openlog( const char* ident, int logopt, int facility );
			  ```
			- ident参数指定的字符串将被添加到日志消息的日期和时间之后，它通常被设置为程序的名字。logopt参数对后续syslog调用的行为进行配置，它可取下列值的按位或:
			- ```c
			  #define LOG_PID		0x01	// 在日志消息中包含程序PID
			  #define LOG_CONS	0x02	// 如果消息不能记录到日志文件，则打印至终端
			  #define LOG_ODELAY	0x04	// 延迟打开日志功能直到第一次调用syslog
			  #define LOG_NDELAY	0x08	// 不延迟打开日志功能
			  ```
			- facility参数可用来修改syslog 函数中的默认设施值。
			- 此外，日志的过滤也很重要。程序在开发阶段可能需要输出很多调试信息，而发布之后我们又需要将这些调试信息关闭。解决这个问题的方法并不是在程序发布之后删除调试代码(因为日后可能还需要用到)，而是简单地设置日志掩码，使日志级别大于日志掩码的日志信息被系统忽略。下面这个函数用于设置syslog 的日志掩码:
			- ```c
			  #include <syslog.h>
			  int setlogmask( int maskpri );
			  ```
			- maskpri参数指定日志掩码值。该函数始终会成功，它返回调用进程先前的日志掩码值。最后，不要忘了使用如下函数关闭日志功能:
			- ```c
			  #include <syslog.h>
			  void closelog();
			  ```
	- #### 用户信息
	  collapsed:: true
		- ##### UID、EUID、GID和EDGID
		  collapsed:: true
			- 用户信息对于服务器程序的安全性来说是很重要的，比如大部分服务器就必须以root身份启动，但不能以root身份运行。下面这一组函数可以获取和设置当前进程的真实用户ID(UID)、有效用户ID (EUID)、真实组ID(GID)和有效组ID （EGID):
			- ```c
			  #include <sys/types.h>
			  #include <unistd.h>
			  uid_t getuid(); 			// 获取真实用户ID
			  uid_t geteuid(); 			// 获取有效用户ID
			  gid_t getgid();				// 获取真实组ID
			  gid_t getegid();			// 获取有效组ID
			  int setuid( uid_t uid );	// 设置真实用户ID
			  int seteuid( uid_t uid ); 	// 设置有效用户ID
			  int setgid( gid_t gid );	// 设置真实组ID
			  int setegid( gid_t gid );	// 设置有效组ID
			  ```
			- 需要指出的是，一个进程拥有两个用户ID:UID和EUID。EUID存在的目的是方便资源访问:它使得运行程序的用户拥有该程序的有效用户的权限。比如 su程序，任何用户都可以使用它来修改自己的账户信息，但修改账户时su程序不得不访问/etc/passwd文件，而访问该文件是需要root权限的。那么以普通用户身份启动的su程序如何能访问/etc/passwd文件呢﹖窍门就在EUID。用ls命令可以查看到，su程序的所有者是root，并且它被设置了set-user-id标志。这个标志表示，任何普通用户运行su程序时，其有效用户就是该程序的所有者root。那么，根据有效用户的含义，任何运行su程序的普通用户都能够访问letc/passwd文件。有效用户为root 的进程称为特权进程〈privileged processes)。EGID 的含义与EUID类似:给运行目标程序的组用户提供有效组的权限。
			  下面的代码清单7-1可以用来测试进程的UID和 EUID的区别。
			- ```c
			  #include <unistd.h>
			  #include <stdio.h>
			  
			  int main() {
			    uid_t uid = getuid();
			    uid_t euid = geteuid();
			    printf("userid is %d, effective userid is : %d\n", uid, euid);
			    return 0;
			  }
			  ```
			- 编译该文件，将生成的可执行文件（名为test_uid）的所有者设置为root，并设置该文件的set-user-id标志，然后运行该程序以查看UID 和EUID。具体操作如下:
			- ![image.png](../assets/image_1659085521838_0.png)
			- 从测试程序的输出来看，进程的UID是启动程序的用户的ID，而EUID则是root账户(文件所有者)的ID.
		- ##### 切换用户
		  collapsed:: true
			- 下面的代码清单7-2展示了如何将以root身份启动的进程切换为以一个普通用户身份运行。
			- ```c
			  static bool switch_to_user( uid_t user_id, gid_t gp_id )
			  {
			    // 先确保目标用户不是root
			    if( (user_id == 0) && (gp_id == 0) )
			    {
			      return false;
			    }
			    // 确保当前用户是合法用户：root或者目标用户
			    gid_t gid = getgid();
			    uid_t uid = getuid();
			    if( ((gid != 0) || (uid != 0)) && ((gid != gp_id) || uid != user_id)) )
			    {
			      return false;
			    }
			    // 如果不是root，则已经是目标用户
			    if(uid != 0) 
			    {
			      return true;
			    }
			    // 切换到目标用户
			    if( (setgid(gp_id) < 0) || (setuid(user_id) < 0) )
			    {
			      return false;
			    }
			    return true;
			  }
			  ```
	- #### 进程间关系
	  collapsed:: true
		- 进程组
			- Linux下每个进程都隶属于一个进程组，因此它们除了PID信息外，还有进程组ID
			  (PGID)。我们可以用如下函数来获取指定进程的PGID:
			- ```c
			  #include <unistd.h>
			  pid_t getpgid( pid_t pid );
			  ```
			- 该函数成功时返回进程pid所属进程组的PGID,失败则返回-1并设置ermo。
			  每个进程组都有一个首领进程，其PGID和PID相同。进程组将一直存在， 直到其中所
			  有进程都退出，或者加入到其他进程组。
			  下面的函数用于设置PGID:
			- ```c
			  #include <unistd.h>
			  int setpgid( pid_t pid, pit_t pgid );
			  ```
			- 该函数将PID为pid的进程的PGID设置为pgid。如果pid和pgid相同，则由pid指
			  定的进程将被设置为进程组首领;如果pid为0,则表示设置当前进程的PGID为pgid;如
			  果pgid为0，则使用pid作为目标PGID.setpgid函数成功时返回0,失败则返回-1并设置
			  errno。
			  一个进程只能设置自己或者其子进程的PGID。并且，当子进程调用exec系列函数后,
			  我们也不能再在父进程中对它设置PGID。
		- 会话
			- 一些有关联的进程组将形成-一个会话(session)。 下面的函数用于创建-一个会话:
			- ```c
			  #include <unistd.h>
			  pid_t setsid( void );
			  ```
			- 该函数不能由进程组的首领进程调用，否则将产生- -个错误。对于非组首领的进程，调用该函数不仅创建新会话，而且有如下额外效果:
				- 调用进程成为会话的首领，此时该进程是新会话的唯- - 成员。
				- 新建-一个进程组，其PGID就是调用进程的PID，调用进程成为该组的首领。
				- 调用进程将甩开终端( 如果有的话)。
				- 该函数成功时返回新的进程组的PGID,失败则返回-1并设置ermo.
			- Linux进程并未提供所谓会话ID(SID)的概念，但Linux系统认为它等于会话首领所
			  在的进程组的PGID，并提供了如下函数来读取SID:
			- ```c
			  #include <unistd.h>
			  pid_t getsid( pid_t pid );
			  ```
	- #### 系统资源限制
	  collapsed:: true
		- Linux上运行的程序都会受到资源限制的影响，比如物理设备限制(CPU数量、内存数量等)、系统策略限制(CPU时间等)，以及具体实现的限制(比如文件名的最大长度)。
		  Linux系统资源限制可以通过如下一对函数来读取和设置:
		- ```c
		  #include <sys/resource.h>
		  int getrlimit( int resource, struct rlimit *rlim );
		  int setrlimit( int resource, const struct rlimit *rlim );
		  ```
		- rlim参数是rlimit结构体类型的指针，rlimit 结构体的定义如下:
		- ```c
		  struct rlimit 
		  {
		    rlim_t rlim_cur;
		    rlim_t rlim_max;
		  };
		  ```
		- rlim_t是一个整数类型，它描述资源级别。rlim_cur成员指定资源的软限制，rlim_max
		  成员指定资源的硬限制。软限制是-一个建议性的、最好不要超越的限制，如果超越的话，系统可能向进程发送信号以终止其运行。例如，当进程CPU时间超过其软限制时，系统将向进程发送SIGXCPU信号;当文件尺寸超过其软限制时，系统将向进程发送SIGXFSZ信号(见第10章)。硬限制一般是软限制的上限。普通程序可以减小硬限制，而只有以root身份运行的程序才能增加硬限制。此外，我们可以使用ulimit命令修改当前shell环境下的资源限制(软限制或/和硬限制)，这种修改将对该shell启动的所有后续程序有效。我们也可以通过修改配置文件来改变系统软限制和硬限制，而且这种修改是永久的，详情见第16章。
		  resource参数指定资源限制类型。表7-1列举了部分比较重要的资源限制类型。
		- ![image.png](../assets/image_1659274711118_0.png)
		- setrlimit和getrlimit成功时返回0，失败则返回-1并设置errno。
		-
	- #### 改变工作目录和根目录
	  collapsed:: true
		- 有些服务器程序还需要改变工作目录和根目录，比如我们第4章讨论的Web服务器。一般来说，Web 服务器的逻辑根目录并非文件系统的根目录“/”，而是站点的根目录(对于Linux的Web服务来说，该目录一般是/var/www/).
		- 获取进程当前工作目录和改变进程工作目录的两数分别是:
		- ```c
		  #include <unistd.h>
		  char* getcwd( char* buf, size_t size );
		  int chdir( const char* path );
		  ```
		- buf参数指向的内存用于存储进程当前工作目录的绝对路径名，其大小由size参数指定。如果当前工作目录的绝对路径的长度(再加上-一个空结束字符“\0”)超过了size, 则getewd将返回NULL,并设置errno为ERANGE.如果buf为NULL并且size非0，则getewd可能在内部使用malloc动态分配内存，并将进程的当前工作目录存储在其中。如果是这种情况，则我们必须自己来释放getcwd在内部创建的这块内存。getcwd 函数成功时返回一个指向目标存储区(buf指向的缓存区或是getcwd在内部动态创建的缓存区)的指针，失败则返回NULL并设置
		  errno。
		- chdir函数的path 参数指定要切换到的目标目录。它成功时返回0，失败时返回-1并设置errno。
		- 改变进程根目录的函数是chroot,其定义如下:
		- ```c
		  #include <unistd.h>
		  int chroot( const char* path );
		  ```
		- path参数指定要切换到的目标根目录。它成功时返回0，失败时返回-1并设置ermo。chroot并不改变进程的当前工作目录，所以调用chroot之后，我们仍然需要使用chdir(“1”)来将工作目录切换至新的根目录。改变进程的根目录之后，程序可能无法访问类似/dev的文件(和目录)，因为这些文件(和目录)并非处于新的根目录之下。不过好在调用chroot之后，进程原先打开的文件描述符依然生效，所以我们可以利用这些早先打开的文件描述符来访问调用chroot之后不能直接访问的文件(和目录)，尤其是一些日志文件。此外，只有特权进程才能改变根目录。
	- #### 服务器程序后台化
	  collapsed:: true
		- 最后，我们讨论如何在代码中让- - 个进程以守护进程的方式运行。守护进程的编写遵循一定的步骤，下面我们通过一个具体实现来探讨，如代码清单7-3所示。
		- ```c
		  bool daemonize() 
		  {
		    // 创建子进程，关闭父进程，这样可以使程序在后台运行
		    pid_t pid = fork();
		    if( pid < 0 ) 
		    {
		      return false;
		    }
		    else if( pid > 0 )
		    {
		      exit( 0 );
		    }
		    /* 设置文件权限掩码，当进程创建新文件（使用open( const char *pathname, int flags, 
		    mode_t mode ) 系统调用）时，文件的权限将是 mode & 0777 */
		    umake( 0 );
		    
		    // 创建新的会话，设置本进程为进程组的首领
		    pid_t sid = setsid();
		    if( sid < 0)
		    {
		      return false;
		    }
		    // 切换工作目录
		    if( ( chdir("/") ) < 0 )
		    {
		      return false;
		    }
		    // 关闭标准输入设备，标准输出设备和标准错误设备
		    close( STDIN_FILENO );
		    close( STDOUT_FILENO );
		    close( STDERR_FILENO );
		    
		    // 关闭其他已经打开的文件描述符，代码省略
		    // 将标准输入、标准输出和标准错误输出都指定到 /dev/null 文件
		    open( "/dev/null", O_RDONLY );
		    open( "/dev/null", O_RDWR );
		    open( "dev/null", O_RDWR );
		    return true;
		  }
		  ```
		- 实际上，Linux提供了完成同样功能的库函数
		- ```c
		  #include <unistd.h>
		  int daemon( int nochdir, int noclose );
		  ```
		- 其中，nochdir 参数用于指定是否改变工作目录，如果给它传递0，则工作目录将被设置为“1" (根目录),否则继续使用当前工作目录。noclose 参数为0时，标准输入、标准输出和标准错误输出都被重定向到/dev/null文件，否则依然使用原来的设备。该函数成功时返回0，失败则返回-1并设置ermno.
		-
- I/O复用
  collapsed:: true
	- #### [[select]]系统调用
	  collapsed:: true
		- select系统调用的用途是:在一段指定时间内，监听用户感兴趣的文件描述符上的可读、可写和异常等事件。
		- ##### select API
		  collapsed:: true
			- select系统调用的原型如下：
			- ```C
			  #include <sys/select.h>
			  int select( int nfds, fd_set* readfds, fd_set* writefds, fd_set* exceptfds,
			            struct timeval* timeout );
			  ```
			- 1) nfds 参数指定被监听的文件描述符的总数。它通常被设置为select监听的所有文件描述符中的最大值加1，因为文件描述符是从0开始计数的。
			- 2) readfds、 writefds 和exceptfds 参数分别指向可读、可写和异常等事件对应的文件
			  描述符集合。应用程序调用select函数时，通过这3个参数传人自己感兴趣的文件描述符。select调用返回时，内核将修改它们来通知应用程序哪些文件描述符已经就绪。这3个参数是fd_ set结构指针类型。fd_ set 结构体的定义如下:
			- ```c
			  #include <typesizes.h>
			  #define __FD_SETSIZE 1024
			  
			  #include <sys/select.h>
			  #define FD_SETSIZE __FD_SETSIZE
			  typedef long int __fd_mask;
			  #undef __FDDBITS
			  #define __NFDBITS ( 8 * (int) sizeof (__fd_mask ) )
			  typedef struct
			  {
			    #ifdef __USE_XOPEN
			    	__fd_mask fds_bits[ __FD_SETSIZE / __NFDBITS ];
			    #define __FDS_BITS(set) ((set)->fds_bits)
			    #else
			    	__fd_mask __fds_bits[ __FD_SETSIZE / __NFDBITS ];
			    #define __FDS_BITS(set) ((set)->__fds_bits)
			    #endif
			  } fd_set;
			  ```
			- 由以上定义可见，fd. set结构体仅包含- -个整型数组， 该数组的每个元素的每- -位 (bit)标记-一个文件描述符。fd_set能容纳的文件描述符数量由FD_SETSIZE指定,这就限制了select能同时处理的文件描述符的总量。
			  由于位操作过于烦琐，我们应该使用下面的-系列宏来访问fd. _set 结构体中的位:
			- ```c
			  #include <sys/select.h>
			  FD_ZERO( fd_set *fdset ); 			// 清除fdset的所有位
			  FD_SET( int fd, fd_set *fdset );	// 设置fdset的位fd
			  FD_CLR( int fd, fd_set *fdset );	// 清除fdset的位fd
			  int FD_ISSET( int fd, fd_set *fdset );	// 测试fdset的位fd是否被设置
			  ```
			- 3) timeout 参数用来设置select函数的超时时间。它是- -个timeval结构类型的指针，采
			  用指针参数是因为内核将修改它以告诉应用程序select等待了多久。不过我们不能完全信任sclect调用返回后的timeout值，比如调用失败时timeout值是不确定的。timeval 结构体的定义如下:
			- ```c
			  struct timeval
			  {
			    long tv_sec;		// 秒数
			    long tv_usec;		// 微秒数
			  }
			  ```
			- 由以上定又可见，select 给我们提供了一个微秒级的定时方式。如果给timcout变量的
			  tv_sec成员和tv_usec成员都传递0，则select将立即返回。如果给timeout传递NULL,则
			  select将一直阻塞，直到某个文件描述符就绪。
			  selct成功时返回就绪(可读、可写和异常)文件描述符的总数。如果在超时时间内没
			  有任何文件描述符就绪，sclect 将返回0. select 失败时返回-1并设置ermo。如果在select等待期间，程序接收到信号，则select立即返回-1，并设置errmo为EINTR.
		- ##### 文件描述符就绪条件
		  collapsed:: true
			- 哪些情况下文件描述符可以被认为是可读、可写或者出现异常，对于select的使用非常关键。在网络编程中，下 列情况下socket 可读:
				- socket内核接收缓存区中的字节数大于或等于其低水位标记sO_ _RCVLOWAT. 此时我们可以无阻塞地读该socket,并且读操作返回的字节数大于0.
				- socket通信的对方关闭连接。此时对该socket的读操作将返回0。
				- 监听socket.上有新的连接请求。
				- socket上有未处理的错误。此时我们可以使用getsockopt来读取和清除该错误。
			- 下列情况下socket可写:
				- socket内核发送缓存区中的可用字节数大于或等于其低水位标记SO_SNDLOWAT。此时我们可以无阻塞地写该socket, 并且写操作返回的字节数大于0。
				- socket的写操作被关闭。对写操作被关闭的socket执行写操作将触发一个SIGPIPE信号。
				- socket使用非阻塞connect 连接成功或者失败(超时)之后。
				- socket上有未处理的错误。此时我们可以使用getsockopt来读取和清除该错误。
				- 网络程序中，select 能处理的异常情况只有一种: socket 上接收到带外数据。
		- ##### 处理带外数据
		  collapsed:: true
			- 上一小节提到，socket上接收到普通数据和带外数据都将使select返回，但socket处于不同的就绪状态:前者处于可读状态，后者处于异常状态。代码清单9-1描述了select是如何同时处理二者的。
			- ![image.png](../assets/image_1659277211018_0.png)
			  ![image.png](../assets/image_1659277266739_0.png)
			  ![image.png](../assets/image_1659277286446_0.png)
			  ![image.png](../assets/image_1659277298057_0.png)
			-
			-
			-
			-
			-
	- #### [[poll]]
	  collapsed:: true
		- poll系统调用和 select类似，也是在指定时间内轮询一定数量的文件描述符，以测试其中是否有就绪者。poll的原型如下:
		- ```c
		  #include <poll.h>
		  int poll( struct pollfd* fds, nfds_t nfds, int timeout );
		  ```
		- 1 ) fds参数是一个pollfd结构类型的数组，它指定所有我们感兴趣的文件描述符上发生的可读、可写和异常等事件。pollfd结构体的定义如下:
		- ```c
		  struct pollfd 
		  {
		    int fd; 		// 文件描述符
		    short events;	// 注册的事件
		    short revents;	// 实际发生的事件，由内核填充
		  }
		  ```
		- 其中，fd成员指定文件描述符;events成员告诉poll监听f(上的哪些事件，它是一系列事件的按位或﹔revents成员则由内核修改，以通知应用程序fd上实际发生了哪些事件。poll支持的事件类型如表9-1所示。
		- ![image.png](../assets/image_1659325432728_0.png)
		  ![image.png](../assets/image_1659325442715_0.png)
		- 表9-1中，POLLRDNORM、POLLRDBAND、POLLWRNORM、POLLWRBAND由XOPEN规范定义。它们实际上是将POLLIN事件和POLLOUT事件分得更细致，以区别对待普通数据和优先数据。但Linux并不完全支持它们。
		  通常，应用程序需要根据recv调用的返回值来区分socket上接收到的是有效数据还是对方关闭连接的请求，并做相应的处理。不过，自Linux内核2.6.17开始，GNU为poll系统调用增加了一个POLLRDHUP事件，它在 socket上接收到对方关闭连接的请求之后触发。这为我们区分上述两种情况提供了一种更简单的方式。但使用POLLRDHUP事件时，我们需要在代码最开始处定义_GNU_sOURCE。
		- 2 ) nfds参数指定被监听事件集合fds的大小。其类型nfds_t的定义如下:
		- ```c
		  typedef unsigned long int nfds_t;
		  ```
		- 3 ) timeout参数指定poll的超时值，单位是毫秒。当timeout为-1时，poll调用将永远阻塞，直到某个事件发生;当timcout为0时，poll 调用将立即返回。
		- poll系统调用的返回值的含义与select相同。
		-
	- #### [[epoll]]
	  collapsed:: true
		- 内核事件表
		  collapsed:: true
			- epoll 是Linux特有的IVO复用函数。它在实现和使用上与select、poll有很大差异。首先，epoll使用一组函数来完成任务，而不是单个函数。其次，epoll把用户关心的文件描述符上的事件放在内核里的一个事件表中，从而无须像select和 poll那样每次调用都要重复传人文件描述符集或事件集。但epoll需要使用一个额外的文件描述符，来唯一标识内核中的这个事件表。这个文件描述符使用如下epoll_create函数来创建:
			- ```c
			  #include <sys/epoll.h>
			  int epoll_create( int size );
			  ```
			- size参数现在并不起作用，只是给内核一个提示，告诉它事件表需要多大。该函数返回的文件描述符将用作其他所有epoll 系统调用的第一个参数，以指定要访问的内核事件表。
			  下面的函数用来操作epoll的内核事件表:
			- ```c
			  #include <sys/epoll.h>
			  int epoll_ctl( int epfd, int op, int fd, struct epoll_event *event );
			  ```
			- fd参数是要操作的文件描述符，op参数则指定操作类型。操作类型有如下3种:
				- EPOLL_CTL_ADD，往事件表中注册fd上的事件。
				- EPOLL_CTL_MOD，修改fd上的注册事件。
				- EPOLL_CTL_DEL，删除fd上的注册事件。
			- event参数指定事件，它是epoll _event结构指针类型。epoll event的定义如下:
			- ```c
			  struct epoll_event 
			  {
			    __unit32_t events; 		// epoll事件
			    epoll_data_t data;		// 用户数据
			  }
			  ```
			- 其中events成员描述事件类型。epoll支持的事件类型和poll 基本相同。表示epoll事件类型的宏是在poll对应的宏前加上“E”，比如 epoll 的数据可读事件是EPOLLIN。但epoll有两个额外的事件类型——EPOLLET和 EPOLLONESHOT。它们对于epoll的高效运作非常关键，我们将在后面讨论它们。data成员用于存储用户数据，其类型epoll_data_t的定义如下:
			- ```c
			  typedef union epoll_data
			  {
			    void* ptr;
			    int fd;
			    unit32_t u32;
			    unit64_t u64;
			  } epoll_data_t;
			  ```
			- epoll_data_t是一个联合体，其4个成员中使用最多的是fd，它指定事件所从属的目标文件描述符。ptr成员可用来指定与f(相关的用户数据。但由于epoll_data_t是一个联合体，我们不能同时使用其ptr成员和fd成员，因此，如果要将文件描述符和用户数据关联起来(正如8.5.2小节讨论的将句柄和事件处理器绑定一样)，以实现快速的数据访问，只能使用其他手段，比如放弃使用epoll_data_t的fd成员，而在ptr指向的用户数据中包含fd。
			  epoll_ctl成功时返回0，失败则返回-1并设置errno.
			-
		- `epoll_wait` 函数
		  collapsed:: true
			- epoll系列系统调用的主要接口是epoll_wait 函数。它在一段超时时间内等待一组文件描述符上的事件，其原型如下:
			- ```c
			  #include <sys/epoll.h>
			  int epoll_wait( int epfd, struct epoll_event* events, int maxevents,
			                int timeout );
			  ```
			- 该函数成功时返回就绪的文件描述符的个数，失败时返回-1并设置errno.
			  关于该函数的参数，我们从后往前讨论。timeout参数的含义与poll接口的timeout参数相同。maxevents参数指定最多监听多少个事件，它必须大于0。
			- epoll_wait函数如果检测到事件，就将所有就绪的事件从内核事件表（由epfd参数指定）中复制到它的第二个参数events指向的数组中。这个数组只用于输出 epoll_wait检测到的就绪事件，而不像select和poll 的数组参数那样既用于传入用户注册的事件，又用于输出内核检测到的就绪事件。这就极大地提高了应用程序索引就绪文件描述符的效率。代码清单9-2体现了这个差别。
			- ![image.png](../assets/image_1659333712198_0.png)
			-
		- `LT` 和 `ET` 模式
		  collapsed:: true
			- epoll对文件描述符的操作有两种模式:LT (Level Trigger，电平触发〉模式和ET（EdgeTrigger，边沿触发〉模式。LT模式是默认的工作模式，这种模式下epoll相当于一个效率较高的poll。当往 epoll内核事件表中注册一个文件描述符上的EPOLLET事件时，epoll将以ET模式来操作该文件描述符。ET模式是epoll 的高效工作模式。
			  对于采用LT工作模式的文件描述符，当epoll_wait检测到其上有事件发生并将此事件通知应用程序后，应用程序可以不立即处理该事件。这样，当应用程序下一次调用epoll_wait时，epoll_wait还会再次向应用程序通告此事件，直到该事件被处理。而对于采用ET工作模式的文件描述符，当epoll_wait检测到其上有事件发生并将此事件通知应用程序后，应用程序必须立即处理该事件，因为后续的epoll_wait调用将不再向应用程序通知这一事件。可见，ET模式在很大程度上降低了同一个epoll事件被重复触发的次数，因此效率要比LT模式高。代码清单9-3体现了LT和ET在工作方式上的差异。
			- ```c
			  #include <sys/socket.h>
			  #include <netinet/in.h>
			  #include <arpa/inet.h>
			  #include <assert.h>
			  #include <stdio.h>
			  #include <unistd.h>
			  #include <errno.h>
			  #include <string.h>
			  #include <fcntl.h>
			  #include <stdlib.h>
			  #include <sys/epoll.h>
			  #include <pthread.h>
			  
			  #define MAX_EVENT_NUMBER 1024
			  #define BUFFER_SIZE 10
			  // 将文件描述符设置成非阻塞的
			  int setnonblocking(int fd)
			  {
			      int old_option = fcntl(fd, F_GETFL);
			      int new_option = old_option | O_NONBLOCK;
			      fcntl(fd, F_SETFL, new_option);
			      return old_option;
			  }
			  // 将文件描述符fd上的EPOLLIN注册到epollfd指示的epoll内核事件表中，参数enable_et指定是否堆fd启用ET模式
			  void addfd(int epollfd, int fd, bool enable_et)
			  {
			      epoll_event event;
			      event.data.fd = fd;
			      event.events = EPOLLIN;
			      if (enable_et)
			      {
			          event.events |= EPOLLET;
			      }
			      epoll_ctl(epollfd, EPOLL_CTL_ADD, fd, &event);
			      setnonblocking(fd);
			  }
			  // LT模式的工作流程
			  void lt(epoll_event *events, int number, int epollfd, int listenfd)
			  {
			      char buf[BUFFER_SIZE];
			      for (int i = 0; i < number; i++)
			      {
			          int sockfd = events[i].data.fd;
			          if (sockfd == listenfd)
			          {
			              struct sockaddr_in client_address;
			              socklen_t client_addrlength = sizeof(client_address);
			              int connfd = accept(listenfd, (struct sockaddr *)&client_address, &client_addrlength);
			              addfd(epollfd, connfd, false);
			          }
			          else if (events[i].events & EPOLLIN)
			          {
			              // 只要socket读缓存中还有未读出的数据，这段代码就被触发
			              printf("event trigger once\n");
			              memset(buf, '\0', BUFFER_SIZE);
			              int ret = recv(sockfd, buf, BUFFER_SIZE - 1, 0);
			              if (ret <= 0)
			              {
			                  close(sockfd);
			                  continue;
			              }
			              printf("get %d bytes of content: %s\n", ret, buf);
			          }
			          else
			          {
			              printf("something else happened \n");
			          }
			      }
			  }
			  void et(epoll_event *events, int number, int epollfd, int listenfd)
			  {
			      char buf[BUFFER_SIZE];
			      for (int i = 0; i < number; i++)
			      {
			          int sockfd = events[i].data.fd;
			          if (sockfd == listenfd)
			          {
			              struct sockaddr_in client_address;
			              socklen_t client_addrlength = sizeof(client_address);
			              int connfd = accept(listenfd, (struct sockaddr *)&client_address, &client_addrlength);
			              addfd(epollfd, connfd, true); // 对connfd开启ET模式
			          }
			          else if (events[i].events & EPOLLIN)
			          {
			              // 这段代码不会被重复触发，所以我们循环读取数据，以确保把socket读缓存中的所有数据读出
			              printf("event trigger once\n");
			              while (1)
			              {
			                  memset(buf, '\0', BUFFER_SIZE);
			                  int ret = recv(sockfd, buf, BUFFER_SIZE - 1, 0);
			                  if (ret < 0)
			                  {
			                      // 对于非阻塞IO,下面的条件成立表示数据已经全部读取完毕。此后，epoll就能再次触发sockfd上的EPOLLIN事件，以驱动下一次读操作
			                      if ((errno == EAGAIN) || (errno == EWOULDBLOCK))
			                      {
			                          printf("read later\n");
			                          break;
			                      }
			                      close(sockfd);
			                      break;
			                  }
			                  else if (ret == 0)
			                  {
			                      close(sockfd);
			                  }
			                  else
			                  {
			                      printf("get %d bytes of content: %s\n", ret, buf);
			                  }
			              }
			          }
			          else
			          {
			              printf("something else happened\n");
			          }
			      }
			  }
			  
			  int main(int argc, char *argv[])
			  {
			      if (argc <= 2)
			      {
			          printf("usage: %s ip_address port_number\n", basename(argv[0]));
			          return 1;
			      }
			      const char *ip = argv[1];
			      int port = atoi(argv[2]);
			  
			      int ret = 0;
			      struct sockaddr_in address;
			      bzero(&address, sizeof(address));
			      address.sin_family = AF_INET;
			      inet_pton(AF_INET, ip, &address.sin_addr);
			      address.sin_port = htons(port);
			  
			      int listenfd = socket(PF_INET, SOCK_STREAM, 0);
			      assert(listenfd >= 0);
			  
			      ret = bind(listenfd, (struct sockaddr *)&address, sizeof(address));
			      assert(ret != -1);
			  
			      ret = listen(listenfd, 5);
			      assert(ret != -1);
			  
			      epoll_event events[MAX_EVENT_NUMBER];
			      int epollfd = epoll_create(5);
			      assert(epollfd != -1);
			      addfd(epollfd, listenfd, true);
			  
			      while (1)
			      {
			          int ret = epoll_wait(epollfd, events, MAX_EVENT_NUMBER, -1);
			          if (ret < 0)
			          {
			              printf("epoll failure\n");
			              break;
			          }
			          lt(events, ret, epollfd, listenfd); // 使用LT模式
			          // et(events, ret, epollfd, listenfd); // ET模式
			      }
			      close(listenfd);
			      return 0;
			  }
			  
			  ```
			- 使用telnet到这个服务器程序上并一次传输超过10字节(BUFFER_SIZE的大小）的数据，然后比较LT模式和ET模式的异同。你会发现，正如我们预期的，ET模式下事件被触发的次数要比LT模式下少很多。
			- > 注意每个使用ET模式的文件描述符都应该是非阻塞的。如果文件描述符是阻塞的，那么读或写操作将会因为没有后续的事件而一直处于阻塞状态（饥渴状态)。
			-
		- `EPOLLONESHOT` 事件
		  collapsed:: true
			- 即使我们使用ET模式，一个socket 上的某个事件还是可能被触发多次。这在并发程序中就会引起一个问题。比如-一个线程（或进程，下同〉在读取完某个socket上的数据后开始处理这些数据，而在数据的处理过程中该socket上又有新数据可读（EPOLLIN再次被触发)，此时另外一个线程被唤醒来读取这些新的数据。于是就出现了两个线程同时操作一个socket的局面。这当然不是我们期望的。我们期望的是一个socket连接在任一时刻都只被一个线程处理。这一点可以使用epoll 的 EPOLLONESHOT事件实现。
			  对于注册了EPOLLONESHOT事件的文件描述符，操作系统最多触发其上注册的一个可读、可写或者异常事件，且只触发一次，除非我们使用epoll_ctl函数重置该文件描述符上注册的EPOLLONESHOT事件。这样，当一个线程在处理某个socket时，其他线程是不可能有机会操作该socket的。但反过来思考，注册了EPOLLONESHOT事件的socket一旦被某个线程处理完毕，该线程就应该立即重置这个socket 上的EPOLLONESHOT事件，以确保这个socket下一次可读时，其EPOLLIN事件能被触发，进而让其他工作线程有机会继续处理这个socket。
			- ```c
			  #include <sys/socket.h>
			  #include <netinet/in.h>
			  #include <arpa/inet.h>
			  #include <assert.h>
			  #include <stdio.h>
			  #include <unistd.h>
			  #include <errno.h>
			  #include <string.h>
			  #include <fcntl.h>
			  #include <stdlib.h>
			  #include <sys/epoll.h>
			  #include <pthread.h>
			  
			  #define MAX_EVENT_NUMBER 1024
			  #define BUFFER_SIZE 10
			  
			  struct fds
			  {
			      int epollfd;
			      int sockfd;
			  };
			  
			  // 将文件描述符设置成非阻塞的
			  int setnonblocking(int fd)
			  {
			      int old_option = fcntl(fd, F_GETFL);
			      int new_option = old_option | O_NONBLOCK;
			      fcntl(fd, F_SETFL, new_option);
			      return old_option;
			  }
			  /* 将fd上的EPOLLIN和EPOLLET事件注册到epollfd指示的epoll内核事件表中，
			      参数oneshot指定是否注册fd上的EPOLLONESHOT事件 */
			  void addfd(int epollfd, int fd, bool oneshot)
			  {
			      epoll_event event;
			      event.data.fd = fd;
			      event.events = EPOLLIN | EPOLLET;
			      if (oneshot)
			      {
			          event.events |= EPOLLONESHOT;
			      }
			      epoll_ctl(epollfd, EPOLL_CTL_ADD, fd, &event);
			      setnonblocking(fd);
			  }
			  /* 充值fd上的事件。这样操作之后，尽管fd上的EPOLLONESHOT事件被注册，但是操作系统
			      仍然会触发fd上的EPOLLIN事件，且只触发一次 */
			  void reset_oneshot(int epollfd, int fd)
			  {
			      epoll_event event;
			      event.data.fd = fd;
			      event.events = EPOLLIN | EPOLLET | EPOLLONESHOT;
			      epoll_ctl(epollfd, EPOLL_CTL_MOD, fd, &event);
			  }
			  // 工作线程
			  void *worker(void *arg)
			  {
			      int sockfd = ((fds *)arg)->sockfd;
			      int epollfd = ((fds *)arg)->epollfd;
			      printf("start new thread to receive data on fd: %d\n", sockfd);
			      char buf[BUFFER_SIZE];
			      memset(buf, '\0', BUFFER_SIZE);
			      while (1)
			      {
			          int ret = recv(sockfd, buf, BUFFER_SIZE - 1, 0);
			          if (ret == 0)
			          {
			              close(sockfd);
			              printf("foreiner closed the connection\n");
			              break;
			          }
			          else if (ret < 0)
			          {
			              if (errno == EAGAIN)
			              {
			                  reset_oneshot(epollfd, sockfd);
			                  printf("read later\n");
			                  break;
			              }
			          }
			          else
			          {
			              printf("get content: %s\n", buf);
			              // 休眠5s，模拟数据处理过程
			              sleep(5);
			          }
			      }
			      printf("end thread reveiving data on fd: %d\n", sockfd);
			  }
			  
			  int main(int argc, char *argv[])
			  {
			      if (argc <= 2)
			      {
			          printf("usage: %s ip_address port_number\n", basename(argv[0]));
			          return 1;
			      }
			      const char *ip = argv[1];
			      int port = atoi(argv[2]);
			  
			      int ret = 0;
			      struct sockaddr_in address;
			      bzero(&address, sizeof(address));
			      address.sin_family = AF_INET;
			      inet_pton(AF_INET, ip, &address.sin_addr);
			      address.sin_port = htons(port);
			  
			      int listenfd = socket(PF_INET, SOCK_STREAM, 0);
			      assert(listenfd >= 0);
			  
			      ret = bind(listenfd, (struct sockaddr *)&address, sizeof(address));
			      assert(ret != -1);
			  
			      ret = listen(listenfd, 5);
			      assert(ret != -1);
			  
			      epoll_event events[MAX_EVENT_NUMBER];
			      int epollfd = epoll_create(5);
			      assert(epollfd != -1);
			  
			      /* 注意，监听socket listenfd上是不能注册EPOLLONESHOT事件的，否则应用程序只能处理一个客户连接！
			      因为后续的客户连接请求将不再触发listenfd上的EPOLLIN事件 */
			      addfd(epollfd, listenfd, false);
			  
			      while (1)
			      {
			          int ret = epoll_wait(epollfd, events, MAX_EVENT_NUMBER, -1);
			          if (ret < 0)
			          {
			              printf("epoll failure\n");
			              break;
			          }
			          for (int i = 0; i < ret; i++)
			          {
			              int sockfd = events[i].data.fd;
			              if (sockfd == listenfd)
			              {
			                  struct sockaddr_in client_address;
			                  socklen_t client_addrlength = sizeof(client_address);
			                  int connfd = accept(listenfd, (struct sockaddr *)&client_address, &client_addrlength);
			                  // 对每个非监听文件描述符都注册EPOLLONESHOT事件
			                  addfd(epollfd, connfd, true);
			              }
			              else if (events[i].events & EPOLLIN)
			              {
			                  pthread_t thread;
			                  fds fds_for_new_worker;
			                  fds_for_new_worker.epollfd = epollfd;
			                  fds_for_new_worker.sockfd = sockfd;
			                  // 新启动一个工作线程未sockfd服务
			                  pthread_create(&thread, NULL, worker, (void *)&fds_for_new_worker);
			              }
			              else
			              {
			                  printf("something else happend \n");
			              }
			          }
			      }
			      close(listenfd);
			      return 0;
			  }
			  
			  ```
			- 从工作线程函数worker来看，如果一个工作线程处理完某个socket 上的一次请求（我们用休眠5s来模拟这个过程）之后，又接收到该socket上新的客户请求，则该线程将继续为这个socket服务。并且因为该socket上注册了EPOLLONESHOT事件，其他线程没有机会接触这个socket，如果工作线程等待5s后仍然没收到该socket上的下一批客户数据，则它将放弃为该socket服务。同时，它调用reset_oneshot 函数来重置该socket上的注册事件,这将使epoll有机会再次检测到该socket 上的EPOLLIN事件，进而使得其他线程有机会为该socket服务。
			  由此看来，尽管一个socket在不同时间可能被不同的线程处理，但同一时刻肯定只有一个线程在为它服务。这就保证了连接的完整性，从而避免了很多可能的竞态条件。
			-
	- #### 三组I/O复用函数比较
	  collapsed:: true
		- ![image.png](../assets/image_1659342573611_0.png)
		-
- [[Signal]] 信号
	- #### Linux信号概述
	  collapsed:: true
		- 发送信号
		  collapsed:: true
			- Linux 下，一个进程给其他进程发送信号的API是kill函数。其定义如下:
			- ```c
			  #include <sys/types.h>
			  #include <signal.h>
			  int kill( pid_t pid, int sig );
			  ```
			- 该函数把信号sig 发送给目标进程﹔目标进程由pid参数指定，其可能的取值及含义如表10-1所示。
			- ![image.png](../assets/image_1659421209270_0.png)
			- Linux定义的信号值都大于0，如果sig取值为0，则 kill函数不发送任何信号。但将sig设置为О可以用来检测目标进程或进程组是否存在，因为检查工作总是在信号发送之前就执行。不过这种检测方式是不可靠的。一方面由于进程PID的回绕，可能导致被检测的PID不是我们期望的进程的PID:另一方面，这种检测方法不是原子操作。
			- 该函数成功时返回0，失败则返回-1并设置errno。几种可能的errno如表10-2所示。
			- ![image.png](../assets/image_1659421257944_0.png)
			-
		- 信号处理方式
		  collapsed:: true
			- 目标进程在收到信号时，需要定义一-个接收函数来处理之。信号处理函数的原型如下:
			- ```c
			  #include <signal.h>
			  typedef void (*__sighandler_t)( int );
			  ```
			- 信号处理函数只带有一个整型参数，该参数用来指示信号类型。信号处理函数应该是可重入的，否则很容易引发一些竞态条件。所以在信号处理函数中严禁调用一些不安全的函数。
			  除了用户自定义信号处理函数外，bits/signum.h头文件中还定义了信号的两种其他处理方式----—SIG_IGN和SIG_DEL:
			- ```c
			  #include <bits/signum.h>
			  #define SIG_DFL ((__sighandler_t) 0)
			  #define SIG_IGN ((__sighandler_t) 1)
			  ```
			- SIG_IGN表示忽略目标信号，SIG_DFL表示使用信号的默认处理方式。信号的默认处理方式有如下几种:结束进程（Term)、忽略信号(Ign)、结束进程并生成核心转储文件(Core)、暂停进程（Stop)，以及继续进程(Cont）。
			-
		- Linux信号
		  collapsed:: true
			- Linux 的可用信号都定义在 bits/signum.h头文件中，其中包括标准信号和POSIX实时信号。本书仅讨论标准信号，如表10-3所示。
			- ![image.png](../assets/image_1659421474733_0.png)
			  ![image.png](../assets/image_1659421495162_0.png)
			-
		- 中断系统调用
		  collapsed:: true
			- 如果程序在执行处于阻塞状态的系统调用时接收到信号，并且我们为该信号设置了信号处理函数，则默认情况下系统调用将被中断，并且errno被设置为EINTR。我们可以使用sigaction函数（见后文）为信号设置SA_RESTART标志以自动重启被该信号中断的系统调用。
			  对于默认行为是暂停进程的信号（比如SIGSTOP、SIGTTIN)，如果我们没有为它们设置信号处理函数，则它们也可以中断某些系统调用（比如connect、epoll_wait)。POSIX没有规定这种行为，这是Linux独有的。
			-
	- #### 信号函数
	  collapsed:: true
		- `signal` 系统调用
		  collapsed:: true
			- 要为一个信号设置处理函数，可以使用下面的signal系统调用:
			- ```c
			  #include <signal.h>
			  _sighandler_t signal( int sig, _sighandler_t _handler )
			  ```
			- sig 参数指出要捕获的信号类型。_handler参数是_sighandler_t类型的函数指针，用于指定信号sig 的处理函数。
			- signal函数成功时返回一个函数指针，该函数指针的类型也是_sighandler_t。这个返回值是前一次调用signal函数时传入的函数指针，或者是信号sig对应的默认处理函数指针sIG_DEF（如果是第一次调用signal的话)。
			- signal系统调用出错时返回SIG_ERR，并设置errno.
		- `sigaction` 系统调用
		  collapsed:: true
			- 设置信号处理函数的更健壮的接口是如下的系统调用:
			- ```c
			  #include <signal.h>
			  int sigaction( int sig, const struct sigaction* act, struct sigaction* oact );
			  ```
			- sig 参数指出要捕获的信号类型，act参数指定新的信号处理方式，oact参数则输出信号先前的处理方式（如果不为NULL的话)。act和 oact都是sigaction结构体类型的指针,sigaction结构体描述了信号处理的细节，其定义如下:
			- ```c
			  struct sigaction
			  {
			    #ifdef __USE_POSIX199309
			    	union
			      {
			        _sighandler_t sa_handler;
			        void (*sa_sigaction) (int, siginfo_t*, void*);
			      }
			    	_sigaction_handler;
			    #define sa_handler 	__sigaction_handler.sa_handler
			    #define sa_sigaction  __sigaction_handler.sa_sigaction
			    #else
			    	_sighandler_t sa_handler;
			    #endif
			    	_sigset_t sa_mask;
			    	int sa_flags;
			    	void (*sa_restorer)(void);
			  };
			  ```
			- 该结构体中的sa_hander成员指定信号处理函数。sa_mask成员设置进程的信号掩码(确切地说是在进程原有信号掩码的基础上增加信号掩码)，以指定哪些信号不能发送给本进程。sa_mask是信号集sigset_t (_sigset_t的同义词)类型，该类型指定一组信号。关于信号集，我们将在后面介绍。sa_flags成员用于设置程序收到信号时的行为，其可选值如表10-4所示。
			- ![image.png](../assets/image_1659422443618_0.png)
			- sa_restorer成员已经过时，最好不要使用。sigaction成功时返回0，失败则返回-1并设置errno.
			-
	- #### 信号集
	  collapsed:: true
		- 信号集函数
		  collapsed:: true
			- 前文提到，Linux使用数据结构sigset_t来表示一组信号。其定义如下:
			- ```c
			  #include <bits/sigset.h>
			  #define _SIGSET_NWORDS(1024/(8 * sizeof(unsigned long int)))
			  typedef struct 
			  {
			    unsigned long int __val[_SIGSET_NWORDS];
			  } __sigset_t;
			  ```
			- 由该定义可见，sigset_t实际上是一个长整型数组，数组的每个元素的每个位表示一个信号。这种定义方式和文件描述符集fd_set类似。Linux提供了如下一组函数来设置、修改、删除和查询信号集:
			- ```c
			  #include <signal.h>
			  int sigemptyset( sigset_t* _set )				// 清空信号集
			  int sigfillset( sigset_t* _set )				// 在信号集中设置所有信号
			  int sigaddset( sigset_t* _set, int _signo )		// 将信号_signo添加到信号集中
			  int sigdelset( sigset_t* _set, int _signo )		// 将信号_signo从信号集中删除
			  int sigismember( const sigset_t* _set, int _signo )	// 测试_signo是否在信号集中
			  ```
		- 进程信号掩码
		  collapsed:: true
			- 前文提到，我们可以利用sigaction结构体的sa_mask成员来设置进程的信号掩码。此外，如下函数也可以用于设置或查看进程的信号掩码:
			- ```c
			  #include <signal.h>
			  int sigprocmask( int _how, const sigset_t* _set, sigset_t* _oset );
			  ```
			- _set参数指定新的信号掩码，_oset参数则输出原来的信号掩码（如果不为NULL的话)。如果_set参数不为NULL，则_how参数指定设置进程信号掩码的方式，其可选值如表10-5所示。
			  ![image.png](../assets/image_1659423407026_0.png)
			- 如果_set为 NULL，则进程信号掩码不变，此时我们仍然可以利用_oset参数来获得进程当前的信号掩码。
			- sigprocmask 成功时返回0，失败则返回-1并设置errno.
			-
		- 被挂起的信号
		  collapsed:: true
			- 设置进程信号掩码后，被屏蔽的信号将不能被进程接收。如果给进程发送一个被屏蔽的信号，则操作系统将该信号设置为进程的一个被挂起的信号。如果我们取消对被挂起信号的屏蔽，则它能立即被进程接收到。如下函数可以获得进程当前被挂起的信号集:
			- ```c
			  #include <signal.h>
			  int sigpending( sigset_t* set );
			  ```
			- set参数用于保存被挂起的信号集。显然，进程即使多次接收到同一个被挂起的信号,sigpending 函数也只能反映一次。并且，当我们再次使用sigprocmask使能该挂起的信号时，该信号的处理函数也只被触发一次。
			  sigpending成功时返回0，失败时返回-1并设置errno。
		- 统一事件源
		  collapsed:: true
			- 信号是一种异步事件:信号处理函数和程序的主循环是两条不同的执行路线。很显然，信号处理函数需要尽可能快地执行完毕，以确保该信号不被屏蔽（前面提到过，为了避免一些竞态条件，信号在处理期间，系统不会再次触发它〉太久。一种典型的解决方案是:把信号的主要处理逻辑放到程序的主循环中，当信号处理函数被触发时，它只是简单地通知主循环程序接收到信号，并把信号值传递给主循环，主循环再根据接收到的信号值执行目标信号对应的逻辑代码。信号处理函数通常使用管道来将信号“传递”给主循环:信号处理函数往管道的写端写人信号值，主循环则从管道的读端读出该信号值。那么主循环怎么知道管道上何时有数据可读呢﹖这很简单，我们只需要使用Io复用系统调用来监听管道的读端文件描述符上的可读事件。如此一来，信号事件就能和其他IO事件一样被处理，即统一事件源。
			  很多优秀的I/O框架库和后台服务器程序都统一处理信号和IO事件，比如Libevent I/O框架库和xinetd超级服务。代码清单10-1给出了统一事件源的一个简单实现。
			- ```c
			  #include <sys/types.h>
			  #include <sys/socket.h>
			  #include <netinet/in.h>
			  #include <arpa/inet.h>
			  #include <assert.h>
			  #include <stdio.h>
			  #include <unistd.h>
			  #include <errno.h>
			  #include <string.h>
			  #include <fcntl.h>
			  #include <stdlib.h>
			  #include <sys/epoll.h>
			  #include <pthread.h>
			  #include <signal.h>
			  
			  #define MAX_EVENT_NUMBER 1024
			  static int pipefd[2];
			  
			  int setnonblocking(int fd)
			  {
			      int old_option = fcntl(fd, F_GETFL);
			      int new_option = old_option | O_NONBLOCK;
			      fcntl(fd, F_SETFL, new_option);
			      return old_option;
			  }
			  
			  void addfd(int epollfd, int fd)
			  {
			      epoll_event event;
			      event.data.fd = fd;
			      event.events = EPOLLIN | EPOLLET;
			      epoll_ctl(epollfd, EPOLL_CTL_ADD, fd, &event);
			      setnonblocking(fd);
			  }
			  // 信号处理函数
			  void sig_handler(int sig)
			  {
			      // 保留原来的errno，在函数最后恢复，以保证函数的可重入性
			      int save_errno = errno;
			      int msg = sig;
			      send(pipefd[1], (char *)&msg, 1, 0); // 将信号值写入管道，以通知主循环
			      errno = save_errno;
			  }
			  // 设置信号的处理函数
			  void addsig(int sig)
			  {
			      struct sigaction sa;
			      memset(&sa, '\0', sizeof(sa));
			      sa.sa_handler = sig_handler;
			      sa.sa_flags |= SA_RESTART;
			      sigfillset(&sa.sa_mask);
			      assert(sigaction(sig, &sa, NULL) != -1);
			  }
			  
			  int main(int argc, char *argv[])
			  {
			      if (argc <= 2)
			      {
			          printf("usage: %s ip_address port_number\n", basename(argv[0]));
			          return 1;
			      }
			      const char *ip = argv[1];
			      int port = atoi(argv[2]);
			  
			      int ret = 0;
			      struct sockaddr_in address;
			      bzero(&address, sizeof(address));
			      address.sin_family = AF_INET;
			      inet_pton(AF_INET, ip, &address.sin_addr);
			      address.sin_port = htons(port);
			  
			      int listenfd = socket(PF_INET, SOCK_STREAM, 0);
			      assert(listenfd >= 0);
			  
			      ret = bind(listenfd, (struct sockaddr *)&address, sizeof(address));
			      if (ret == -1)
			      {
			          printf("errno is %d\n", errno);
			          return 1;
			      }
			      ret = listen(listenfd, 5);
			      assert(ret != -1);
			  
			      epoll_event events[MAX_EVENT_NUMBER];
			      int epollfd = epoll_create(5);
			      assert(epollfd != -1);
			      addfd(epollfd, listenfd);
			  
			      // 使用socketpair创建管道，注册pipefd[0]上的可读事件
			      ret = socketpair(PF_UNIX, SOCK_STREAM, 0, pipefd);
			      assert(ret != -1);
			      setnonblocking(pipefd[1]);
			      addfd(epollfd, pipefd[0]);
			  
			      // 设置一些信号的处理函数
			      addsig(SIGHUP);
			      addsig(SIGCHLD);
			      addsig(SIGTERM);
			      addsig(SIGINT);
			      bool stop_server = false;
			  
			      while (!stop_server)
			      {
			          int number = epoll_wait(epollfd, events, MAX_EVENT_NUMBER, -1);
			          if ((number < 0) && (errno != EINTR))
			          {
			              printf("epoll failure\n");
			              break;
			          }
			          for (int i = 0; i < number; i++)
			          {
			              int sockfd = events[i].data.fd;
			              // 如果就绪的文件描述符是listenfd,则处理新的连接
			              if (sockfd == listenfd)
			              {
			                  struct sockaddr_in client_address;
			                  socklen_t client_addrlength = sizeof(client_address);
			                  int connfd = accept(listenfd, (struct sockaddr *)&client_address, &client_addrlength);
			                  addfd(epollfd, connfd);
			              }
			              // 如果就绪的文件描述符是pipefd[0]，则处理信号
			              else if ((sockfd == pipefd[0]) && (events[i].events & EPOLLIN))
			              {
			                  int sig;
			                  char signals[1024];
			                  ret = recv(pipefd[0], signals, sizeof(signals), 0);
			                  if (ret == -1)
			                  {
			                      continue;
			                  }
			                  else if (ret == 0)
			                  {
			                      continue;
			                  }
			                  else
			                  {
			                      /* 因为每个信号值占1个字节，所以按字节来逐个接收信号，我们以SIGTERM
			                      为例，来说明如何安全地终止服务器主循环 */
			                      for (int i = 0; i < ret; ++i)
			                      {
			                          switch (signals[i])
			                          {
			                          case SIGCHLD:
			                          case SIGHUP:
			                              continue;
			                          case SIGTERM:
			                          case SIGINT:
			                          {
			                              stop_server = true;
			                          }
			                          }
			                      }
			                  }
			              }
			              else
			              {
			              }
			          }
			          printf("close fds\n");
			          close(listenfd);
			          close(pipefd[1]);
			          close(pipefd[0]);
			          return 0;
			      }
			  }
			  ```
		- 网络编程相关信号
		  collapsed:: true
			- `SIGHUP`
				- 当挂起进程的控制终端时，SIGHUP信号将被触发。对于没有控制终端的网络后台程序而言，它们通常利用SIGHUP信号来强制服务器重读配置文件。一个典型的例子是xinetd超级服务程序。
				  xinetd程序在接收到SIGHUP信号之后将调用hard_reconfig函数（见xinetd源码),它循环读取/etc/xinetd.d/目录下的每个子配置文件，并检测其变化。如果某个正在运行的子服务的配置文件被修改以停止服务，则xinetd主进程将给该子服务进程发送SIGTERM信号以结束它。如果某个子服务的配置文件被修改以开启服务，则xinetd将创建新的socket并将其绑定到该服务对应的端口上。
			- `SIGPIPE`
				- 默认情况下，往一个读端关闭的管道或socket连接中写数据将引发SIGPIPE信号。我们需要在代码中捕获并处理该信号，或者至少忽略它，因为程序接收到SIGPIPE信号的默认行为是结束进程，而我们绝对不希望因为错误的写操作而导致程序退出。引起 SIGPIPE信号的写操作将设置crrno 为EPIPE。
				  第5章提到，我们可以使用send函数的MSG_NOSIGNAL标志来禁止写操作触发SIGPIPE信号。在这种情况下，我们应该使用send函数反馈的errno值来判断管道或者socket连接的读端是否已经关闭。
				  此外，我们也可以利用IO复用系统调用来检测管道和socket连接的读端是否已经关闭。以poll为例，当管道的读端关闭时，写端文件描述符上的 POLLHUP事件将被触发﹔当socket连接被对方关闭时，socket 上的 POLLRDHUP事件将被触发。
			- `SIGURG`
				- 在Linux环境下，内核通知应用程序带外数据到达主要有两种方法:一种是第9章介绍的I/O复用技术，select等系统调用在接收到带外数据时将返回，并向应用程序报告socket上的异常事件，代码清单9-1给出了一个这方面的例子﹔另外一种方法就是使用SIGURG信号，如代码清单10-3所示。
- 定时器
  collapsed:: true
	- socket选项 `SO_RCVTIMEO` 和 `SO_SNDTIMEO`
		- ![image.png](../assets/image_1659432284581_0.png)
		- 由表11-1可见，在程序中，我们可以根据系统调用(send、sendmsg、recv、recvmsg.accept和 connect）的返回值以及errno来判断超时时间是否已到，进而决定是否开始处理定时任务。
	- `SIGALRM` 信号
		-
- [[进程]]
	- #### [[fork]]
	  collapsed:: true
		- Linux下创建新进程的系统调用是fork.其定义如下:
		- #include <sys/types.h>
		  #include <unistd.h>
		  pid_t fork( void );
		- 该函数的每次调用都返回两次，在父进程中返回的是子进程的PID，在子进程中则返回
		  0.该返回值是后续代码判断当前进程是父进程还是子进程的依据。fork 调用失败时返回-1,并设置errmo。
		- fork函数复制当前进程，在内核进程表中创建一个新的进程表项。 新的进程表项有很多
		  属性和原进程相同，比如堆指针、栈指针和标志寄存器的值。但也有许多属性被赋予了新的值，比如该进程的PPID被设置成原进程的PID,信号位图被清除(原进程设置的信号处理函数不再对新进程起作用)。
		- 子进程的代码与父进程完全相同，同时它还会复制父进程的数据(堆数据、栈数据和静
		  态数据)。数据的复制采用的是所谓的写时复制(copy on witte),即只有在任- -进程(父进
		  程或子进程)对数据执行了写操作时，复制才会发生(先是缺页中断，然后操作系统给子进程分配内存并复制父进程的数据)。即便如此，如果我们在程序中分配了大量内存，那么使用fork时也应当十分谨慎，尽量避免没必要的内存分配和数据复制。
		- 此外，创建子进程后，父进程中打开的文件描述符默认在子进程中也是打开的，且文件描述符的引用计数加1.不仅如此，父进程的用户根目录、当前工作目录等变量的引用计数均会加1.
	- #### [[exec]]
		- 有时我们需要在子进程中执行其他程序，即替换当前进程映像，这就需要使用如下exee 系列函数之一:
		- ```c
		  #include <unistd.h>
		  extern char** environ;
		    
		  int execl( const char* path, const char* arg, ... );
		  int execlp( const char* file, const char* arg, ... );
		  int execle( const char* path, const char* arg, ..., char* const envp[] );
		  int execv( const char* path, char* const argv[] ); 
		  int execvp( const char* file, char* const argvl] );
		  int execve( const char* path, char* const argv[], char* const envp[] );
		  ```
		- path参数指定可执行文件的完整路径，file 参数可以接受文件名，该文件的具体位置则
		  在环境变量PATH中搜寻。arg 接受可变参数，argv 则接受参数数组，它们都会被传递给新程序(path或file指定的程序)的main兩数。envp参数用于设置新程序的环境变量。如果未设置它，则新程序将使用由全局变量environ指定的环境变量。
		- -般情况下，exec函数是不返回的，除非出错。它出错时返回-1,并设置
		  errno。如果没出错，则原程序中exec调用之后的代码都不会执行，因为此时原程序已经被exec的参数指定的程序完全替换(包括代码和数据)。
		  exec函数不会关闭原程序打开的文件描述符，除非该文件描述符被设置了类似SOCK_
		  CLOEXEC的属性。
	- #### 处理僵尸进程
	  collapsed:: true
		- 对于多进程程序而言，父进程-般需要跟踪子进程的退出状态。因此，当子进程结束运
		  行时，内核不会立即释放该进程的进程表表项，以满足父进程后续对该子进程退出信息的查询(如果父进程还在运行)。在子进程结束运行之后，父进程读取其退出状态之前，我们称该子进程处于僵尸态。另外-一种使子进程进入僵尸态的情况是:父进程结束或者异常终止，而子进程继续运行。此时子进程的PPID将被操作系统设置为1，即init 进程。init 进程接管了该子进程，并等待它结束。在父进程退出之后，子进程退出之前，该子进程处于僵尸态。
		- 由此可见，无论哪种情况，如果父进程没有正确地处理子进程的返回信息，子进程都将
		  停留在僵尸态，并占据着内核资源。这是绝对不能容许的，毕竟内核资源有限。下面这对函数在父进程中调用，以等待子进程的结束，并获取子进程的返回信息，从而避免了僵尸进程的产生，或者使子进程的僵尸态立即结束:
		- ```c
		  #include <sys/types.h>
		  #include <sys/wait.h>
		  pid_t wait( int* stat_loc );
		  pid_t waitpid( pid_t pid, int* stat_loc, int options );  
		  ```
		- wait函数将阻塞进程，直到该进程的某个子进程结束运行为止。它返回结束运行的子进程的PID,并将该子进程的退出状态信息存储于stat_ loc 参数指向的内存中。sys/wait.h 头文
		  件中定义了几个宏来帮助解释子进程的退出状态信息，如表13-1所示。
		  ![image.png](../assets/image_1659514578982_0.png)
		- wait函数的阻塞特性显然不是服务器程序期望的，而waitpid函数解决了这个问题。
		  waitpid只等待由pid参数指定的子进程。如果pid取值为-1，那么它就和wait函数相同，即
		  等待任意-一个子进程结束。stat_ loc 参数的含义和wait函数的stat_ loc 参数相同。options 参
		  数可以控制waitpid函数的行为。该参数最常用的取值是WNOHANG。当options的取值是
		  WNOHANG时，waitpid调用将是非阻塞的:如果pid指定的目标子进程还没有结束或意外
		  终止，则waitpid立即返回0;如果目标子进程确实正常退出了，则waitpid返回该子进程的
		  PID. waitpid 调用失败时返回-1并设置ermo.
		  8.3节曾提到，要在事件已经发生的情况下执行非阻塞调用才能提高程序的效率。对
		  waitpid函数而言，我们最好在某个子进程退出之后再调用它。那么父进程从何得知某个子进程已经退出了呢?这正是SIGCHLD信号的用途。当一个进程结束时，它将给其父进程发送
		  一个SIGCHLD信号。因此，我们可以在父进程中捕获SIGCHLD信号，并在信号处理函数
		  中调用waitpid函数以“彻底结束”一个子进程，如代码清单13-1 所示。
		  ![image.png](../assets/image_1659514652020_0.png)
		-
	- #### 管道 [[pipe]]
	  collapsed:: true
		- 第6章中我们介绍过创建管道的系统调用pipe.我们也多次在代码中利用它来实现进程
		  内部的通信。实际上，管道也是父进程和子进程间通信的常用手段。
		  管道能在父、子进程间传递数据，利用的是fork调用之后两个管道文件描述符(fd[0]
		  和fd[1])都保持打开.一对这样的文件描述符只能保证父、子进程间一个方向的数据传输，父进程和子进程必须有- -个关闭fd[0],另-一个关闭fd[1]。比如，我们要使用管道实现从父进程向子进程写数据，就应该按照图13-1 所示来操作。
		- ![image.png](../assets/image_1659514690880_0.png)
		- 显然，如果要实现父、子进程之间的双向数据传输，就必须使用两个管道。第6章中我
		  们还介绍过，socket 编程接口提供了一个创建全双工管道的系统调用: socketpair. squid 服务
		  器程序(见第4章)就是利用socketpair创建管道，以实现在父进程和日志服务子进程之间
		  传递日志信息。
		-
	- #### [[信号量]]
		- 信号量原语
		  collapsed:: true
			- 当多个进程同时访问系统上的某个资源的时候，比如同时写- -个数据库的某条记录，或者同时修改某个文件，就需要考虑进程的同步问题，以确保任- -时刻只有- -个进程可以拥有对资源的独占式访问。通常，程序对共享资源的访问的代码只是很短的一段，但就是这一段代码引发了进程之间的竞态条件。我们称这段代码为关键代码段，或者临界区。对进程同步，也就是确保任-时刻只有- -个进程能进人关键代码段。要编写具有通用目的的代码，以确保关键代码段的独占式访问是非常困难的。有两个名为Dekker算法和Peterson算法的解决方案，它们试图从语言本身(不需要内核支持)解决并发问题。但它们依赖于忙等待，即进程要持续不断地等待某个内存位置状态的改变。这种方式下CPU利用率太低，显然是不可取的。
			  id:: 62ea313d-e71a-49d2-bd95-56014a166f44
			  Dijkstra提出的信号量(Semaphore) 概念是并发编程领域迈出的重要-一步。信号量是一种特殊的变量，它只能取自然数值并且只支持两种操作:等待(wait)和信号(signal)。 不过在Linux/UNIX中，“等待”和“信号”都已经具有特殊的含义，所以对信号量的这两种操作更常用的称呼是P、V操作。这两个字母来自于荷兰语单词passeren (传递，就好像进人临界区)和vrijgeven (释放，就好像退出临界区)。假设有信号量SV,则对它的P、V操作含义如下:
				- P(SV),如果SV的值大于0，就将它减1;如果sv的值为0,则挂起进程的执行。
				- V(SV),如果有其他进程因为等待SV而挂起，则唤醒之;如果没有，则将sv加1.
			- 信号量的取值可以是任何自然数。但最常用的、最简单的信号量是二进制信号量，它只能取0和1这两个值。本书仅讨论二进制信号量。使用二进制信号量同步两个进程,以确保关键代码段的独占式访问的-一个典型例子如图13-2所示。
			- 在图13-2中，当关键代码段可用时，二进制信号量SV的值为1,进程A和B都有机会进人关键代码段。如果此时进程A执行了P(SV)操作将SV减1，则进程B若再执行P(SV)操作就会被挂起。直到进程A离开关键代码段，并执行V(SV)操作将SV加1，关键代码段才重新变得可用。如果此时进程B因为等待SV而处于挂起状态，则它将被唤醒，并进入关键代码段。同样，这时进程A如果再执行P(SV)操作，则也只能被操作系统挂起以等待进程B退出关键代码段。
			  ![image.png](../assets/image_1659515319457_0.png)
			-
		- `semget`
		  collapsed:: true
			- semget系统调用创建一个新的信号量集， 或者获取-一个已经存在的信号量集。其定义如下:
			- ```c
			  #include <sys/sem.h>
			  int semget( key_t key, int num_sems, int sem_flags );
			  ```
			- key参数是-一个键值，用来标识一个全局唯- 的信号量集， 就像文件名全局唯-地标识一个文件- -样.要通过信号量通信的进程需要使用相同的键值来创建/获取该信号量。num_ sems参数指定要创建1获取的信号量集中信号量的数目。如果是创建信号量，则该值必须被指定:如果是获取已经存在的信号量，则可以把它设置为0。
			- sem_flags参数指定--组标志。它低端的9个比特是该信号量的权限，其格式和含义都与系统调用open的mode参数相同。此外，它还可以和IPC_ CREAT 标志做按位“或”运算以创建新的信号量集。此时即使信号量已经存在，semget 也不会产生错误。我们还可以联合使用IPC_CREAT和IPC_ _EXCL标志来确保创建一组新的、 唯- -的信号量集。在这种情况下，如果信号量集已经存在，则senget返回错误并设置errno为EEXIST.这种创建信号量的行为与用0. CREAT和O_ EXCL标志调用open来排他式地打开- -个文件相似。
			- semget成功时返回一个正整数值，它是信号量集的标识符: semget 失败时返回-1,并设置errno。
			  如果semget用于创建信号量集，则与之关联的内核数据结构体semid_ ds将被创建并初始化。semid_ _ds 结构体的定义如下:
			- ```c
			  #include <sys/sem.h>
			  // 该结构体用于描述IPC对象（信号量、共享内存和消息队列）的权限
			  struct ipc_perm
			  {
			    key_t key;			// 键值
			    uid_t uid;			// 所有者的有效用户ID
			    gid_t gid;			// 所有者的有效组ID
			    uid_t cuid;			// 创建者的有效用户ID
			    get_t cgid;			// 创建者的有效组ID
			    mode_t mode;			// 访问权限
			    						// 省略其他填充字段
			  }
			  
			  struct semid_ds
			  {
			    struct ipc_perm sem_perm;		// 信号量的操作权限
			    unsigned long int sem_nsems;	// 该信号量级中的信号量数目
			    time_t sem_otime;				// 最后一次调用semop的时间
			    time_t sem_ctime;				// 最后一次调用semctl的时间
			    								// 省略其他填充字段
			  }
			  ```
			- semget对semid_ ds结构体的初始化包括:
				- 将sem_perm.cuid 和sem_ perm.uid 设置为调用进程的有效用户ID.
				- 将sem_perm.cgid 和sem_ perm.gid 设置为调用进程的有效组ID.
				- 将sem_perm.mode 的最低9位设置为sem_ fags 参数的最低9位。
				- 将sem_nsems 设置为num_ sems。
				- 将sem_otime设置为0。
				- 将sem_ctime设置为当前的系统时间。
		- `semop`
		  collapsed:: true
			- semop系统调用改变信号量的值，即执行P、V操作。在讨论semop之前，我们需要先介绍与每个信号量关联的一些重要的内核变量:
			- ```c
			  unsigned short semval;		// 信号量的值
			  unsigned short semzcnt;		// 等待信号量值变为0的进程数量
			  unsigned short semncnt;		// 等待信号量值增加的进程数量
			  pid_t sempid;				// 最后一次执行semop操作的进程ID
			  ```
			- semop对信号量的操作实际上就是对这些内核变量的操作。semop 的定义如下:
			- ```c
			  #include <sys/sem.h>
			  int semop( int sem_id, struct sembuf* sem_ops, size_t num_sem_ops );
			  ```
			- sem_ id参数是由semget调用返回的信号量集标识符，用以指定被操作的目标信号量集。sem ops 参数指向-一个sembuf结构体类型的数组，sembuf 结构体的定义如下:
			  ```c
			  struct sembuf
			  {
			    unsigned short int sem_num;
			    short int sem_op;
			    short int sem_flg;
			  }
			  ```
			- 其中，sem_ num 成员是信号量集中信号量的编号，0表示信号量集中的第-一个信号量。
			  sem_ op成员指定操作类型，其可选值为正整数、0和负整数。每种类型的操作的行为又受到sem_ fg成员的影响。sem_flg 的可选值是IPC_ NOWAIT和SEM_UNDO。IPC_ NOWAIT的
			  含义是，无论信号量操作是否成功，semop 调用都将立即返回，这类似于非阻塞IO操作。
			  SEM_UNDO的含义是，当进程退出时取消正在进行的semop操作。具体来说，sem_op和
			  sem_flg 将按照如下方式来影响semop的行为: .
				- 如果sem_op大于0，则semop将被操作的信号量的值semval增加sem_op。该操作
				  要求调用进程对被操作信号量集拥有写权限。此时若设置了SEM_ _UNDO标志，则系
				  统将更新进程的semadj变量(用以跟踪进程对信号量的修改情况)。
				- 如果sem_ op 等于0，则表示这是-一个“等待0”(wait-for-zero) 操作。该操作要求调
				  用进程对被操作信号量集拥有读权限。如果此时信号量的值是0，则调用立即成功返
				  回。如果信号量的值不是0，则semop失败返回或者阻塞进程以等待信号量变为0.
				  在这种情况下，当IPC_ NOWAIT标志被指定时，semop 立即返回-一个错误，并设置
				  errmo为EAGAIN。如果未指定IPC _NOWAIT标志，则信号量的semzcnt值加1,进程被投人睡眠直到下列3个条件之一发生:信号量的值semval变为0,此时系统将该信号量的semzcnt值减1 ;被操作信号量所在的信号量集被进程移除，此时semop调用失败返回，errno 被设置为EIDRM ;调用被信号中断，此时semop调用失败返回，errno被设置为EINTR，同时系统将该信号量的semzcnt值减1。
				- 如果sem_ op小于0，则表示对信号量值进行减操作，即期望获得信号量。该操
				  作要求调用进程对被操作信号量集拥有写权限。如果信号量的值semval大于或等
				  于sem_op的绝对值，则semop操作成功，调用进程立即获得信号量，并且系统将
				  该信号量的semval值减去sem_ op的绝对值。此时如果设置了SEM_ UNDO标志，则系统将更新进程的semadj变量。如果信号量的值semval小于sem.op的绝对
				  值，则semop 失败返回或者阻塞进程以等待信号量可用。在这种情况下，当IPC_
				  NOWAIT标志被指定时，semop 立即返回一个错误，并设置erno为EAGAIN。如
				  果未指定IPC NOWAIT标志，则信号量的semncnt值加1.进程被投入睡眠直到下
				  列3个条件之- .发生: 信号量的值semval变得大于或等于sem. _op 的绝对值，此时
				  系统将该信号量的semncnt值减I,并将semval减去sem._op的绝对值，同时，如
				  果SEMUNDO标志被设置，则系统更新semadj变量;被操作信号量所在的信号
				  量集被进程移除，此时semop调用失败返回，errno 被设置为EIDRM;调用被信号
				  中断，此时semop调用失败返回，errno 被设置为EINTR,同时系统将该信号量的
				  semncnt值减1。
			- semop系统调用的第3个参数num_ sem_ ops指定要执行的操作个数，即sem. _ops 数组中
			  元素的个数。semop对数组sem_ops 中的每个成员按照数组顺序依次执行操作，并且该过程是原子操作，以避免别的进程在同一时刻按照不同的顺序对该信号集中的信号量执行semop.操作导致的竞态条件。
			- semop成功时返回0，失败则返回-1并设置errmo。失败的时候，sem_ ops 数组中指定的所有操作都不被执行。
			-
		- `semctl`
		  collapsed:: true
			- sermctl系统调用允许调用者对信号量进行直接控制。其定义如下:
			- ```c
			  #include <sys/sem.h>
			  int semctl( int sem_id, int sem_num, int command, ... );
			  ```
			- scm_id参数是由semget调用返回的信号量集标识符，用以指定被操作的信号量集。
			  sem_ num 参数指定被操作的信号量在信号量集中的编号。command 参数指定要执行的命令。有的命令需要调用者传递第4个参数。第4个参数的类型由用户自己定义，但sys/sem.h头文件给出了它的推荐格式，具体如下:
			- ```c
			  union semun
			  {
			    int val;					// 用于SETVAL命令
			    struct semid_ds*buf;		// 用于IPC_STAT和IPC_SET命令
			    unsigned short* array;	// 用于GETALL和SETALL命令
			    struct seminfo* __buf;	// 用于IPC_INFO命令
			  };
			  
			  struct seminfo
			  {
			    int semmap;				// Linux内核没有使用
			    int semmni;				// 系统最多可以拥有的信号量集数目
			    int semmns;				// 系统最多可以拥有的信号量数目
			    int semmnu;				// Linux内核没有使用
			    int semmsl;				// 一个信号量最多允许包含的信号量数据
			    int semopm;				// semop一次最多能执行的sem_op操作数目
			    int semume;				// Linux内核没有使用
			    int semusz;				// sem_undo结构体的大小
			    int semvmx;				// 最大允许的信号量值
			    // 最多允许的UNDO次数（带SEM_UNDO标志的semop操作的次数）
			    int semaem;
			  }
			  ```
			- semctl支持的所有命令如表13-2 所示。
			- ![image.png](../assets/image_1659516891259_0.png)
			- semctl成功时的返回值取决于command参数，如表13-2所示，semetl 失败时返回-1,
			  并设置errno.
			-
	- #### [[共享内存]]
		- `shmget` 系统调用
		  collapsed:: true
			- shmget系统调用创建- -段新的共享内存，或者获取一段已经存在的共享内存。其定义如下:
			- ```c
			  #include <sys/shm.h>
			  int shmget( key_t key, size_t size, int shmflg );
			  ```
			- 和semget系统调用- -样，key 参数是一个键值，用来标识一段全局唯一 的共 享内存。size参数指定共享内存的大小，单位是字节。如果是创建新的共享内存，则size值必须被指定。如果是获取已经存在的共享内存，则可以把size设置为0.
			- shmfg参数的使用和含义与semget系统调用的sem_ fags参数相同。不过shmget支持两个额外的标志一SHM_ HUGETLB和SHM_ NORESERVE.它们的含义如下:
				- SHM_ HUGETLB,类似于mmap的MAP_ HUGETLB标志，系统将使用“大页面”
				  来为共享内存分配空间。
				- SHM_ NORESERVE, 类似于mmap的MAP_ NORESERVE 标志，不为共享内存保留
				  交换分区(swap空间)。这样，当物理内存不足的时候，对该共享内存执行写操作将触发SIGSEGV信号。
			- shmget成功时返回-个正整数值，它是共享内存的标识符。shmget 失败时返回-1,并
			  设置errmo.
			  如果shmget用于创建共享内存，则这段共享内存的所有字节都被初始化为0，与之关联的内核数据结构shmid_ ds 将被创建并初始化。shmid_ ds 结构体的定义如下:
			- ```c
			  struct shmid_ds
			  {
			    struct ipc_perm shm_perm;			// 共享内存的操作权限
			    size_t shm_segsz;					// 共享内存大小，单位是字节
			    __time_t shm_atime;				// 对这段内存最后一次调用shmat的时间
			    __time_t shm_dtime;				// 对这段内存最后一次调用shmdt的时间
			    __time_t shm_ctime;				// 对这段内存最后一次调用shmctl的时间
			    __pid_t shm_cpid;					// 创建者的PID
			    __pid_t shm_lpid;					// 最后一次执行shmat或shmdt操作的进程的PID
			    shmatt_t shm_nattach;				// 目前关联到次内存的进程数量
			    /* 省略一些填充字段 */
			  }
			  ```
			- shmget对shmid_ ds 结构体的初始化包括:
				- 将shm_perm.cuid 和shm_perm.uid 设置为调用进程的有效用户ID.
				- 将shm_perm.cgid 和shm_perm.gid 设置为调用进程的有效组ID.
				- 将shm_perm.mode的最低9位设置为shmflg 参数的最低9位。
				- 将shm_ segsz设置为size.
				- 将shm_lpid、shm_ nattach、shm_atime、shm_dtime 设置为0。
				- 将shm_ctime 设置为当前的时间。
		- `shmat` 和 `shmdt` 系统调用
		  collapsed:: true
			- 共享内存被创建1获取之后，我们不能立即访问它，而是需要先将它关联到进程的地址
			  空间中。使用完共享内存之后，我们也需要将它从进程地址空间中分离。这两项任务分别由如下两个系统调用实现:
			- ```c
			  #include <sys/shm.h>
			  void* shmat( int shm_id, const void* shm_addr, int shmflg );
			  int shmdt( const void* shm_addr );
			  ```
			- 其中，shm_ jid 参数是由shmget调用返回的共享内存标识符。shm_ addr 参数指定将共享
			  内存关联到进程的哪块地址空间，最终的效果还受到shmfig参数的可选标志SHM _RND的
			  影响:
				- 如果shm_ addr 为NULL,则被关联的地址由操作系统选择。这是推荐的做法，以确保代码的可移植性.
				- 如果shm_addr 非空，并且SHM_ RND标志未被设置，则共享内存被关联到addr指定
				  的地址处。
				- 如果shm_ addr 非空，并且设置了SHM_RND标志，则被关联的地址是[shm_ addr-
				  (shm__addr%SHMLBA)]。SHMLBA的含义是“段低端边界地址倍数”(Segment
				  Low Boundary Address Multiple)，它必须是内存页面大小(PAGE_ SIZE)的整
				  数倍。现在的Linux内核中，它等于一个内存页大小。SHM_ RND的含义是圆整.
				  (round),即将共享内存被关联的地址向下圆整到离shm__addr最近的SHMLBA的
				  整数倍地址处。
			- 除了SHM_ rND标志外，shmfg 参数还支持如下标志:
				- SHM_ RDONLY.进程仅能读取共享内存中的内容。若没有指定该标志，则进程可同
				  时对共享内存进行读写操作( 当然，这需要在创建共享内存的时候指定其读写权限)。
				- SHM_ REMAP.如果地址shmaddr已经被关联到一段共享内存上，则重新关联。
				- SHM_ EXEC.它指定对共享内存段的执行权限。对共享内存而言，执行权限实际上
				  和读权限是一样的。
			- shmat成功时返回共享内存被关联到的地址，失败则返回(void*)-1并设置ermo。 shmat
			  成功时，将修改内核数据结构shmid_ds 的部分字段，如下
				- 将shm_ nattach 加1。
				- 将shm_ Jpid 设置为调用进程的PID.
				- 将shm_atime设置为当前的时间。
			- shmdt函数将关联到shm_ addr 处的共享内存从进程中分离。它成功时返回o,失败则返
			  回-1并设置ermno. shmdt 在成功调用时将修改内核数据结构shmid _ds的部分字段，如下:
				- 将shm_nattach 减1.
				- 将shm_pid 设置为调用进程的PID.
				- 将shm_dtime 设置为当前的时间。
		- `shmctl` 系统调用
		  collapsed:: true
			- shmctl系统调用控制共享内存的某些属性。其定义如下:
			- ```c
			  #include <sys/shm.h>
			  int shmctl( int shm_id, int command, struct shmid_ds* buf );
			  ```
			- 其中，shm_ id参数是由shmget调用返回的共享内存标识符。command 参数指定要执行
			  的命令。shmctl 支持的所有命令如表13-3所示。
			- ![image.png](../assets/image_1659579976600_0.png)
			- shmctl成功时的返回值取决于command参数，如表13-3所示。shmetl 失败时返回-1，并设置errno.
			-
			-
			-
		- 共享内存的POSIX方法
		  collapsed:: true
			- 6.5节中我们介绍过mmap兩数。利用它的MAP_ _ANONYMOUS标志我们可以实现父、子进程之间的匿名内存共享。通过打开同一个文件，mmap也可以实现无关进程之间的内存共享。Linux 提供了另外- -种利用mmap在无关进程之间共享内存的方式。这种方式无须任何文件的支持，但它需要先使用如下函数来创建或打开-一个POSIX共享内存对象:
			- ```c
			  #include <sys/mman.h>
			  #include <sys/stat.h>
			  #include <fcntl.h>
			  int shm_open( const char* name, int oflag, mode_t mode );S
			  ```
			- shm_ open的使用方法与open系统调用完全相同。
			- name参数指定要创建1打开的共享内存对象。从可移植性的角度考虑，该参数应该使用
			  “/somename”的格式:以“/”开始，后接多个字符，且这些字符都不是“/”;以“\ 0”结尾，长度不超过NAME MAX (通常是255)。
			- oflag参数指定创建方式。它可以是下列标志中的-一个或者多个的按位或:
				- O_RDONLY.以只读方式打开共享内存对象。
				- O_RDWR。 以可读、可写方式打开共享内存对象。
				- O_CREAT.如果共享内存对象不存在，则创建之。此时mode参数的最低9位将指
				  定该共享内存对象的访问权限。共享内存对象被创建的时候，其初始长度为0。
				- O_ EXCL。和O_ CREAT一起使用，如果由name指定的共享内存对象已经存在，则
				  shm_open 调用返回错误，否则就创建-个新的共享内存对象。
				- O_ TRUNC。如果共享内存对象已经存在，则把它截断，使其长度为0。
			- shm_ _open 调用成功时返回-一个文件描述符。该文件描述符可用于后续的mmap调用，
			  从而将共享内存关联到调用进程。shm_ open 失败时返回-1,并设置errno。
			- 和打开的文件最后需要关闭一样，由shm_ open 创建的共享内存对象使用完之后也需要
			  被删除。这个过程是通过如下函数实现的:
			- ```c
			  #include <sys/mman.h>
			  #include <sys/stat.h>
			  #include <fcntl.h>
			  int shm_unlink( const char *name );
			  ```
			- 该函数将name参数指定的共享内存对象标记为等待删除。当所有使用该共享内存对象
			  的进程都使用ummap将它从进程中分离之后，系统将销毁这个共享内存对象所占据的资源。
			  如果代码中使用了，上述POSIX共享内存函数，则编译的时候需要指定链接选项-lrt.
	- #### [[消息队列]]
		- `msgget`
		  collapsed:: true
			- msgget系统调用创建-一个消息队列，或者获取-一个已有的消息队列。其定义如下:
			- ```c
			  #include <sys/msg.h>
			  int msgget( key_t key, int msgflg );
			  ```
			- 和semget系统调用- -样， key 参数是一个键值，用来标识一个全局唯一的消息队列。
			- msgfg参数的使用和含义与semget系统调用的sem_ fags参数相同。
			- msgget成功时返回-个正整数值，它是消息队列的标识符。msgget 失败时返回-1,并设置errmo。
			- 如果msgget用于创建消息队列，则与之关联的内核数据结构msqid_ds将被创建并初始化。msqid_ds 结构体的定义如下:
			- ```c
			  struct msqid_ds 
			  {
			    struct ipc_perm msg_perm;			// 消息队列的操作权限
			    time_t msg_stime;					// 最后一次调用msgsnd的时间
			    time_t msg_rtime;					// 最后一次调用msgrcv的时间
			    time_t msg_ctime;					// 最后一次被修改的时间
			    unsigned long __msg_cbites;		// 消息队列中已有的字节数
			    msggnum_t msg_qunum;				// 消息队列中已有的消息数
			    msglen_t msg_qbytes;				// 消息队列允许的最大字节数
			    pid_t msg_lspid;					// 最后执行msgsnd的进程的PID
			    pid_t msg_lrpid;					// 最后执行msgrcv的进程的PID
			  };
			  ```
			-
		- `msgsnd`
		  collapsed:: true
			- msgsnd系统调用把- -条消息添加到消息队列中。其定义如下:
			- ```c
			  #include <sys/msg.h>
			  int msgsnd( int msqid, const void* msg_ptr, size_t msg_sz, int msgflg );
			  ```
			- msqid参数是由msgget调用返回的消息队列标识符。
			- msg_ ptr 参数指向一个准备发送的消息，消息必须被定义为如下类型:
			- ```c
			  struct msgbuf
			  {
			    long mtype;		// 消息类型
			    char mtext[512];	// 消息数据
			  }
			  ```
			- 其中，mtype 成员指定消息的类型，它必须是-一个正整数。mtext 是消息数据。msg. _sz
			  参数是消息的数据部分(mtext)的长度。这个长度可以为0,表示没有消息数据。
			- msgfg参数控制msgsnd的行为。它通常仅支持IPC__NOWAIT标志，即以非阻塞的方式发送消息。默认情况下，发送消息时如果消息队列满了，则msgsnd将阻塞。若IPC_NOWAIT标志被指定，则msgsnd将立即返回并设置errmo为EAGAIN。
			- 处于阻塞状态的msgsnd调用可能被如下两种异常情况所中断:
				- 消息队列被移除。此时msgsnd调用将立即返回并设置erno为EIDRM.
				- 程序接收到信号。此时msgsnd调用将立即返回并设置ermo为EINTR.
			- msgsnd成功时返回0,失败则返回-1并设置errmo. msgsnd 成功时将修改内核数据结构msqid_ _ds 的部分字段，如下所示:
				- 将msg_qnum加1.
				- 将msg_Ispid 设置为调用进程的PID.
				- 将msg_stime设置为当前的时间。
			-
		- `msgrcv`
		  collapsed:: true
			- msgrev系统调用从消息队列中获取消息。其定义如下:
			- ```c
			  #include <sys/msg.h>
			  int msgrcv( int msgid, void* msg_ptr, size_t msg_sz, long int msgtype, int msgflg );
			  ```
			- msqid参数是由msgget调用返回的消息队列标识符。
			- msg_ ptr 参数用于存储接收的消息，msg. sz参数指的是消息数据部分的长度。
			- msgtype参数指定接收何种类型的消息。我们可以使用如下几种方式来指定消息类型:
				- msgtype等于0。读取消息队列中的第一一个消息。
				- msgtype大于0。读取消息队列中第一个类型为msgtype的消息(除非指定了标志MSG_ EXCEPT,见后文)。
				- msgtype小于0。读取消息队列中第- - 个类型值比msgtype的绝对值小的消息。数msgfg控制msgrev函数的行为。它可以是如下- -些标志的按位或:
				- IPC_ NOWAIT.如果消息队列中没有消息，则msgrev调用立即返回并设置errno为ENOMSG.
				- MSG_ EXCEPT.如果msgtype大于0，则接收消息队列中第-一个非msgtype类型的消息。
				- MSG_ NOERROR。如果消息数据部分的长度超过了msg. sz, 就将它截断.
			- 处于阻塞状态的msgrcv调用还可能被如下两种异常情况所中断:
				- 消息队列被移除。此时msgrcv调用将立即返回并设置ermo为EIDRM.
				- 程序接收到信号。此时msgrcv调用将立即返回并设置ermno为EINTR.
			- msgrcv成功时返回0，失败则返回-1并设置errno. msgrev 成功时将修改内核数据结构msqid_ ds的部分字段，如下所示:
				- 将msg. _qnum 减1.
				- 将msg. _lrpid 设置为调用进程的PID.
				- 将msg_ _rtime 设置为当前的时间。
		- `msgctl`
		  collapsed:: true
			- msgctl系统调用控制消息队列的某些属性。其定义如下:
			- ```c
			  #include <sys/msg.h>
			  int msgctl( int msqid, int command, struct msqid_ds* buf );
			  ```
			- msqid参数是由msgget调用返回的共享内存标识符。command 参数指定要执行的命令。
			  msgctl支持的所有命令如表13-4所示。
			- ![image.png](../assets/image_1659581902310_0.png)
			  ![image.png](../assets/image_1659581911593_0.png)
			- msgctl成功时的返回值取决于command参数，如表13-4所示。msgctl函数失败时返回-1
			  并设置ermo。
			-
			-
- [[线程]]
	- 创建线程和结束线程
	  collapsed:: true
		- `pthread_create`
			- 创建一个线程的函数是pthread_create。其定义如下：
			- ```c
			  #include <pthread.h>
			  int pthread_create( pthread_t* thread, const pthread_attr_t* attr,
			                    void* (*start_routine)(void*), void* arg );
			  ```
			- thread参数是新线程的标识符，后续pthread_*函数通过它来引用新线程。其类型pthread_t的定义如下：
			- ```c
			  #include <bits/pthreadtypes.h>
			  typedef unsigned long int pthread_t;
			  ```
			- attr参数用于设置新线程的属性。给它传递NULL表示使用默认线程属性。线程拥有众多属性，我们将在后面详细讨论之。start_ routine 和arg参数分别指定新线程将运行的函数及其参数。
			- pthread create 成功时返回0,失败时返回错误码。-一个用户可以打开的线程数量不能超
			  过RLIMIT. NPROC软资源限制(见表7-1)。 此外，系统上所有用户能创建的线程总数也不
			  得超过/proc/s/kerme/threads-max内核参数所定义的值。
		- `pthread_exit`
			- -个进程中的所有线程都可以调用pthread_ join 函数来回收其他线程(前提是目标线程
			  是可回收的，见后文)，即等待其他线程结束，这类似于回收进程的wait和waitpid系统调用。pthread_ join 的定义如下:
			- ```c
			  #include <pthread.h>
			  int pthread_join( pthread_t thread, void** retval );
			  ```
			- thread参数是目标线程的标识符，retval 参数则是目标线程返回的退出信息。该函数会一直阻塞，直到被回收的线程结束为止。该函数成功时返回0，失败则返回错误码。可能的错误码如表14-1 所示。
			  ![image.png](../assets/image_1659600851702_0.png)
		- `pthread_cancel`
			- 有时候我们希望异常终止一个线程，即取消线程，它是通过如下函数实现的：
			- ```c
			  #include <pthread.h>
			  int pthread_cancel( pthread_t thread );
			  ```
			- thread参数是目标线程的标识符。该兩数成功时返回0,失败则返回错误码。不过，
			  接收到取消请求的目标线程可以决定是否允许被取消以及如何取消，这分别由如下两个函数完成:
			- ```c
			  #include <pthread.h>
			  int pthread_setcancelstate( int state, int *oldstate );
			  int pthread_setcanceltype( int type, int *oldtype );
			  ```
			- 这两个函数的第一个参数分别用于设置线程的取消状态(是否允许取消)和取消类型
			  (如何取消)，第二个参数则分别记录线程原来的取消状态和取消类型。state 参数有两个可选值:
				- PTHREAD_ CANCEL ENABLE, 允许线程被取消。它是线程被创建时的默认取消状态。
				- PTHREAD_ CANCEL DISABLE,禁止线程被取消。这种情况下，如果一个线程收到取消请求，则它会将请求挂起，直到该线程允许被取消。
			- type参数也有两个可选值:
				- PTHREAD_ CANCEL_ ASYNCHRONOUS,线程随时都可以被取消。它将使得接收到
				  取消请求的目标线程立即采取行动。
				- PTHREAD_ CANCEL_ DEFERRED, 允许目标线程推迟行动，直到它调用了下面几个
				  所谓的取消点函数中的一个: pthread_ join、 pthread_ testcancel、 pthread_ cond_ wait、pthread_ cond_ timedwait、 sem_wait 和sigwait.根据POSIX标准，其他可能阻塞的系统调用，比如read、wait, 也可以成为取消点。不过为了安全起见，我们最好在可能会被取消的代码中调用pthread_testcancel 函数以设置取消点。
			- pthread_ setcancelstate 和pthread. _setcanceltype 成功时返回0,失败则返回错误码。
			-
		-
		-
	- 线程属性
	  collapsed:: true
		- pthread_attr_t 结构体定义了一套完整的线程属性，如下所示：
		- ```c
		  #include <bits/pthreadtypes.h>
		  #define __SIZEOF_PTHREAD_ATTR_T 36
		  typedef union 
		  {
		    char __size[__SIZEOF_PTHREAD_ATTR_T];
		    long int __align; 
		  } pthread_attr_t;
		  ```
		- 可见，各种线程属性全部包含在-一个字符数组中。线程库定义了一系列函数来操作
		  pthread_ attr_ t类型的变量，以方便我们获取和设置线程属性。这些函数包括:
		- ```c
		  #include <pthread.h>
		  /* 初始化线程属性对象 */
		  int pthread_attr_init( pthread_attr_t* attr ) ;
		  /* 销毁线程属性对象。被销毁的线程属性对象只有再次初始化之后才能继续使用 */
		  int pthread_attr_destroy ( pthread_attr_t* attr );
		  /* 下面这些函数用于获取和设置线程属性对象的某个属性 */
		  int pthread_attr_getdetachstate ( const pthread_attr_t* attr, int* detachstate );
		  int pthread_attr_setdetachstate ( pthread_attr_t* attr, int detachstate );
		  int pthread_attr_getstackaddr (const pthread_attr_t* attr, void ** stackaddr );
		  int pthread_attr_setstackaddr ( pthread_attr_t* attr, void* stackaddr );
		  int pthread_attr_getstacksize ( const pthread_attr_t* attr, size_t* stacksize );
		  int pthread_attr_setstacksize ( pthread_attr_t* attr, size_t stacksize);
		  int pthread_attr_getstack ( const pthread_attr_t* attr, void** stackaddr,
		                             size_t* stacksize);
		  int pthread_attr_setstack ( pthread_attr_t* attr, void* stackaddr,
		                             size_t stacksize );
		  int pthread_attr_getguardsize ( const pthread_attr_t *__attr, size_t guardsize );
		  int pthread_attr_setguardsize ( pthread_attr_t* attr, size_t guardsize );
		  int pthread_attr_getschedparam ( const pthread_attr_t* attr, 
		                                  struct sched_param* param );
		  int pthread_attr_setschedparam ( pthread_attr_t* attr, 
		                                  const struct sched_param* param ) ;
		  int pthread_attr_getschedpolicy ( const pthread_attr_t* attr, int* policy );
		  int pthread_attr_setschedpolicy ( pthread_attr_t* attr, int policy );
		  int pthread_attr_getinheritsched ( const pthread_attr_t* attr, int* inherit );
		  int pthread_attr_setinheritsched (pthread_attr_t* attr, int inherit );
		  int pthread_attr_getscope ( const pthread_attr_t* attr,
		                             int* scope ) ;
		  int pthread_attr_setscope ( pthread_attr_t* attr, int scope );
		  ```
		- detachstate,线程的脱离状态。它有PTHREAD_CREATE_JOINABLE和PTHREAD_CREATE_DETACH两个可选值。前者指定线程是可以被回收的，后者使调用线程脱离与进程中其他线程的同步。脱离了与其他线程同步的线程称为“脱离线程”。脱离线程在退出时将自行释放其占用的系统资源。线程创建时该属性的默认值是PTHREAD_CREATE_JOINABLE。此外，我们也可以使用pthread_detach 函数直接将
		  线程设置为脱离线程。
		- stackaddr和stacksize,线程堆栈的起始地址和大小。- -般来说，我们不需要自己来管理线程堆栈，因为Linux默认为每个线程分配了足够的堆栈空间(- -般是8 MB)。我们可以使用ulimt -s 命令来查看或修改这个默认值。
		- guardsize,保护区域大小。如果guardsize大于0，则系统创建线程的时候会在其堆栈的尾部额外分配guardsize字节的空间，作为保护堆栈不被错误地覆盖的区域。如果guardsize等于0，则系统不为新创建的线程设置堆栈保护区。如果使用者通过pthread_attr_setstackadd 或pthread_attr_setstack 函数手动设置线程的堆栈，则
		  guardsize属性将被忽略。
		- schedparam,线程调度参数。其类型是sched_param结构体。该结构体目前还只有一个整型类型的成员一sched_ priority, 该成员表示线程的运行优先级。
		- schedpolicy，线程调度策略。该属性有SCHED_FIFO、SCHED_RR和SCHED_OTHER三个可选值，其中SCHED OTHER是默认值。SCHED_ RR表示采用轮转算法(round-robin) 调度，SCHED_ FIFO表示使用先进先出的方法调度，这两种调度方法都具备实时调度功能，但只能用于以超级用户身份运行的进程。
		- inheritsched,是否继承调用线程的调度属性。该属性有PTHREAD_INHERIT_SCHED和PTHREAD_EXPLICIT_SCHED两个可选值。前者表示新线程沿用其创建者的线程调度参数，这种情况下再设置新线程的调度参数属性将没有任何效果。后者表示调用者要明确地指定新线程的调度参数。
		- scope,线程间竞争CPU的范围，即线程优先级的有效范围。POSIX标准定义了该属性的PTHREAD_SCOPE_SYSTEM和PTHREAD_SCOPE_PROCESS 两个可选值，前者表示目标线程与系统中所有线程一起竞争CPU的使用，后者表示目标线程仅与其他隶属于同一进程的线程竞争CPU的使用。目前Linux只支持PTHREAD_SCOPE_SYSTEM这一种取值.
	- POSIX[[信号量]]
	  collapsed:: true
		- 和多进程程序一样，多线程程序也必须考虑同步问题。pthread_join 可以看作- - 种简单
		  的线程同步方式，不过很显然，它无法高效地实现复杂的同步需求，比如控制对共享资源的独占式访问，又抑或是在某个条件满足之后唤醒一个线程。 接下来我们讨论3种专门用于线程同步的机制: POSIX 信号量、互斥量和条件变量。
		- POSIX信号量函数的名字都以sem_开头，并不像大多数线程丽数那样以pthread_开
		  头。常用的POSIX信号量函数是下面5个:
		- ```c
		  #include <semaphore.h>
		  int sem_init( sem_t* sem, int pshared, unsigned int value );
		  int sem_destroy( sem_t* sem );
		  int sem_wait( sem_t* sem );
		  int sem_trywait( sem_t* sem );
		  int sem_port( sem_t* sem );
		  ```
		- 这些函数的第- -个参数sem指向被操作的信号量。
		- sem_ init 函数用于初始化-一个未命名的信号量(POSIX 信号量API支持命名信号量,不过本书不讨论它)。pshared 参数指定信号量的类型。如果其值为0,就表示这个信号量是当
		  前进程的局部信号量，否则该信号量就可以在多个进程之间共享。value 参数指定信号量的初始值。此外，初始化-一个已经被初始化的信号量将导致不可预期的结果。
		- sem_ destroy 函数用于销毁信号量，以释放其占用的内核资源。如果销毁-一个正被其他线程等待的信号量，则将导致不可预期的结果。
		  sem_wait 函数以原子操作的方式将信号量的值减1。如果信号量的值为0，则scm_wait
		  将被阻塞，直到这个信号量具有非0值。
		- sem_trywait 与sem_wait 函数相似，不过它始终立即返回，而不论被操作的信号量是否
		  具有非0值，相当于sem_ wait 的非阻塞版本。当信号量的值非0时，sem. trywait 对信号量执行减1操作。当信号量的值为0时，它将返回-1并设置errno为EAGAIN,
		- sem_post 函数以原子操作的方式将信号量的值加1.当信号量的值大于0时，其他正在
		  调用sem_ wait等待信号量的线程将被唤醒。
		- 上面这些函数成功时返回0，失败则返回-1并设置ermo.
	- [[互斥锁]]
	  collapsed:: true
		- 基础API
		  collapsed:: true
			- POSIX互斥锁的相关函数主要有如下5个:
			- ```c
			  #include <pthread.h>
			  int pthread_mutex_init( pthread_mutex_t* mutex, 
			                         const pthread_mutexattr_t* mutexattr );
			  int pthread_mutex_destroy( pthread_mutex_t* mutex );
			  int pthread_mutex_lock( pthread_mutex_t* mutex );
			  int pthread_mutex_trylock( pthread_mutex_t* mutex );
			  int pthread_mutex_unlock( pthread_mutex_t* mutex ); 
			  ```
			- 这些函数的第-一个参数mutex指向要操作的目标互斥锁，互斥锁的类型是pthread_mutex_ t结构体。
			- pthread_mutex_init 函数用于初始化互斥锁。mutexatr 参数指定互斥锁的属性。如果将它
			  设置为NULL,则表示使用默认属性。我们将在下一小节讨论互斥锁的属性。除了这个函数
			  外，我们还可以使用如下方式来初始化一个互斥锁:
			- ```c
			  pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
			  ```
			- 宏PTHREAD MUTEX. INITIALIZER 实际上只是把互斥锁的各个字段都初始化为0.
			- pthread_mutex_destroy 函数用于销毁互斥锁，以释放其占用的内核资源。销毁-一个已经加锁的互斥锁将导致不可预期的后果。
			- pthread mutex_lock 函数以原子操作的方式给一个互斥锁加锁。如果目标互斥锁已经被
			  锁上，则pthread_mutex_lock 调用将阻塞，直到该互斥锁的占有者将其解锁。
			- pthread_mutex_trylock 与pthread_mutex_lock 函数类似，不过它始终立即返回，而不论被操作的互斥锁是否已经被加锁，相当于pthread_mutex_lock 的非阻塞版本。当目标互斥锁未被加锁时，pthread_mutex_trylock 对互斥锁执行加锁操作。当互斥锁已经被加锁时，
			  pthread_mutex_trylock 将返回错误码EBUSY。需要注意的是，这里讨论的pthread_mutex_lock和pthread_mutex_trylock 的行为是针对普通锁而言的。后面我们将看到，对于其他类型的锁而言，这两个加锁函数会有不同的行为。
			- pthread_mutex_unlock函数以原子操作的方式给一个互斥 锁解锁。如果此时有其他线程
			  正在等待这个互斥锁，则这些线程中的某一个将获得它。
			- 上面这些函数成功时返回0,失败则返回错误码。
		- 互斥锁属性
		  collapsed:: true
			- pthread_mutexattr_t 结构体定义了一套完整的互斥锁属性。线程库提供了一系列函数来操作pthread_mutexattr_t类型的变量，以方便我们获取和设置互斥锁属性。这里我们列出其中一些主要的函数: .
			- ```c
			  #include <pthread.h>
			  /*初始化互斥锁属性对象*/
			  int pthread_mutexattr_init( pthread_mutexattr_t* attr );
			  /*销毁互斥锁属性对象*/
			  int pthread_mutexattr_destroy( pthread_mutexattr_t* attr );
			  /*获取和设置互斥锁的pshared属性*/
			  int pthread_mutexattr_getpshared( const pthread_mutexattr_t* attr, int* pshared ) ;
			  int pthread_mutexattr_setpshared ( pthread_mutexattr_t* attr, int pshared );
			  /*获取和设置互斥锁的type属性*/S
			  int pthread_mutexattr_gettype( const pthread_mutexattr_t* attr, int* type) ;
			  int pthread_mutexattr_settype( pthread_mutexattr_t* attr, int type );
			  ```
			- 本书只讨论互斥锁的两种常用属性: pshared 和type。互斥锁属性pshared指定是否允许跨进程共享互斥锁，其可选值有两个:
				- PTHREAD_PROCESS_SHARED。互斥锁可以被跨进程共享。
				- PTHREAD_PROCESS_PRIVATE。 互斥锁只能被和锁的初始化线程隶属于同一个进程
				  的线程共享。
			- 互斥锁属性type指定互斥锁的类型。Linux 支持如下4种类型的互斥锁:
				- PTHREAD_ MUTEX_ NORMAL,普通锁。这是互斥锁默认的类型。当一个线程对 一个普通锁加锁以后，其余请求该锁的线程将形成-一个等待队列，并在该锁解锁后按优先级获得它。这种锁类型保证了资源分配的公平性。但这种锁也很容易引发问题: 一个线程如果对一个已经加锁的普通锁再次加锁，将引发死锁;对一个已经被其他线程加锁的普通锁解锁，或者对一个已经解锁的普通锁再次解锁，将导致不可预期的后果。
				- PTHREAD_MUTEX_ERRORCHECK, 检错锁。一个线程如果对一个已经加锁的检错锁再次加锁，则加锁操作返回EDEADLK。对一个已经被其他线程加锁的检错锁解锁，或者对一个已经解锁的检错锁再次解锁，则解锁操作返回EPERM。
				- PTHREAD_MUTEX_RECURSIVE， 嵌套锁。这种锁允许-一个线程在释放锁之前多次对它加锁而不发生死锁。不过其他线程如果要获得这个锁，则当前锁的拥有者必须执行相应次数的解锁操作。对一个已经被其他线程加锁的嵌套锁解锁，或者对一个已经
				  解锁的嵌套锁再次解锁，则解锁操作返回EPERM。
				- PTHREAD MUTEX_DEFAULT, 默认锁。一个线 程如果对一个已 经加锁的默认锁再次加锁，或者对一个已经被其他线程加锁的默认锁解锁，或者对一个已经解锁的默认锁再次解锁，将导致不可预期的后果。这种锁在实现的时候可能被映射为上面三种锁之一。
	- 条件变量
	  collapsed:: true
		- 如果说互斥锁是用于同步线程对共享数据的访问的话，那么条件变量则是用于在线程之
		  间同步共享数据的值。条件变量提供了一种线程间的通知机制:当某个共享数据达到某个值的时候，唤醒等待这个共享数据的线程。
		- 条件变量的相关函数主要有如下5个:
		- ```c
		  #include <pthread.h>
		  int pthread_cond_init( pthread_cond_t* cond, const pthread_condattr_t* cond_attr );
		  int pthread_cond_destroy( pthread_cond_t* cond );
		  int pthread_cond_broadcast( pthread_cond_t* cond );
		  int pthread_cond_signal( pthread_cond_t* cond );
		  int pthread_cond_wait( pthread_cond_t* cond, pthread_mutex_t* mutex );
		  ```
		- 这些函数的第一个参数cond指向要操作的目标条件变量，条件变量的类型是pthread_
		  cond_ t 结构体。pthread_cond_init 函数用于初始化条件变量。cond_attr 参数指定条件变量的属性。如果将它设置为NULL,则表示使用默认属性。条件变量的属性不多，而且和互斥锁的属性类型相似，所以我们不再赘述。除了pthread_cond_init 函数外，我们还可以使用如下方式来初始化一个条件变量:
		- ```c
		  pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
		  ```
		- 宏PTHREAD_COND_INITIALIZER 实际上只是把条件变量的各个字段都初始化为0.
		  pthread_cond_destroy 函数用于销毁条件变量，以释放其占用的内核资源。销毁一个正在
		  被等待的条件变量将失败并返回EBUSY.
		- pthread_cond_broadcast 函数以广播的方式唤醒所有等待目标条件变量的线程。pthread_
		  cond_ signal 函数用于唤醒- - 个等待目标条件变量的线程。至于哪个线程将被唤醒，则取决于线程的优先级和调度策略。有时候我们可能想唤醒--个指定的线程，但pthread没有对该需求提供解决方法。不过我们可以间接地实现该需求:定义- -个能够唯一表示 目标线程的全局变量，在唤醒等待条件变量的线程前先设置该变量为目标线程，然后采用广播方式唤醒所有等待条件变量的线程，这些线程被唤醒后都检查该变量以判断被唤醒的是否是自己，如果是就开始执行后续代码，如果不是则返回继续等待。
		- pthread_cond_wait 函数用于等待目标条件变量。mutex 参数是用于保护条件变量的互斥
		  锁，以确保pthread_cond_wait操作的原子性。在调用pthread_cond_wait 前，必须确保互斥锁mutex已经加锁，否则将导致不可预期的结果。pthread_cond_wait 函数执行时，首先把调用线程放入条件变量的等待队列中，然后将互斥锁mutex解锁。可见，从pthread_cond wait开始执行到其调用线程被放人条件变量的等待队列之间的这段时间内，pthread_cond_signal和pthread_cond_broadcast 等函数不会修改条件变量。换言之，pthread_ cond_wait函数不会错过目标条件变量的任何变化”。当pthread_cond_wait 函数成功返回时，互斥锁mutex将再次被锁上。
		- 上面这些函数成功时返回0, 失败则返回错误码。
	- 多编程环境
	  collapsed:: true
		- 线程和进程
		  collapsed:: true
			- 如果一个多线程程序的某个线程调用了fork 内数，那么新创建的子进程是否将自动创建和父进程相同数量的线程呢?答案是“否”，正如我们期望的那样。子进程只拥有一个执行线程，它是调用fork的那个线程的完整复制。并且子进程将自动继承父
			  进程中互斥锁(条件变量与之类似)的状态。也就是说，父进程中已经被加锁的互斥锁在子
			  进程中也是被锁住的。这就引起了一一个问题:子进程可能不清楚从父进程继承而来的互斥锁的具体状态(是加锁状态还是解锁状态)。这个互斥锁可能被加锁了，但并不是由调用fork
			  函数的那个线程锁住的，而是由其他线程锁住的。如果是这种情况，则子进程若再次对该互斥锁执行加锁操作就会导致死锁。
			- 不过，pthread 提供了一个专门的函数pthread_ atfork， 以确保fork调用后父进程和子进
			  程都拥有一个清楚的锁状态。该函数的定义如下:
			- ```c
			  #include <pthread.h>
			  int pthread_atfork( void (*prepare)(void), void (*parent)(void), void(*child)(void) );
			  ```
			- 该函数将建立3个fork句柄来帮助我们清理互斥锁的状态。prepare句柄将在fork调
			  用创建出子进程之前被执行。它可以用来锁住所有父进程中的互斥锁。parent 句柄则是fork
			  调用创建出子进程之后，而fork返回之前，在父进程中被执行。它的作用是释放所有在
			  prepare句柄中被锁住的互斥锁。child 句柄是fork返回之前，在子进程中被执行。和parent
			  句柄- -样， child 句柄也是用于释放所有在prepare句柄中被锁住的互斥锁。该函数成功时返
			  回0，失败则返回错误码。
			  因此，如果要让代码清单14-3 正常工作，就应该在其中的fork调用前加人代码清单14-4
			  所示的代码。
			- ![image.png](../assets/image_1659664223750_0.png)
			-
			-
		- 线程和信号
		  collapsed:: true
			- 每个线程都可以独立地设置信号掩码。我们在10.3.2小节讨论过设置进程信号掩码的函数sigprocmask,但在多线程环境下我们应该使用如下所示的pthread版本的sigprocmask丽
			  数来设置线程信号掩码:
			- ```c
			  #include <pthread.h>
			  #include <signal.h>
			  int pthread_sigmask( int how, const sigset_t* newmask, sigset_t* oldmask );
			  ```
			- 该函数的参数的含义与sigprocmask的参数完全相同，因此不再赘述。pthread sigmask
			  成功时返回0，失败则返回错误码。
			- 由于进程中的所有线程共享该进程的信号，所以线程库将根据线程掩码决定把信号发
			  送给哪个具体的线程。因此，如果我们在每个子线程中都单独设置信号掩码，就很容易导致逻辑错误。此外，所有线程共享信号处理丽数。也就是说，当我们在一个线程中设置 了某个信号的信号处理函数后，它将覆盖其他线程为同-一个信号 设置的信号处理函数。这两点都说明，我们应该定义-一个专门的线程来处理所有的信号。这可以通过如下两个步骤来实现:
				- 1)在主线程创建出其他子线程之前就调用pthread. sigmask 来设置好信号掩码，所有新
				  创建的子线程都将自动继承这个信号掩码。这样做之后，实际上所有线程都不会响应被屏蔽的信号了。
				- 2)在某个线程中调用如下函数来等待信号并处理之:
				- ```c
				  #include <signal.h>
				  int sigwait( const sigset_t* set, int* sig );
				  ```
				- set参数指定需要等待的信号的集合。我们可以简单地将其指定为在第1步中创建的信
				  号掩码，表示在该线程中等待所有被屏蔽的信号。参数sig指向的整数用于存储该函数返回的信号值。sigwait 成功时返回0,失败则返回错误码。一旦sigwait正确返回，我们就可以对接收到的信号做处理了。很显然，如果我们使用了sigwait, 就不应该再为信号设置信号处理函数了。这是因为当程序接收到信号时，二者中只能有一-个起作用。
				- 代码清单14-5取自pthread. sigmask函数的man手册。它展示了如何通过上述两个步骤
				  实现在一个线程中统一处理所有 信号。
				- ```c
				  #include <pthread.h>
				  #include <stdio.h>
				  #include <stdlib.h>
				  #include <unistd.h>
				  #include <signal.h>
				  #include <errno.h>
				  
				  #define handle_error_en(en, msg) \
				      do                           \
				      {                            \
				          errno = en;              \
				          perror(msg);             \
				          exit(EXIT_FAILURE);      \
				      } while (0)
				  
				  static void *sig_thread(void *arg)
				  {
				      sigset_t *set = (sigset_t *)arg;
				      int s, sig;
				      for (;;)
				      {
				          // 第二个步骤，调用sigwait等待信号
				          s = sigwait(set, &sig);
				          if (s != 0)
				              handle_error_en(s, "sigwait");
				          printf("Signal handling thread got signal %d\n", sig);
				      }
				  }
				  
				  int main(int argc, char *argv[])
				  {
				      pthread_t thread;
				      sigset_t set;
				      int s;
				      // 第一个步骤，在主线程中设置信号掩码
				      sigemptyset(&set);
				      sigaddset(&set, SIGQUIT);
				      sigaddset(&set, SIGUSR1);
				      s = pthread_sigmask(SIG_BLOCK, &set, NULL);
				      if (s != 0)
				          handle_error_en(s, "pthread_sigmask");
				  
				      s = pthread_create(&thread, NULL, &sig_thread, (void *)&set);
				      if (s != 0)
				          handle_error_en(s, "pthread_create");
				  
				      pause();
				  }
				  ```
				- 最后, pthread还提供了下面的方法，使得我们可以明确地将-个信号 发送给指定的线程:
				- ```c
				  #include <signal.h>
				  int pthread_kill( pthread_t thread, int sig );
				  ```
				- 其中，thread 参数指定目标线程，sig 参数指定待发送的信号。如果sig为0,则pthread_
				  kill不发送信号，但它任然会执行错误检查。我们可以利用这种方式来检测目标线程是否存在。pthread_ kill 成功时返回0，失败则返回错误码。
				-
			-
		-
- 线程池&进程池
	- 半同步/半异步进程池实现
	  collapsed:: true
		- 综合前面的讨论，本节我们实现-一个基于图8-11所示的半同步1半异步并发模式的进程
		  池，如代码清单15-1所示。为了避免在父、子进程之间传递文件描述符，我们将接受新连接的操作放到子进程中。很显然，对于这种模式而言，一个客户连接上的所有任务始终是由一个子进程来处理的。
		- ```cpp
		  #ifndef PROCESSPOLL_H
		  #define PROCESSPOLL_H
		  
		  #include <sys/types.h>
		  #include <sys/socket.h>
		  #include <netinet/in.h>
		  #include <arpa/inet.h>
		  #include <assert.h>
		  #include <unistd.h>
		  #include <errno.h>
		  #include <string.h>
		  #include <fcntl.h>
		  #include <stdlib.h>
		  #include <sys/epoll.h>
		  #include <signal.h>
		  #include <sys/wait.h>
		  #include <sys/stat.h>
		  #include <stdio.h>
		  
		  // 描述一个子进程的类，m_pid是目标子进程的PID，m_pipefd是父进程是和子进程通信用的管道
		  class process
		  {
		  public:
		      process() : m_pid(-1) {}
		  
		  public:
		      pid_t m_pid;
		      int m_pipefd[2];
		  };
		  
		  /* 用于处理信号的管道，以实现统一事件源。后面称之为信号管道 */
		  static int sig_pipefd[2];
		  
		  static int setnonblocking(int fd)
		  {
		      int old_option = fcntl(fd, F_GETFL);
		      int new_option = old_option | O_NONBLOCK;
		      fcntl(fd, F_SETFL, new_option);
		      return old_option;
		  }
		  
		  static void addfd(int epollfd, int fd)
		  {
		      epoll_event event;
		      event.data.fd = fd;
		      event.events = EPOLLIN | EPOLLET;
		      epoll_ctl(epollfd, EPOLL_CTL_ADD, fd, &event);
		      setnonblocking(fd);
		  }
		  /* 从epollfd标识的epoll内核事件表中删除fd上的所有注册事件 */
		  static void removefd(int epollfd, int fd)
		  {
		      epoll_ctl(epollfd, EPOLL_CTL_DEL, fd, 0);
		      close(fd);
		  }
		  static void sig_handler(int sig)
		  {
		      int save_errno = errno;
		      int msg = sig;
		      send(sig_pipefd[1], (char *)&msg, 1, 0);
		      errno = save_errno;
		  }
		  static void addsig(int sig, void(handler)(int), bool restart = true)
		  {
		      struct sigaction sa;
		      memset(&sa, '\0', sizeof(sa));
		      sa.sa_handler = handler;
		      if (restart)
		      {
		          sa.sa_flags |= SA_RESTART;
		      }
		      sigfillset(&sa.sa_mask);
		      assert(sigaction(sig, &sa, NULL) != -1);
		  }
		  
		  /* 进程池类，将它定义为模板类是为了代码复用。其模板参数是处理逻辑任务的类 */
		  template <typename T>
		  class processpoll
		  {
		  private:
		      /* 将构造函数定义为私有的，因此我们只能通过后面的create静态函数来创建processpoll实例 */
		      processpoll(int listenfd, int process_number = 8) : m_listenfd(listenfd), m_process_number(process_number), m_idx(-1), m_stop(false)
		      {
		          /* 进程池构造函数。参数listenfd是监听socket，他必须在创建进程池之前被创建，否则子进程无法直接引用它。
		   参数process_number指定进程池中子进程的数量 */
		          assert((process_number > 0) && (process_number <= MAX_PROCESS_NUMBER));
		  
		          m_sub_process = new process[process_number];
		          assert(m_sub_process);
		  
		          /* 创建process_number个子进程，并创建他们的父进程之间的管道 */
		          for (int i = 0; i < process_number; ++i)
		          {
		              int ret = socketpair(PF_UNIX, SOCK_STREAM, 0, m_sub_process[i].m_pipefd);
		              assert(ret == 0);
		  
		              m_sub_process[i].m_pid = fork();
		              assert(m_sub_process[i].m_pid >= 0);
		              if (m_sub_process[i].m_pid > 0)
		              {
		                  close(m_sub_process[i].m_pipefd[1]);
		                  continue;
		              }
		              else
		              {
		                  close(m_sub_process[i].m_pipefd[0]);
		                  m_idx = i;
		                  break;
		              }
		          }
		      }
		  
		  public:
		      /* 单体模式，以保证程序最多创建一个processpoll实例，这是程序正确处理信号的必要条件 */
		      static processpoll<T> *create(int listenfd, int process_number = 8)
		      {
		          if (!m_instance)
		          {
		              m_instance = new processpoll<T>(listenfd, process_number);
		          }
		          return m_instance;
		      }
		      ~processpoll()
		      {
		          delete[] m_sub_process;
		      }
		      // 启动线程池
		      void run()
		      {
		          if (m_idx != -1)
		          {
		              run_child();
		              return;
		          }
		          run_parent();
		      }
		  
		  private:
		      void setup_sig_pipe()
		      {
		          /* 创建epoll事件监听表和信号管道 */
		          m_epollfd = epoll_create(5);
		          assert(m_epollfd != -1);
		  
		          int ret = socketpair(PF_UNIX, SOCK_STREAM, 0, sig_pipefd);
		          assert(ret != -1);
		  
		          setnonblocking(sig_pipefd[1]);
		          addfd(m_epollfd, sig_pipefd[0]);
		  
		          /* 设置信号处理函数 */
		          addsig(SIGCHLD, sig_handler);
		          addsig(SIGTERM, sig_handler);
		          addsig(SIGINT, sig_handler);
		          addsig(SIGPIPE, SIG_IGN);
		      }
		      void run_parent()
		      {
		          setup_sig_pipe();
		          /* 父进程监听m_listenfd */
		          addfd(m_epollfd, m_listenfd);
		  
		          epoll_event events[MAX_EVENT_NUMBER];
		          int sub_process_counter = 0;
		          int new_conn = 1;
		          int number = 0;
		          int ret = -1;
		  
		          while (!m_stop)
		          {
		              number = epoll_wait(m_epollfd, events, MAX_EVENT_NUMBER, -1);
		              if ((number < 0) && (errno != EINTR))
		              {
		                  printf("epoll failure\n");
		                  break;
		              }
		              for (int i = 0; i < number; i++)
		              {
		                  int sockfd = events[i].data.fd;
		                  if (sockfd == m_listenfd)
		                  {
		                      /* 如果有新连接到来，就采取Round Robin方式将其分配给一个子进程处理 */
		                      int i = sub_process_counter;
		                      do
		                      {
		                          if (m_sub_process[i].m_pid != -1)
		                          {
		                              break;
		                          }
		                          i = (i + 1) & m_process_number;
		                      } while (i != sub_process_counter);
		                      if (m_sub_process[i].m_pid == -1)
		                      {
		                          m_stop = true;
		                          break;
		                      }
		                      sub_process_counter = (i + 1) & m_process_number;
		                      send(m_sub_process[i].m_pipefd[0], (char *)&new_conn, sizeof(new_conn), 0);
		                      printf("send request to client %d\n", i);
		                  }
		                  /* 下面处理父进程接受到的信号 */
		                  else if ((sockfd == sig_pipefd[0]) && (events[i].events & EPOLLIN))
		                  {
		                      int sig;
		                      char signals[1024];
		                      ret = recv(sig_pipefd[0], signals, sizeof(signals), 0);
		                      if (ret <= 0)
		                      {
		                          continue;
		                      }
		                      else
		                      {
		                          for (int i = 0; i < ret; ++i)
		                          {
		                              switch (signals[i])
		                              {
		                              case SIGCHLD:
		                                  pid_t pid;
		                                  int stat;
		                                  while ((waitpid(-1, &stat, WNOHANG)) > 0)
		                                  {
		                                      for (int i = 0; i < m_process_number; ++i)
		                                      {
		                                          /* 如果进程池中第i个子进程退出了，则主进程关闭响应的通信管道，
		                                          并设置响应的m_pid为-1，以表记该子进程已经退出 */
		                                          if (m_sub_process[i].m_pid == pid)
		                                          {
		                                              printf("child %d join\n", i);
		                                              close(m_sub_process[i].m_pipefd[0]);
		                                              m_sub_process[i].m_pid = -1;
		                                          }
		                                      }
		                                  }
		                                  /* 如果所有子进程都已经退出了，则父进程也退出 */
		                                  m_stop = true;
		                                  for (int i = 0; i < m_process_number; ++i)
		                                  {
		                                      if (m_sub_process[i].m_pid != -1)
		                                      {
		                                          m_stop = false;
		                                      }
		                                  }
		                                  break;
		                              case SIGTERM:
		                              case SIGINT:
		                                  /* 如果父进程接受到终止信号，那么就杀死所有子进程，并等待他们全部结束。当然，通知子进程结束更好的方法
		                                  是向父、子进程之间的通信管道发送特殊数据。 */
		                                  printf("kill all the client new\n");
		                                  for (int i = 0; i < m_process_number; ++i)
		                                  {
		                                      int pid = m_sub_process[i].m_pid;
		                                      if (pid != -1)
		                                      {
		                                          kill(pid, SIGTERM);
		                                      }
		                                  }
		                                  break;
		                              default:
		                                  break;
		                              }
		                          }
		                      }
		                  }
		                  else
		                  {
		                      continue;
		                  }
		              }
		          }
		          // close(m_listenfd); 由创建者关闭这个文件描述符
		          close(m_epollfd);
		      }
		      void run_child()
		      {
		          setup_sig_pipe();
		          /* 每个子进程都通过其在进程池中的信号值m_idx找到与父进程通信的管道 */
		          int pipefd = m_sub_process[m_idx].m_pipefd[1];
		          /* 子进程需要监听管道文件描述符pipefd,因为父进程将通过它来通知子进程accept新连接 */
		          addfd(m_epollfd, pipefd);
		          epoll_event events[MAX_EVENT_NUMBER];
		          T *users = new T[USER_PER_PROCESS];
		          assert(users);
		          int number = 0;
		          int ret = -1;
		  
		          while (!m_stop)
		          {
		              number = epoll_wait(m_epollfd, events, MAX_EVENT_NUMBER, -1);
		              if ((number < 0) && (errno != EINTR))
		              {
		                  printf("epoll filure\n");
		                  break;
		              }
		  
		              for (int i = 0; i < number; i++)
		              {
		                  int sockfd = events[i].data.fd;
		                  if ((sockfd == pipefd) && (events[i].events & EPOLLIN))
		                  {
		                      int client = 0;
		                      /* 从父、子进程之间的管道读取数据，并将结果保存在变量client中。
		                      如果读取成功，则标识有新客户连接到来 */
		                      ret = recv(sockfd, (char *)&client, sizeof(client), 0);
		                      if (((ret < 0) && (errno != EAGAIN)) || ret == 0)
		                      {
		                          continue;
		                      }
		                      else
		                      {
		                          struct sockaddr_in client_address;
		                          socklen_t client_addrlength = sizeof(client_address);
		                          int connfd = accept(m_listenfd, (struct sockaddr *)&client_address, &client_addrlength);
		                          if (connfd < 0)
		                          {
		                              printf("errno is: %d\n", errno);
		                              continue;
		                          }
		                          addfd(m_epollfd, connfd);
		                          /* 模板类T必须实现init方法，以初始化一个客户连接。我们直接使用connfd
		                          来索引逻辑处理对象（T类型的对象），以提高程序效率 */
		                          users[connfd].init(m_epollfd, connfd, client_address);
		                      }
		                  }
		                  /* 下面处理子进程接受到的信号 */
		                  else if ((sockfd == sig_pipefd[0]) && (events[i].events & EPOLLIN))
		                  {
		                      int sig;
		                      char signals[1024];
		                      ret = recv(sig_pipefd[0], signals, sizeof(signals), 0);
		                      if (ret <= 0)
		                      {
		                          continue;
		                      }
		                      else
		                      {
		                          for (int i = 0; i < ret; i++)
		                          {
		                              switch (signals[i])
		                              {
		                              case SIGCHLD:
		                                  pid_t pid;
		                                  int stat;
		                                  while ((pid = waitpid(-1, &stat, WNOHANG)) > 0)
		                                  {
		                                      continue;
		                                  }
		                                  break;
		                              case SIGTERM:
		                              case SIGINT:
		                              {
		                                  m_stop = true;
		                                  break;
		                              }
		                              default:
		                                  break;
		                              }
		                          }
		                      }
		                  }
		                  else if (events[i].events & EPOLLIN)
		                  {
		                      users[sockfd].process();
		                  }
		                  else
		                  {
		                      continue;
		                  }
		              }
		          }
		          delete[] users;
		          users = NULL;
		          close(pipefd);
		          // close(m_listenfd);
		          /* 我们将这句话注释掉，以提醒读者：应该由m_listenfd的创建者来关闭这个文件描述符，
		          即所谓的“对象（比如一个文件描述符，又或者一段堆内存）由哪个函数创建，就应该由哪个函数销毁” */
		          close(m_epollfd);
		      }
		  
		  private:
		      /* 进程池允许的最大子进程数量 */
		      static const int MAX_PROCESS_NUMBER = 16;
		      /* 每个子进程最多能处理的客户数量 */
		      static const int USER_PER_PROCESS = 65536;
		      /* epoll最多能处理的事件数 */
		      static const int MAX_EVENT_NUMBER = 10000;
		      /* 进程池中的进程总数 */
		      int m_process_number;
		      /* 子进程在池中的序号，从0开始 */
		      int m_idx;
		      /* 每个进程都有一个epoll内核事件表，用m_epollfd标识 */
		      int m_epollfd;
		      /* 监听socket */
		      int m_listenfd;
		      /* 子进程通过m_stop来决定是否停止运行 */
		      int m_stop;
		      /* 保存所有子进程的描述信息 */
		      process *m_sub_process;
		      /* 进程池静态实例 */
		      static processpoll<T> *m_instance = NULL;
		  };
		  #endif
		  
		  ```
	- 半同步/半反应堆线程池实现
	  collapsed:: true
		- 相比进程池实现，该线程池的通用性要高得多，因为它使用一个工作队列完全解除了主线程和工作线程的耦合关系:主线程往工作队列中插入任务，工作线程通过竞争来取得任务并执行它。不过，如果要将该线程池应用到实际服务器程序中，那么我们必须保证所有客户请求都是无状态的，因为同一个连接上的不同请求可能会由不同的线程处理。
		- ```cpp
		  // locker.h
		  #ifndef LOCKER_H
		  #define LOCKER_H
		  
		  #include <exception>
		  #include <pthread.h>
		  #include <semaphore.h>
		  // 封装信号量的类
		  class sem
		  {
		  private:
		      sem_t m_sem;
		  
		  public:
		      sem()
		      {
		          if (sem_init(&m_sem, 0, 0) != 0)
		          {
		              // 构造函数没有返回值，可以通过抛出异常来报告错误
		              throw std::exception();
		          }
		      }
		      ~sem()
		      {
		          sem_destroy(&m_sem);
		      }
		      // 等待信号量
		      bool wait()
		      {
		          return sem_wait(&m_sem) == 0;
		      }
		      // 增加信号量
		      bool post()
		      {
		          return sem_post(&m_sem) == 0;
		      }
		  };
		  
		  // 封装互斥锁的类
		  class locker
		  {
		  private:
		      pthread_mutex_t m_mutex;
		  
		  public:
		      // 创建并初始化互斥锁
		      locker()
		      {
		          if (pthread_mutex_init(&m_mutex, NULL) != 0)
		          {
		              throw std::exception();
		          }
		      }
		      ~locker()
		      {
		          pthread_mutex_destroy(&m_mutex);
		      }
		      // 获取互斥锁
		      bool lock()
		      {
		          return pthread_mutex_lock(&m_mutex);
		      }
		      // 解锁互斥锁
		      bool unlock()
		      {
		          return pthread_mutex_unlock(&m_mutex);
		      }
		  };
		  
		  // 封装条件变量的类
		  class cond
		  {
		  private:
		      pthread_mutex_t m_mutex;
		      pthread_cond_t m_cond;
		  
		  public:
		      cond()
		      {
		          if (pthread_mutex_init(&m_mutex, NULL) != 0)
		          {
		              throw std::exception();
		          }
		          if (pthread_cond_init(&m_cond, NULL) != 0)
		          {
		              // 构造函数中一旦出现问题，就应该立即释放已经成功分配了的资源
		              pthread_mutex_destroy(&m_mutex);
		              throw std::exception();
		          }
		      }
		      ~cond()
		      {
		          pthread_mutex_destroy(&m_mutex);
		          pthread_cond_destroy(&m_cond);
		      }
		      // 等待条件变量
		      bool wait()
		      {
		          int ret = 0;
		          pthread_mutex_lock(&m_mutex);
		          ret = pthread_cond_wait(&m_cond, &m_mutex);
		          pthread_mutex_unlock(&m_mutex);
		          return ret == 0;
		      }
		      // 唤醒等待条件变量的线程
		      bool signal() 
		      {
		          return pthread_cond_signal(&m_mutex) == 0;
		      }
		  };
		  
		  #endif
		  ```
		- ```cpp
		  #ifndef PTHREADPOLL_H
		  #define PTHREADPOLL_T
		  
		  #include <list>
		  #include <cstdio>
		  #include <exception>
		  #include <pthread.h>
		  /* 引用线程同步机制包装类 */
		  #include "./locker.h"
		  
		  /* 线程池类，将他定义为模版类是为了代码复用，模版参数T是任务类 */
		  template <typename T>
		  class threadpoll
		  {
		  
		  public:
		      /* 参数pthread_number是线程池中线程的数量，max_requests是请求队列中最多
		      允许的、等待处理的请求的数量 */
		      threadpoll(int pthread_number = 8, int max_requests = 10000);
		      ~threadpoll();
		      /* 忘请求队列中添加任务 */
		      bool append(T *request);
		  
		  private:
		      /* 工作线程运行的函数，它不断从工作队列中取出任务并执行之 */
		      static void *worker(void *arg);
		      void run();
		  
		  private:
		      int m_thread_number;        // 线程池中的线程数
		      int m_max_requests;         // 请求队列中允许的最大请求数
		      pthread_t *m_threads;       // 秒数线程池的树组，其大小为m_pthread_number
		      std::list<T *> m_workqueue; // 请求队列
		      locker m_queuelocker;       // 保护请求队列的互斥锁
		      sem m_queuestat;            // 是否有任务需要处理
		      bool m_stop;                // 是否结束线程
		  };
		  
		  template <typename T>
		  threadpoll<T>::threadpoll(int thread_number, int max_requests) : m_thread_number(thread_number), m_max_requests(max_requests),
		                                                                   m_stop(false), m_thread(NULL)
		  {
		      if ((thread_number <= 0) || (max_requests <= 0))
		      {
		          throw std::exception();
		      }
		      m_threads = new pthread_t[m_thread_number];
		      if (!m_threads)
		      {
		          throw std::exception();
		      }
		      /* 创建thread_number个线程，并将他们都设置为脱离线程 */
		      for (int i = 0; i < thread_number; i++)
		      {
		          printf("create the %dth thread\n", i);
		          if (pthread_create(m_threads + i, NULL, worker, this) != 0)
		          {
		              delete[] m_threads;
		              throw std::exception();
		          }
		          if (pthread_detach(m_threads[i]))
		          {
		              delete[] m_threads;
		              throw std::exception();
		          }
		      }
		  }
		  
		  template <typename T>
		  threadpoll<T>::~threadpoll()
		  {
		      delete[] m_threads;
		      m_stop = true;
		  }
		  
		  template <typename T>
		  bool threadpoll<T>::append(T *request)
		  {
		      /* 操作工作队列时一定要加锁，因为它被所有线程共享 */
		      m_queuelocker.lock();
		      if (m_workqueue.size() > m_max_requests)
		      {
		          m_queuelocker.unlock();
		          return false;
		      }
		      m_workqueue.push_back(request);
		      m_queuelocker.unlock();
		      m_queuestat.post();
		      return true;
		  }
		  
		  template <typename T>
		  void *threadpoll<T>::worker(void *arg)
		  {
		      threadpoll *poll = (threadpoll *)arg;
		      poll->run();
		      return poll;
		  }
		  
		  template <typename T>
		  void threadpoll<T>::run()
		  {
		      while (!m_stop)
		      {
		          m_queuestat.wait();
		          m_queuelocker.lock();
		          if (m_workqueue.empty())
		          {
		              m_queuelocker.unlock();
		              continue;
		          }
		          T *request = m_workqueue.front();
		          m_workqueue.pop_front();
		          m_queuelocker.unlock();
		          if (!request)
		          {
		              continue;
		          }
		          request->process();
		      }
		  }
		  
		  #endif
		  ```
		- 值得- -提的是，在C++程序中使用pthread_ create 函数时，该函数的第3个参数必须指向一个静态函数。而要在-一个静态函数中使用类的动态成员(包括成员函数和成员变量),则只能通过如下两种方式来实现:
			- 通过类的静态对象来调用。比如单体模式中，静态函数可以通过类的全局唯一实例来访问动态成员函数。
			- 将类的对象作为参数传递给该静态函数，然后在静态函数中引用这个对象，并调用其动态方法。
		- 代码清单15-3使用的是第2种方式:将线程参数设置为this 指针，然后在worker函数中获取该指针并调用其动态方法run.
	- 简单web服务器
		- locker.h
		  collapsed:: true
			- ```c
			  #ifndef LOCKER_H
			  #define LOCKER_H
			  
			  #include <exception>
			  #include <pthread.h>
			  #include <semaphore.h>
			  // 封装信号量的类
			  class sem
			  {
			  private:
			      sem_t m_sem;
			  
			  public:
			      sem()
			      {
			          if (sem_init(&m_sem, 0, 0) != 0)
			          {
			              // 构造函数没有返回值，可以通过抛出异常来报告错误
			              throw std::exception();
			          }
			      }
			      ~sem()
			      {
			          sem_destroy(&m_sem);
			      }
			      // 等待信号量
			      bool wait()
			      {
			          return sem_wait(&m_sem) == 0;
			      }
			      // 增加信号量
			      bool post()
			      {
			          return sem_post(&m_sem) == 0;
			      }
			  };
			  
			  // 封装互斥锁的类
			  class locker
			  {
			  private:
			      pthread_mutex_t m_mutex;
			  
			  public:
			      // 创建并初始化互斥锁
			      locker()
			      {
			          if (pthread_mutex_init(&m_mutex, NULL) != 0)
			          {
			              throw std::exception();
			          }
			      }
			      ~locker()
			      {
			          pthread_mutex_destroy(&m_mutex);
			      }
			      // 获取互斥锁
			      bool lock()
			      {
			          return pthread_mutex_lock(&m_mutex);
			      }
			      // 解锁互斥锁
			      bool unlock()
			      {
			          return pthread_mutex_unlock(&m_mutex);
			      }
			  };
			  
			  // 封装条件变量的类
			  class cond
			  {
			  private:
			      pthread_mutex_t m_mutex;
			      pthread_cond_t m_cond;
			  
			  public:
			      cond()
			      {
			          if (pthread_mutex_init(&m_mutex, NULL) != 0)
			          {
			              throw std::exception();
			          }
			          if (pthread_cond_init(&m_cond, NULL) != 0)
			          {
			              // 构造函数中一旦出现问题，就应该立即释放已经成功分配了的资源
			              pthread_mutex_destroy(&m_mutex);
			              throw std::exception();
			          }
			      }
			      ~cond()
			      {
			          pthread_mutex_destroy(&m_mutex);
			          pthread_cond_destroy(&m_cond);
			      }
			      // 等待条件变量
			      bool wait()
			      {
			          int ret = 0;
			          pthread_mutex_lock(&m_mutex);
			          ret = pthread_cond_wait(&m_cond, &m_mutex);
			          pthread_mutex_unlock(&m_mutex);
			          return ret == 0;
			      }
			      // 唤醒等待条件变量的线程
			      bool signal()
			      {
			          return pthread_cond_signal(&m_cond) == 0;
			      }
			  };
			  
			  #endif
			  ```
		- threadpoll.h
		  collapsed:: true
			- ```cpp
			  #ifndef PTHREADPOLL_H
			  #define PTHREADPOLL_T
			  
			  #include <list>
			  #include <cstdio>
			  #include <exception>
			  #include <pthread.h>
			  /* 引用线程同步机制包装类 */
			  #include "./locker.h"
			  
			  /* 线程池类，将他定义为模版类是为了代码复用，模版参数T是任务类 */
			  template <typename T>
			  class threadpoll
			  {
			  
			  public:
			      /* 参数pthread_number是线程池中线程的数量，max_requests是请求队列中最多
			      允许的、等待处理的请求的数量 */
			      threadpoll(int pthread_number = 8, int max_requests = 10000);
			      ~threadpoll();
			      /* 忘请求队列中添加任务 */
			      bool append(T *request);
			  
			  private:
			      /* 工作线程运行的函数，它不断从工作队列中取出任务并执行之 */
			      static void *worker(void *arg);
			      void run();
			  
			  private:
			      int m_thread_number;        // 线程池中的线程数
			      int m_max_requests;         // 请求队列中允许的最大请求数
			      pthread_t *m_threads;       // 秒数线程池的树组，其大小为m_pthread_number
			      std::list<T *> m_workqueue; // 请求队列
			      locker m_queuelocker;       // 保护请求队列的互斥锁
			      sem m_queuestat;            // 是否有任务需要处理
			      bool m_stop;                // 是否结束线程
			  };
			  
			  template <typename T>
			  threadpoll<T>::threadpoll(int thread_number, int max_requests) : m_thread_number(thread_number), m_max_requests(max_requests),
			                                                                   m_stop(false), m_threads(NULL)
			  {
			      if ((thread_number <= 0) || (max_requests <= 0))
			      {
			          throw std::exception();
			      }
			      m_threads = new pthread_t[m_thread_number];
			      if (!m_threads)
			      {
			          throw std::exception();
			      }
			      /* 创建thread_number个线程，并将他们都设置为脱离线程 */
			      for (int i = 0; i < thread_number; i++)
			      {
			          printf("create the %dth thread\n", i);
			          if (pthread_create(m_threads + i, NULL, worker, this) != 0)
			          {
			              delete[] m_threads;
			              throw std::exception();
			          }
			          if (pthread_detach(m_threads[i]))
			          {
			              delete[] m_threads;
			              throw std::exception();
			          }
			      }
			  }
			  
			  template <typename T>
			  threadpoll<T>::~threadpoll()
			  {
			      delete[] m_threads;
			      m_stop = true;
			  }
			  
			  template <typename T>
			  bool threadpoll<T>::append(T *request)
			  {
			      /* 操作工作队列时一定要加锁，因为它被所有线程共享 */
			      m_queuelocker.lock();
			      if (m_workqueue.size() > m_max_requests)
			      {
			          m_queuelocker.unlock();
			          return false;
			      }
			      m_workqueue.push_back(request);
			      m_queuelocker.unlock();
			      m_queuestat.post();
			      return true;
			  }
			  
			  template <typename T>
			  void *threadpoll<T>::worker(void *arg)
			  {
			      threadpoll *poll = (threadpoll *)arg;
			      poll->run();
			      return poll;
			  }
			  
			  template <typename T>
			  void threadpoll<T>::run()
			  {
			      while (!m_stop)
			      {
			          m_queuestat.wait();
			          m_queuelocker.lock();
			          if (m_workqueue.empty())
			          {
			              m_queuelocker.unlock();
			              continue;
			          }
			          T *request = m_workqueue.front();
			          m_workqueue.pop_front();
			          m_queuelocker.unlock();
			          if (!request)
			          {
			              continue;
			          }
			          request->process();
			      }
			  }
			  
			  #endif
			  ```
		- http_conn.h
		  collapsed:: true
			- ```c
			  #ifndef HTTPCONNECTION_H
			  #define HTTPCONNECTION_H
			  
			  #include <unistd.h>
			  #include <signal.h>
			  #include <sys/types.h>
			  #include <sys/epoll.h>
			  #include <fcntl.h>
			  #include <sys/socket.h>
			  #include <netinet/in.h>
			  #include <arpa/inet.h>
			  #include <assert.h>
			  #include <sys/stat.h>
			  #include <string.h>
			  #include <pthread.h>
			  #include <stdio.h>
			  #include <stdlib.h>
			  #include <sys/mman.h>
			  #include <stdarg.h>
			  #include <errno.h>
			  #include <sys/uio.h>
			  #include "./locker.h"
			  
			  class http_conn
			  {
			  public:
			      /* 文件名的最大长度 */
			      static const int FILENAME_LEN = 200;
			      /* 读缓冲区的大小 */
			      static const int READ_BUFFER_SIZE = 2048;
			      /* 写缓冲区的大小 */
			      static const int WRITE_BUFFER_SIZE = 1024;
			      /* HTTP请求方法，但我们仅支持GET */
			      enum METHOD
			      {
			          GET = 0,
			          POST,
			          HEAD,
			          PUT,
			          DELETE,
			          TRACE,
			          OPTIONS,
			          CONNECT,
			          PATCH
			      };
			      /* 解析客户请求时，主状态机所处的状态 */
			      enum CHECK_STATE
			      {
			          CHECK_STATE_REQUESTLINE = 0,
			          CHECK_STATE_HEADER,
			          CHECK_STATE_CONTENT
			      };
			      /* 服务器处理HTTP请求的可能结果 */
			      enum HTTP_CODE
			      {
			          NO_REQUEST,
			          GET_REQUEST,
			          BAD_REQUEST,
			          NO_RESOURCE,
			          FORBIDDEN_REQUEST,
			          FILE_REQUEST,
			          INTERNAL_ERRNO,
			          CLOSED_CONNECTION
			      };
			      /* 行的读取状态 */
			      enum LINE_STATUS
			      {
			          LINE_OK = 0,
			          LINE_BAD,
			          LINE_OPEN
			      };
			  
			      http_conn() {}
			      ~http_conn() {}
			  
			  public:
			      /* 初始化新接受的连接 */
			      void init(int sockfd, const sockaddr_in &addr);
			      /* 关闭连接 */
			      void close_conn(bool real_close = true);
			      /* 处理客户请求 */
			      void process();
			      /* 非阻塞读操作 */
			      bool read();
			      /* 非阻塞写操作 */
			      bool write();
			  
			  private:
			      /* 初始化连接 */
			      void init();
			      /* 解析HTTP请求 */
			      HTTP_CODE process_read();
			      /* 填充HTTP应答 */
			      bool process_write(HTTP_CODE ret);
			  
			      /* 下面这一组函数被process_read调用以分析HTTP请求 */
			      HTTP_CODE parse_request_line(char *text);
			      HTTP_CODE parse_hreader(char *text);
			      HTTP_CODE parse_content(char *text);
			      HTTP_CODE do_request();
			      char *get_line() { return m_read_buf + m_start_line; };
			      LINE_STATUS parse_line();
			  
			      /* 下面这一组函数被process_write调用以填充HTTP应答 */
			      void unmap();
			      bool add_response(const char *format, ...);
			      bool add_content(const char *content);
			      bool add_status_line(int status, const char *title);
			      bool add_headers(int content_length);
			      bool add_content_length(int content_length);
			      bool add_linger();
			      bool add_block_line();
			  
			  public:
			      /* 所有socket上的时间都被注册到同一个epoll内核事件表中，所以将epoll文件
			      描述符设置为静态的 */
			      static int m_epollfd;
			      /* 统计用户数量 */
			      static int m_user_count;
			  
			  private:
			      /* 该HTTP连接的socket和对方的socket地址 */
			      int m_sockfd;
			      sockaddr_in m_address;
			  
			      /* 读缓冲区 */
			      char m_read_buf[READ_BUFFER_SIZE];
			      /* 标识读缓冲区中已经读入的客户数据和最后一个字节的下一个位置 */
			      int m_read_idx;
			      /* 当前正在分析的字符在读缓冲区中的位置 */
			      int m_checked_idx;
			      /* 当前正在解析的行的起始位置 */
			      int m_start_line;
			      /* 写缓冲区 */
			      char m_write_buf[WRITE_BUFFER_SIZE];
			      /* 谢缓冲区中待发送的字节数 */
			      int m_write_idx;
			  
			      /* 主状态机当前所处的状态 */
			      CHECK_STATE m_check_state;
			      /* 请求方法 */
			      METHOD m_method;
			  
			      /* 客户请求的目标文件的完整路径其内容等于doc_root + m_url, doc_root是网站根目录 */
			      char m_real_file[FILENAME_LEN];
			      /* 客户请求的目标文件的文件名 */
			      char *m_url;
			      /* HTTP协议版本号，我们仅支持HTTP/1.1 */
			      char *m_version;
			      /* 主机名 */
			      char *m_host;
			      /* HTTP请求的消息体的长度 */
			      int m_content_length;
			      /* HTTP请求是否要求保持连接 */
			      bool m_linger;
			  
			      /* 客户请求的目标文件被mmap到内存中的起始位置 */
			      char *m_file_address;
			      /* 目标文件的状态。通过它我们可以判断文件是否存在、是否为目录、是否
			      可读，并获取文件大小等信息 */
			      struct stat m_file_stat;
			      /* 我们将采用writev来执行写操作，所以定义下面两个成员，其中m_iv_count表示
			      被写内存块的数量 */
			      struct iovec m_iv[2];
			      int m_iv_count;
			  };
			  
			  #endif
			  ```
		- http_conn.cpp
		  collapsed:: true
			- ```cpp
			  #include "http_conn.h"
			  
			  /* 定义HTTP响应的一些状态信息 */
			  const char *ok_200_title = "OK";
			  const char *error_400_title = "Bad Request";
			  const char *error_400_form = "You request has bad syntax or is inherently impossible to satisfy.\n";
			  const char *error_403_title = "Forbidden";
			  const char *error_403_form = "You do not have permission to get file from this server.\n";
			  const char *error_404_title = "Not Found";
			  const char *error_404_form = "The requested file was not found on this server.\n";
			  const char *error_500_title = "Internal Error";
			  const char *error_500_form = "There was an unusual problem serving the requested file.\n";
			  /* 网站的根目录 */
			  const char *doc_root = "/var/www/html";
			  
			  int setnonblock(int fd)
			  {
			      int old_option = fcntl(fd, F_GETFL);
			      int new_option = old_option |= O_NONBLOCK;
			      fcntl(fd, F_SETFL, new_option);
			      return old_option;
			  }
			  void addfd(int epollfd, int fd, bool one_shot)
			  {
			      epoll_event event;
			      event.data.fd = fd;
			      event.events = EPOLLIN | EPOLLET | EPOLLRDHUP;
			      if (one_shot)
			      {
			          event.events |= EPOLLONESHOT;
			      }
			      epoll_ctl(epollfd, EPOLL_CTL_ADD, fd, &event);
			      setnonblock(fd);
			  }
			  void removefd(int epollfd, int fd)
			  {
			      epoll_ctl(epollfd, EPOLL_CTL_DEL, fd, 0);
			      close(fd);
			  }
			  void modfd(int epollfd, int fd, int ev)
			  {
			      epoll_event event;
			      event.data.fd = fd;
			      event.events = ev | EPOLLET | EPOLLONESHOT | EPOLLRDHUP;
			      epoll_ctl(epollfd, EPOLL_CTL_MOD, fd, &event);
			  }
			  
			  int http_conn::m_user_count = 0;
			  int http_conn::m_epollfd = -1;
			  
			  void http_conn::close_conn(bool real_close)
			  {
			      if (real_close && (m_sockfd != -1))
			      {
			          removefd(m_epollfd, m_sockfd);
			          m_sockfd = -1;
			          m_user_count--; /* 关闭一个连接时，将客户总数减1 */
			      }
			  }
			  void http_conn::init(int sockfd, const sockaddr_in &addr)
			  {
			      m_sockfd = sockfd;
			      m_address = addr;
			      /* 如下两行是为了避免TIME_WAIT状态，仅用于调试，实际使用应该去掉 */
			      int reuse = 1;
			      setsockopt(m_sockfd, SOL_SOCKET, SO_REUSEADDR, &reuse, sizeof(reuse));
			      addfd(m_epollfd, sockfd, true);
			      m_user_count++;
			  
			      init();
			  }
			  void http_conn::init()
			  {
			      m_check_state = CHECK_STATE_REQUESTLINE;
			      m_linger = false;
			  
			      m_method = GET;
			      m_url = 0;
			      m_version = 0;
			      m_content_length = 0;
			      m_host = 0;
			      m_start_line = 0;
			      m_checked_idx = 0;
			      m_read_idx = 0;
			      m_write_idx = 0;
			      memset(m_read_buf, '\0', READ_BUFFER_SIZE);
			      memset(m_write_buf, '\0', WRITE_BUFFER_SIZE);
			      memset(m_real_file, '\0', FILENAME_LEN);
			  }
			  /* 从状态机，参考8.6 */
			  http_conn::LINE_STATUS http_conn::parse_line()
			  {
			      char temp;
			      for (; m_checked_idx < m_read_idx; ++m_checked_idx)
			      {
			          temp = m_read_buf[m_checked_idx];
			          if (temp == '\r')
			          {
			              if ((m_checked_idx + 1) == m_read_idx)
			              {
			                  return LINE_OPEN;
			              }
			              else if (m_read_buf[m_checked_idx + 1] == '\n')
			              {
			                  m_read_buf[m_checked_idx++] = '\0';
			                  m_read_buf[m_checked_idx] = '\0';
			                  return LINE_OK;
			              }
			              return LINE_BAD;
			          }
			          else if (temp == '\n')
			          {
			              if ((m_checked_idx > 1) && (m_read_buf[m_checked_idx - 1] == '\r'))
			              {
			                  m_read_buf[m_checked_idx - 1] = '\0';
			                  m_read_buf[m_checked_idx++] = '\0';
			                  return LINE_OK;
			              }
			              return LINE_BAD;
			          }
			      }
			      return LINE_OPEN;
			  }
			  /* 循环读取客户数据，直到无数据可读或者对方关闭连接 */
			  bool http_conn::read()
			  {
			      if (m_read_idx >= READ_BUFFER_SIZE)
			      {
			          return false;
			      }
			      int bytes_read = 0;
			      while (true)
			      {
			          bytes_read = recv(m_sockfd, m_read_buf + m_read_idx, READ_BUFFER_SIZE - m_read_idx, 0);
			          if (bytes_read == -1)
			          {
			              if (errno == EAGAIN || errno == EWOULDBLOCK)
			              {
			                  break;
			              }
			              return false;
			          }
			          else if (bytes_read == 0)
			          {
			              return false;
			          }
			          m_read_idx += bytes_read;
			      }
			      return true;
			  }
			  /* 解析HTTP请求行，获取请求方法、目标URL，以及HTTP版本号 */
			  http_conn::HTTP_CODE http_conn::parse_request_line(char *text)
			  {
			      m_url = strpbrk(text, " \t");
			      if (!m_url)
			      {
			          return BAD_REQUEST;
			      }
			      *m_url++ = '\0';
			      char *method = text;
			      if (strcasecmp(method, "GET") == 0)
			      {
			          m_method = GET;
			      }
			      else
			      {
			          return BAD_REQUEST;
			      }
			      m_url += strspn(m_url, " \t");
			      m_version = strpbrk(m_url, " \t");
			      if (!m_version)
			      {
			          return BAD_REQUEST;
			      }
			      *m_version++ = '\0';
			      m_version += strspn(m_version, " \t");
			      if (strcasecmp(m_version, "HTTP/1.1") != 0)
			      {
			          return BAD_REQUEST;
			      }
			      if (strncasecmp(m_url, "http://", 7) == 0)
			      {
			          m_url += 7;
			          m_url = strchr(m_url, '/');
			      }
			      if (!m_url || m_url[0] != '/')
			      {
			          return BAD_REQUEST;
			      }
			      m_check_state = CHECK_STATE_HEADER;
			      return NO_REQUEST;
			  }
			  /* 解析HTTP请求的一个头部信息 */
			  http_conn::HTTP_CODE http_conn::parse_hreader(char *text)
			  {
			      /* 遇到空行，表示头部字段解析完毕 */
			      if (text[0] == '\0')
			      {
			          /* 如果HTTP请求有消息体，则还需要读取m_content_length字节的消息体，状态
			          机转移到CHECK_STATE_CONTENT状态 */
			          if (m_content_length != 0)
			          {
			              m_check_state = CHECK_STATE_CONTENT;
			              return NO_REQUEST;
			          }
			          /* 否则说明我们已经得到了一个完整的HTTP请求 */
			          return GET_REQUEST;
			      }
			      /* 处理Connection头部字段 */
			      else if (strncasecmp(text, "Connection:", 11) == 0)
			      {
			          text += 11;
			          text += strspn(text, " \t");
			          if (strcasecmp(text, "keep-alive") == 0)
			          {
			              m_linger == true;
			          }
			      }
			      /* 处理Content-Length头部字段 */
			      else if (strncasecmp(text, "Content-Length:", 15) == 0)
			      {
			          text += 15;
			          text += strspn(text, " \t");
			          m_content_length = atol(text);
			      }
			      /* 处理Host头部字段 */
			      else if (strncasecmp(text, "Host:", 5) == 0)
			      {
			          text += 5;
			          text += strspn(text, " \t");
			          m_host = text;
			      }
			      else
			      {
			          printf("oop! unknow header %s\n", text);
			      }
			      return NO_REQUEST;
			  }
			  /* 我们没有真正解析HTTP请求的消息体，只是判断它是否被完整的读入了 */
			  http_conn::HTTP_CODE http_conn::parse_content(char *text)
			  {
			      if (m_read_idx >= (m_content_length + m_checked_idx))
			      {
			          text[m_content_length] = '\0';
			          return GET_REQUEST;
			      }
			      return NO_REQUEST;
			  }
			  /* 主状态机。参考8.6 */
			  http_conn::HTTP_CODE http_conn::process_read()
			  {
			      LINE_STATUS line_status = LINE_OK;
			      HTTP_CODE ret = NO_REQUEST;
			      char *text = 0;
			  
			      while (((m_check_state == CHECK_STATE_CONTENT) && (line_status == LINE_OK)) || ((line_status = parse_line()) == LINE_OK))
			      {
			          text = get_line();
			          m_start_line = m_checked_idx;
			          printf("got 1 http line: %s\n", text);
			  
			          switch (m_check_state)
			          {
			          case CHECK_STATE_REQUESTLINE:
			              if (ret == BAD_REQUEST)
			                  return BAD_REQUEST;
			          case CHECK_STATE_HEADER:
			          {
			              ret = parse_hreader(text);
			              if (ret == BAD_REQUEST)
			                  return BAD_REQUEST;
			              else if (ret == GET_REQUEST)
			                  return do_request();
			          }
			          case CHECK_STATE_CONTENT:
			          {
			              ret = parse_content(text);
			              if (ret == GET_REQUEST)
			                  return do_request();
			              line_status = LINE_OPEN;
			              break;
			          }
			          default:
			              return INTERNAL_ERRNO;
			          }
			      }
			      return NO_REQUEST;
			  }
			  /* 当得到一个完整、正确的HTTP请求时，我们就分析目标文件的属性。如果目标文件存在、对
			  所有用户可读，而不是目录，则使用mmap将其映射到内存地址m_file_address处，并告知调用者获取文件成功 */
			  http_conn::HTTP_CODE http_conn::do_request()
			  {
			      strcpy(m_real_file, doc_root);
			      int len = strlen(doc_root);
			      strncpy(m_real_file + len, m_url, FILENAME_LEN - len - 1);
			      if (stat(m_real_file, &m_file_stat) < 0)
			      {
			          return NO_RESOURCE;
			      }
			      if (!(m_file_stat.st_mode & S_IROTH))
			      {
			          return FORBIDDEN_REQUEST;
			      }
			      if (S_ISDIR(m_file_stat.st_mode))
			      {
			          return BAD_REQUEST;
			      }
			      int fd = open(m_real_file, O_RDONLY);
			      m_file_address = (char *)mmap(0, m_file_stat.st_size, PROT_READ,
			                                    MAP_PRIVATE, fd, 0);
			      close(fd);
			      return FILE_REQUEST;
			  }
			  /* 对内存映射区执行munmap操作 */
			  void http_conn::unmap()
			  {
			      if (m_file_address)
			      {
			          munmap(m_file_address, m_file_stat.st_size);
			          m_file_address = 0;
			      }
			  }
			  /* 写HTTP响应 */
			  bool http_conn::write()
			  {
			      int temp = 0;
			      int bytes_have_send = 0;
			      int bytes_to_send = m_write_idx;
			      if (bytes_to_send == 0)
			      {
			          modfd(m_epollfd, m_sockfd, EPOLLIN);
			          init();
			          return true;
			      }
			      while (1)
			      {
			          temp = writev(m_sockfd, m_iv, m_iv_count);
			          if (temp <= -1)
			          {
			              /* 如果TCP写缓冲没有空间， 则等待下一轮EPOLLOUT事件。虽然在此期间，
			              服务器无法立即接收同一客户的下一个请求，但这可以保证连接的完整性*/
			              if (errno == EAGAIN)
			              {
			                  modfd(m_epollfd, m_sockfd, EPOLLOUT);
			                  return true;
			              }
			              unmap();
			              return false;
			          }
			          bytes_to_send -= temp;
			          bytes_have_send += temp;
			          if (bytes_to_send <= bytes_have_send)
			          {
			              /* 发送HTTP响应成功，根据HTTP请求中的Connection字段决定是否立即关闭连接 */
			              unmap();
			              if (m_linger)
			              {
			                  init();
			                  modfd(m_epollfd, m_sockfd, EPOLLIN);
			                  return true;
			              }
			              else
			              {
			                  modfd(m_epollfd, m_sockfd, EPOLLIN);
			                  return false;
			              }
			          }
			      }
			  }
			  /* 往写缓冲区写入待发送的数据 */
			  bool http_conn::add_response(const char *format, ...)
			  {
			      if (m_write_idx >= WRITE_BUFFER_SIZE)
			      {
			          return false;
			      }
			      va_list arg_list;
			      va_start(arg_list, format);
			      int len = vsnprintf(m_write_buf + m_write_idx, WRITE_BUFFER_SIZE - 1 - m_write_idx, format, arg_list);
			      if (len >= (WRITE_BUFFER_SIZE - 1 - m_write_idx))
			      {
			          return false;
			      }
			      m_write_idx += len;
			      va_end(arg_list);
			      return true;
			  }
			  bool http_conn::add_status_line(int status, const char *title)
			  {
			      return add_response("%s %d %s\r\n", "HTTP/1.1", status, title);
			  }
			  bool http_conn::add_headers(int content_len)
			  {
			      add_content_length(content_len);
			      add_linger();
			      add_block_line();
			      return true;
			  }
			  bool http_conn::add_content_length(int content_len)
			  {
			      return add_response("Content-Length: %d\r\n", content_len);
			  }
			  bool http_conn::add_linger()
			  {
			      return add_response("Connection: %s\r\n", (m_linger == true) ? "keep-alive" : "close");
			  }
			  bool http_conn::add_block_line()
			  {
			      return add_response("%s", "\r\n");
			  }
			  bool http_conn::add_content(const char *content)
			  {
			      return add_response("%s", content);
			  }
			  /* 根据服务器处理HTTP请求的结果，决定返回给客户端的内容 */
			  bool http_conn::process_write(HTTP_CODE ret)
			  {
			      switch (ret)
			      {
			      case INTERNAL_ERRNO:
			          add_status_line(500, error_500_title);
			          add_headers(strlen(error_500_form));
			          if (!add_content(error_500_form))
			          {
			              return false;
			          }
			          break;
			      case BAD_REQUEST:
			      {
			          add_status_line(400, error_500_title);
			          add_headers(strlen(error_400_form));
			          if (!add_content(error_400_form))
			          {
			              return false;
			          }
			          break;
			      }
			      case NO_RESOURCE:
			      {
			          add_status_line(404, error_404_title);
			          add_headers(strlen(error_404_form));
			          if (!add_content(error_404_form))
			          {
			              return false;
			          }
			          break;
			      }
			      case FORBIDDEN_REQUEST:
			      {
			          add_status_line(403, error_403_title);
			          add_headers(strlen(error_403_form));
			          if (!add_content(error_403_form))
			          {
			              return false;
			          }
			          break;
			      }
			      case FILE_REQUEST:
			      {
			          add_status_line(200, ok_200_title);
			          if (m_file_stat.st_size != 0)
			          {
			              add_headers(m_file_stat.st_size);
			              m_iv[0].iov_base = m_write_buf;
			              m_iv[0].iov_len = m_write_idx;
			              m_iv[1].iov_base = m_file_address;
			              m_iv[1].iov_len = m_file_stat.st_size;
			              m_iv_count = 2;
			              return true;
			          }
			          else
			          {
			              const char *ok_string = "<html><body></body></html>";
			              add_headers(strlen(ok_string));
			              if (!add_content(ok_string))
			              {
			                  return false;
			              }
			          }
			      }
			      default:
			          return false;
			      }
			      m_iv[0].iov_base = m_write_buf;
			      m_iv[0].iov_len = m_write_idx;
			      m_iv_count = 1;
			      return true;
			  }
			  /* 由线程池中的工作线程调用，这是处理HTTP请求的入口函数 */
			  void http_conn::process()
			  {
			      HTTP_CODE read_ret = process_read();
			      if (read_ret == NO_REQUEST)
			      {
			          modfd(m_epollfd, m_sockfd, EPOLLIN);
			          return;
			      }
			      bool write_ret = process_write(read_ret);
			      if (!write_ret)
			      {
			          close_conn();
			      }
			      modfd(m_epollfd, m_sockfd, EPOLLOUT);
			  }
			  ```
		- server.cpp
		  collapsed:: true
			- ```cpp
			  #include <sys/socket.h>
			  #include <netinet/in.h>
			  #include <arpa/inet.h>
			  #include <stdio.h>
			  #include <unistd.h>
			  #include <errno.h>
			  #include <string.h>
			  #include <fcntl.h>
			  #include <stdlib.h>
			  #include <cassert>
			  #include <sys/epoll.h>
			  
			  #include "locker.h"
			  #include "threadpoll.h"
			  #include "http_conn.h"
			  
			  #define MAX_FD 65535
			  #define MAX_EVENT_NUMBER 10000
			  
			  extern int addfd(int epollfd, int fd, bool one_shot);
			  extern int removefd(int epollfd, int fd);
			  
			  void addsig(int sig, void(handler)(int), bool restart = true)
			  {
			      struct sigaction sa;
			      memset(&sa, '\0', sizeof(sa));
			      if (restart)
			      {
			          sa.sa_flags |= SA_RESTART;
			      }
			      sigfillset(&sa.sa_mask);
			      assert(sigaction(sig, &sa, NULL) != -1);
			  }
			  void show_error(int connfd, const char *info)
			  {
			      printf("%s", info);
			      send(connfd, info, strlen(info), 0);
			      close(connfd);
			  }
			  
			  int main(int argc, char *argv[])
			  {
			      if (argc <= 2)
			      {
			          printf("usage: %s ip_address port_number\n",
			                 basename(argv[0]));
			          return 1;
			      }
			      const char *ip = argv[1];
			      int port = atoi(argv[2]);
			  
			      /* 忽略SIGPIPE信号 */
			      addsig(SIGPIPE, SIG_IGN);
			  
			      /* 创建线程池 */
			      threadpoll<http_conn> *poll = NULL;
			      try
			      {
			          poll = new threadpoll<http_conn>;
			      }
			      catch (...)
			      {
			          return 1;
			      }
			      /* 预先为每个可能的客户连接分配一个http_conn对象 */
			      http_conn *users = new http_conn[MAX_FD];
			      assert(users);
			      int user_count = 0;
			  
			      int listenfd = socket(AF_INET, SOCK_STREAM, 0);
			      assert(listenfd >= 0);
			      struct linger tmp = {1, 0};
			      setsockopt(listenfd, SOL_SOCKET, SO_LINGER, &tmp, sizeof(tmp));
			  
			      int ret = 0;
			      struct sockaddr_in address;
			      bzero(&address, sizeof(address));
			      inet_pton(AF_INET, ip, &address.sin_addr);
			      address.sin_port = htons(port);
			      address.sin_family = AF_INET;
			  
			      ret = bind(listenfd, (struct sockaddr *)&address, sizeof(address));
			      assert(ret >= 0);
			  
			      ret = listen(listenfd, 5);
			      assert(ret >= 0);
			  
			      epoll_event events[MAX_EVENT_NUMBER];
			      int epollfd = epoll_create(5);
			      assert(epollfd != -1);
			      addfd(epollfd, listenfd, false);
			      http_conn::m_epollfd = epollfd;
			  
			      while (true)
			      {
			          int number = epoll_wait(epollfd, events, MAX_EVENT_NUMBER, -1);
			          if ((number < 0) && (errno != EINTR))
			          {
			              printf("epoll failure\n");
			              break;
			          }
			          for (int i = 0; i < number; i++)
			          {
			              int sockfd = events[i].data.fd;
			              if (sockfd == listenfd)
			              {
			                  sockaddr_in client_address;
			                  socklen_t client_addrlength = sizeof(client_address);
			                  int connfd = accept(listenfd, (sockaddr *)&client_address, &client_addrlength);
			                  if (connfd < 0)
			                  {
			                      printf("errno is: %d\n", errno);
			                      continue;
			                  }
			                  if (http_conn::m_user_count >= MAX_FD)
			                  {
			                      show_error(connfd, "Internal server busy");
			                      continue;
			                  }
			                  /* 初始化客户连接 */
			                  users[connfd].init(connfd, client_address);
			              }
			              else if (events[i].events & (EPOLLRDHUP | EPOLLHUP | EPOLLERR))
			              {
			                  /* 如果有异常，直接关闭客户连接 */
			                  users[sockfd].close_conn();
			              }
			              else if (events[i].events & EPOLLIN)
			              {
			                  /* 根据读的结果，决定时将任务添加到线程池还是关闭连接 */
			                  if (users[sockfd].read())
			                  {
			                      poll->append(users + sockfd);
			                  }
			                  else
			                  {
			                      users[sockfd].close_conn();
			                  }
			              }
			              else if (events[i].events & EPOLLOUT)
			              {
			                  /* 根据写的结果，决定是否关闭连接 */
			                  if (!users[sockfd].write())
			                  {
			                      users[sockfd].close_conn();
			                  }
			              }
			              else
			              {
			              }
			          }
			      }
			      close(epollfd);
			      close(listenfd);
			      delete[] users;
			      delete poll;
			      return 0;
			  }
			  ```