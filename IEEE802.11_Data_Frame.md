最近在看[Network Forensics](http://www.amazon.cn/%E9%BB%91%E5%AE%A2%E5%A4%A7%E8%BF%BD%E8%B8%AA-%E7%BD%91%E7%BB%9C%E5%8F%96%E8%AF%81%E6%A0%B8%E5%BF%83%E5%8E%9F%E7%90%86%E4%B8%8E%E5%AE%9E%E8%B7%B5-%E9%9B%AA%E8%8E%89%C2%B7%E5%A4%A7%E5%8D%AB%E6%9D%9C%E5%A4%AB/dp/B00R6J9KZ4)看到无线网络分析的时候觉得无线数据帧太复杂，但是毕竟是底层协议的死东西（XD
于是记下一篇

#IEEE802.11数据帧
##0x01 IEEE802.11与IEEE802.3区别
###产生原因
  无线收发器和铜质电缆没有相同物理性质，所以信号传输的方法也不一样。为了使得物理性质差异不影响第三层(IP层)，需要中介协议来创造一个能够接入网络曾的统一接口。于是IEEE802.11诞生。
  可以简单地理解为802.11(Wireless)与802.3(Ethernet)分别是无线网络和有限网络的在Mac层的不同标准，实际上协议的细节很复杂。有兴趣请移步Wiki[IEEE802.11](http://en.wikipedia.org/wiki/IEEE_802.11)、[IEEE802.3](http://en.wikipedia.org/wiki/Ethernet)
  
###802.11数据帧区别
 - 根据帧类型不同，802.11的mac地址数量不定，比如ACK帧只有一个mac地址，数据帧有三个，WDS模式有四个......
 - 管理帧所携带的信息长度不定，比如SSID，安全设置之类
 - WPE，WPA，WPA2加密信息。实际上差别是很大的，因为毕竟传输方式都变了，详细的差别还是了解数据帧之后比较。

##0x02 802.11帧类型
802.11协议族定义了不同的数据帧类型，分别在通信是担负不同的任务：

 - **管理帧**——控制基站之间的通信，除了流量控制，是最复杂的帧(后面能分析多少是多少，管理帧实在太TM复杂);
 - **控制帧**——支持不同有效传输介质之上的流量控制;
 - **数据帧**——对无线网络设备之间通讯的三层之上的数据进行加密或者封装，也是长度最大的帧。

##0x03 通用帧格式
![Generic 802.11 MAC frame](http://evilddog.qiniudn.com/Generic-802.11-MAC-frame.png)
看着就蛋疼的结构，我们慢慢来

###Frame Control
所有帧的开头都是`2Bytes==16bits`的Frame Control（帧控制）位
![Frame control field](http://evilddog.qiniudn.com/Frame-control-field.png)

 - **Protocol Version**——即协议版本，由2bit组成，因为802.11MAC只有一个版本，它的协议编号为0，所以目前都是`00`;
 - **Type**——这个是指上文提到的帧类型，`00`表示管理帧， `01`表示控制帧，`10`表示数据帧;
 - **Subtype**——这个在`802.11 Wireless Networks the Definitive`中有介绍
![Type and subtype identifiers](http://evilddog.qiniudn.com/Type-and-subtype-identifiers-1.png)
![Type and subtype identifiers](http://evilddog.qiniudn.com/Type-and-subtype-identifiers-2.png)
 - **To DS**&&**From DS**——这两个数据帧表明数据包的发送方向
```
若数据包To DS为0，From DS为0，表明该数据包在网络主机间传输
若数据包To DS为0，From DS为1，表明该数据帧来自AP
若数据包To DS为1，From DS为0，表明该数据帧发送往AP
若数据包To DS为1，From DS为1，表明该数据帧是从AP发送自AP的，也就是说这个是个WDS(Wireless Distribution System)数据帧
```
 - **More Flag**——分片标志，是为1，否为0
 - **Retry bit**——表示需要重传的帧，若是为1，不是为0
 - 剩下的用不到，暂时不做探讨

###Duration/ID
`Duration/ID`位紧跟在`Frame control`位之后长度同为`16bits`，有三种形式
![Duration/ID field](http://evilddog.qiniudn.com/Duration-ID-field.png)

 - **Duration**设定NAV当第15bit为零时，表示传输预计使用介质多少微秒
 - **CFP**然而因为我太水，我并不明白是干嘛的
 - **PS-Poll**然而因为我太水，我并不明白有什么用

###Address
Address当然是mac address,前面说了最多可以有四个Address为啥呢。。。因为802.11比较屌

 - **目的地址**——48bits长度的IEEEmac识别码代表最后接收端
 - **源地址**—— 同样长度表示最开始发送端地址
 - **接收地址**——代表负责处理该帧的无线工作站
 - **传送端地址**——代表将帧传送至无线介质的无线界面。传送端地址通常只用于无线桥接
 - **BSSID**——用来在同一个区域划分不同的局域网。

要使用多少地址位,取决于帧类型。大部分的数据帧会用到三个位:来源、目的以及 BSSID。数据帧中,地址位的编号与排列方式取决于帧的传送路径。大部分的传输只会用到三个地址,这解释了为什么在帧格式中,四个地址位都有其中三个位相邻的。

###Sequence Control Field
此位的长度为 `16bits` ，作用是重组帧片段以及丢弃重复帧，在此不详解。
![Sequence Control Field](http://evilddog.qiniudn.com/Sequence-Control-Field.png)

###Frame Body
这个就是最重要的payload，802.11 帧的一个重要区别就是在其中并没有上层协议的标记可以区别，不像802.3帧中的第13、14字节用来指定上层协议版本
![](http://evilddog.qiniudn.com/Screenshot_2015-05-16_19-21-34.png)

###FCS
和以太网一样的帧检验序列，长度位置也相同。**不同在于以太网中如果计算所得FCS和帧结尾的不同则会被抛弃该帧，否则就会传送给上层协议处理。
而在802.11中计算无误的还必须要得到正面应答否则就要重传，这样就会大大降低传输效率。**