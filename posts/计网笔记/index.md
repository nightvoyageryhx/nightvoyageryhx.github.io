# 计网笔记


&lt;!--more--&gt;

## 概念

**ITU**（International Telecommunication Union， 国际电信联盟

**ISO**（International Standards Organization，国 际标准化组织）

**IEEE**（Institute of Electrical and Electronics  Engineers，电气和电子工程师协会）

**PPP** （Point-to-Point Protocol） 端到端协议

**SAP**（stable abstraction principle  ）稳定抽象原则

**ICMP**（Internet Control Message Protocol）Internet控制 报文 协议

传输控制协议（**TCP**，Transmission Control Protocol）

**UDP** User Datagram Protocol 用户数据报协议

开放式最短路径优先**OSPF**（Open Shortest Path First）

动态主机配置协议**DHCP**（Dynamic Host Configuration Protocol）

路由信息协议**RIP**（Routing Information Protocol）基于距离矢量算法的路由协议，利用跳数来作为计量标准

**实时传输协议**（**Real-time Transport Protocol**或简写**RTP**）是一个[网络传输协议](https://baike.baidu.com/item/网络传输协议/0?fromModule=lemma_inlink)，它是由IETF的多媒体传输工作小组1996年在RFC 1889中公布的。

实时传输控制协议(**R**eal-**t**ime  transport **C**ontrol **P**rotocol，RTCP)

**NAT**（Network Address Translation），是指网络地址转换，1994年提出的。NAT是用于在本地网络中使用私有地址，在连接互联网时转而使用全局 IP 地址的技术。

**TSAP** Transport Service Access Point 传输服务访问点

**ARP**（Address Resolution Protocol，地址解析协议）是用来将IP地址解析为MAC地址的协议。主机或三层网络设备上会维护一张ARP表，用于存储IP地址和MAC地址的映射关系，一般ARP表项包括动态ARP表项和静态ARP表项。

OSI (Open System Interconnection Reference Model)开放式通信系统互联参考模型

DNS（Domain Name System）域名解析协议、域名系统

国际互联网工程任务组（The Internet Engineering Task Force，简称 IETF）

 **征求意见稿**（英语：Request For Comments，缩写为RFC）

PSTN 公共交换电话网络

非对称[数字用户线路](https://baike.baidu.com/item/数字用户线路/18758554?fromModule=lemma_inlink)（ADSL，Asymmetric Digital Subscriber Line）

MACA带有冲突避免的多路访问

非屏蔽双绞线(UTP：Unshielded Twisted Pair)

屏蔽双绞线(STP：Shielded Twisted Pair)

**IGMP**(Internet Group Management Protocol，因特网组管理协议)允许Internet中的计算机参加多播，是计算机用做向相邻多目路由器报告多组成员的协议。多目路由器是支持组播的路由器，它向本地网络发送IGMP查询，计算机通过发送IGMP报告来应答查询。多目路由器负责将组播包转发到网络中所有组播成员。

ATM 异步传输模式(Asynchronous Transfer Mode)

无类别域间路由（Classless Inter-Domain Routing、CIDR）

## 知识点

802.3网络 MAC地址长度为48位（6个字节）

**成帧方法** ：

字符计数法 

含字节填充的分界符法 

含位填充的分界标志法 

物理层编码违例法

![IP数据报](https://picx.zhimg.com/80/v2-73e5887f92e82f68fd54572e340472e1_720w.png?source=d16d100b)

![TCP协议头](https://pic1.zhimg.com/80/v2-fd6a23525efe79103335a806082aa340_720w.png?source=d16d100b)

TCP协议头在数据部分

ICMP 报文封装在IP数据报中

前导域：10101010，用于接收方和发送方时钟同步。 8个字节

TCP在不可靠的互联网络上，提供一个端 到端的，全双工的，可靠的，面向连接的， 一对一的字节流服务

--------------

24年9月21日重启

**邮箱**

pop3、IMAP、SNMP

[POP3、SMTP和IMAP之间的区别和联系_smpt pop3 imap-CSDN博客](https://blog.csdn.net/Tomcat_king/article/details/117806384)

简单网络管理协议（**SNMP**）是TCP/IP协议簇的一个应用层协议。在1988年被制定，并被Internet体系结构委员会（IAB）采纳作为一个短期的网络管理解决方案；由于SNMP的简单性，在Internet时代得到了蓬勃的发展，1992年发布了SNMPv2版本，以增强SNMPv1的安全性和功能。现在，已经有了SNMPv3版本。



---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/%E8%AE%A1%E7%BD%91%E7%AC%94%E8%AE%B0/  

