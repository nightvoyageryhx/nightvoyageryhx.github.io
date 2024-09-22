# 无线信号测量及分析


&lt;!--more--&gt;

- 模3冲突

![](https://picx.zhimg.com/80/v2-fe3ce1da96a3575e65624ea420938050_720w.png &#34;&#34;)

PCI全称Physical Cell Identifier，即物理小区标识，LTE中终端以此区分不同小区的无线信号。从物理层来看，PCI是由主同步信号（PSS）与辅同步信号（SSS）组成，可以通过简单运算获得
$$
PCI=PSS&#43;{3}\times{SSS}
$$
模3干扰，其实就是两个小区的PCI值除3 余数相同，也就是PSS冲突，且方向对打，产生的干扰，例A小区PCI为9，除3余数为0，B小区PCI为21除3最后余数为0，且A跟B小区方向对打，就产生了模3干扰，一般处理方法可以是合理的调下其中一个小区的PCI位置，避开余数相同的小区对打。

- 覆盖问题

  RSRP (Reference Signal Receiving Power，参考信号接收功率) 是LTE网络中可以代表无线信号强度的关键参数。RSRP的功率可反应接收到信号的强度和覆盖的情况。

  1.弱覆盖


![测量数据](https://picx.zhimg.com/80/v2-9291c349fa0b848b5192dea71fc33c19_720w.png)


  2.假弱覆盖



![](https://picx.zhimg.com/80/v2-06988d0fe24dd7585785f2d2bfb8faab_720w.png)



在很近的位置由高强度和低强度的小区，调整接入小区规则和邻区来改善质量。



![](https://pic1.zhimg.com/80/v2-bfe109ff7456d0de810a8e55192064d7_720w.png)

![](https://pic1.zhimg.com/80/v2-d7e3c87f6fcf693338c2391ccc22c503_720w.png)



以下内容由小组共同完成

主要的测试过程就是将电脑连接到手机和gps，手机根据有gps给的位置和接收到的各基站信号的强弱打点。

手机不断地进行ping或者拨号来和基站交换信息以记录信号的情况，每一次呼叫或者ping都会生成一个点，连续的ping，即可在路径上由点连成线来观测路径上各位置的信号状况。

**********

测试西邮校区信号强度并分析基站调整策略

一、实训目的

1、pilot pioneer软件安装；

2、相关硬件安装。

3、利用pilot pioneer软件对西邮校园各点进行信号强度测量；

4、与2019年信号强度数据进行对比，猜测基站的调整方法。

*******

二、实训步骤

1、 安装测试软件鼎利pioneer；

2、 安装硬件加密狗（类似u盘）；

3、 安装手机驱动程序；

4、 安装GPS接收机驱动程序；

5、 打点测试。

*********

三、问题寻找与分析

覆盖问题

·覆盖差区域——RSRP（参考信号接收功率）小于-105dBm——红

·覆盖一般区域——RSRP为-100dBm到-95dBm——浅蓝

·覆盖良好区域——RSRP为-95dBm以上——浅绿

·覆盖特别好区域——RSRP大于-80dBm——深绿

质量问题

·质量差区域：SINR小于3——红

·质量一般区域：SINR为3到15——浅绿

·质量好区域：SINR为15以上——深绿

弱覆盖

真的弱覆盖

·调整周边小区天馈

·增加小区

非真弱覆盖

·完善邻区关系

·调整切换参数

质量问题

邻区不完整

·完善邻区关系

重叠覆盖

·控制周边小区覆盖

模3干扰

·调整PCI

·调整小区天馈

*************

四、数据分析与对比

[EARFCN](https://www.baidu.com/s?wd=EARFCN&amp;rsv_idx=2&amp;tn=15007414_12_dg&amp;usm=1&amp;ie=utf-8&amp;rsv_pq=e99ab96404059eaf&amp;oq=earfcn在通信中叫什么&amp;rsv_t=0f28PfXBXrNPtxr5itI2l3sNRNC3TmbnaM3BIMJbRv6opFOLy7258JbEcKmfwJGCDPR8hiU&amp;sa=re_dqa_zy&amp;icon=1)在通信中称为E-UTRA Absolute Radio Frequency Channel Number。

**EARFCN**是LTE系统中用来表示频段的参数，它是一个无单位的数字，用于标识TE系统中的每个频段。频段是指无线通信中分配给各种无线技术使用的一段特定的频率范围。**RSRP**（Reference Signal Received Power）：参考信号接收功率，是衡量UE（用户设备）在某一服务小区内接收到的下行参考信号的功率大小。RSRP主要反映了UE与基站之间的无线信号强度，是评估无线链路质量的重要参数。

**RSRQ**（Reference Signal Received Quality）：参考信号接收质量，是UE测量到的参考信号的质量。RSRQ是通过比较接收到的参考信号功率与总干扰加噪声功率来计算的，因此它可以更全面地反映UE在特定小区内的信号质量。

[RSSI](https://www.baidu.com/s?wd=RSSI&amp;rsv_idx=2&amp;tn=15007414_12_dg&amp;usm=3&amp;ie=utf-8&amp;rsv_pq=9f9d8aea04028792&amp;oq=rssi是什么意思&amp;rsv_t=6f1booC7R0QArUfGKBxlPXuxUHPUJlMpheaTguK9I5R5sruUsWMZFSWNklUKX8NQ7rVU1yg&amp;sa=re_dqa_zy&amp;icon=1)是&#34;Received Signal Strength Indication&#34;的缩写，即接收的信号强度指示。它是一个用于衡量设备从基站或其他发射点接收到的信号强度的测量单位，通常以[dBm](https://www.baidu.com/s?wd=dBm&amp;rsv_idx=2&amp;tn=15007414_12_dg&amp;usm=3&amp;ie=utf-8&amp;rsv_pq=9f9d8aea04028792&amp;oq=rssi是什么意思&amp;rsv_t=3fbfii3EVvs1t0u8C7dzJq1NZN9nyU6dNmDx/UBuDdMjRALKcGZecYjWVpWRP21l1&#43;ptctQ&amp;sa=re_dqa_zy&amp;icon=1)（分贝毫瓦）表示。RSSI的值越大，表示设备接收到的信号越强。RSSI不仅用于判断链接质量，还是决定是否增大广播发送强度的一个依据。RSSI的测量值一般不包括天线增益或传输系统的损耗，它是一种定位技术，通过接收到的信号强弱测定信号点与接收点的距离，进而根据相应数据进行定位计算。

![西邮部分道路实测信号强度](https://picx.zhimg.com/80/v2-c9dd84bcb7b8d92c299081d6baba2a66_720w.png)

（一）-105dBm

1、西操场

![](https://picx.zhimg.com/80/v2-d84a4d9ec7c1dd1cfd4352db918ad273_720w.jpeg)

![](https://pic1.zhimg.com/80/v2-7bc5a5056087260dbcd316d8ebc86529_720w.gif)

西操场信号强度在&gt;-105dBm区域，2019和2024年没有区别 

2、湖边崇德路

![](https://picx.zhimg.com/80/v2-e2c864f2fb517f9adf4b22f5c1fa8ace_720w.jpeg)

![](https://pic1.zhimg.com/80/v2-b2fc6892f613d95ad5edbeef124af970_720w.gif)

前后对比也没有区别

3、2019学术交流中心（没有重合路径）故只对2019的信号强度进行单独分析

![](https://pic1.zhimg.com/80/v2-e0c59ed2eb51e5c7e11f8508d01c3efa_720w.jpeg)

![](https://pic1.zhimg.com/80/v2-85a7aa4958a9b2131a8820bc2b28907b_720w.jpeg)

![](https://pica.zhimg.com/80/v2-bb8c204f43f426a4b699d1abb8465dbc_720w.jpeg)

根据基站和楼层相对位置可知，改变天馈无法解决弱覆盖问题，猜测应该增加小区
********

（二）-100dBm

1、教学楼

2019

![](https://pica.zhimg.com/80/v2-7b65edfede8765be31f9adf554a46638_720w.jpeg)

和之前-105dBm情况一样，真弱覆盖，楼层遮挡，增加基站


2、西操场南部

![](https://picx.zhimg.com/80/v2-4c292b3792343c1c52e169c0ed95c243_720w.jpeg)


![](https://pica.zhimg.com/80/v2-106ed6098c06d5f2278e04867399df82_720w.gif)


总体效果是2024基站信号得到改善；我们通过基站数据进一步分析；

![](https://picx.zhimg.com/80/v2-5196f3e5e4e1535544c06a278293e147_720w.jpeg)

![](https://pic1.zhimg.com/80/v2-b19725a4368c4b3acab883ead77b3c75_720w.jpeg)

根据基站位置可知，40936小区比38400小区信号弱，再根据2019和2024两年小区对比，猜测通过改善邻区关系，另选41134小区改善质量

3、湖边崇德路

![](https://picx.zhimg.com/80/v2-e065129d28fa84dae7c9e8643d7156d0_720w.jpeg)

![](https://picx.zhimg.com/80/v2-0c84cdcf7c37dfb50a802eae29d32bc5_720w.gif)

和2024年的图片进行对比，发现只有这一个点信号较弱，不是真弱覆盖

![](https://pica.zhimg.com/80/v2-e4ea5c11c108267891ed5d80f8b0753e_720w.jpeg)

但是邻近点信号较好，两个点所选小区不同，38544小区信号明显好于40936，猜测改善方式为调整邻区关系

4、教学楼

共有4个信号较弱的点，分为两类进行分析

（1）

![](https://picx.zhimg.com/80/v2-ec496f35f83d5a3a0b088044be9b13db_720w.jpeg)

![](https://picx.zhimg.com/80/v2-7f4e7d4501a14abb6b88d8dfbc678790_720w.gif)

图上三个蓝点，信号较弱，真弱覆盖，猜测增加了基站，或调整了天馈。

（2）

![](https://pic1.zhimg.com/80/v2-def060aeacfeeb53ceb3a527447dbd77_720w.jpeg)

但是在北边一点的位置，有信号较强的小区，但是信号还是很弱，为切换参数设置不当，猜测更改了切换参数，使信号变强。
*******
（三）-95dBm

1、湖边崇德路

![](https://pic1.zhimg.com/80/v2-569cfd8a1ac1125a71600830187b64cd_720w.jpeg)

![](https://pic1.zhimg.com/80/v2-d0ebc1e738debaaa95838570fd23a266_720w.gif)

![](https://pic1.zhimg.com/80/v2-6363f7e4f4fcc3a94dd7aaf5e3b9dd8c_720w.jpeg)

![](https://picx.zhimg.com/80/v2-0e857d4e4ef0ebcf7eb08dd76cd1abd6_720w.jpeg)

和2024年相比，-90dBm以上信号较多；并且发现教学楼旁和该路上都为同一个基站进行覆盖，~~故猜测牺牲人数密度小的信号强度来改善教学楼的信号强度~~，主要原因可能是4g的部分带宽划给5g，中国移动2600M频段双工TDD制式LTR/NR带宽有160M，20年后随着5G的建设，100M分给了5G，导致4G信号变弱。

2、3号教学楼

![](https://picx.zhimg.com/80/v2-e417e688eced02caae1f1527688ed0b9_720w.jpeg)

![](https://picx.zhimg.com/80/v2-6c5746f8c8d6bcbb4efd5ee61870780a_720w.gif)

3号教学楼旁信号得到改善，猜测基站天馈变化或增加了基站数量

3、西篮球场

![](https://picx.zhimg.com/80/v2-00f3fe5d5e75f50da2dd24df247e462c_720w.jpeg)

![](https://picx.zhimg.com/80/v2-2c7ed1e7faaaa96a166c3db19a42486f_720w.gif)

西操场信号依旧较强
***********
（四）-80dBm

对-80dBm以上信号进行对比，分析整体信号改善情况

1、 西操场

![](https://pic1.zhimg.com/80/v2-91daa6370d28e377e9cb1db0b6807278_720w.jpeg)

2、教学楼和学术交流中心

![](https://pic1.zhimg.com/80/v2-320195c603cb862aedfc2a2e0b1327e0_720w.jpeg)


3、湖边崇德路

![](https://picx.zhimg.com/80/v2-69b80b31e264a312e7d882b19b9b7dc0_720w.jpg)

 


---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/%E6%97%A0%E7%BA%BF%E4%BF%A1%E5%8F%B7%E6%B5%8B%E9%87%8F%E5%8F%8A%E5%88%86%E6%9E%90/  

