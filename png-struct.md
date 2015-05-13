title: PNG图片结构分析和取证
date: 2014-11-10 18:58:02
tags: png
categories: CTF

---

关于png的图片结构
<!--more -->

# 0x00 废话

最近比赛略忙,一大堆作业(摔,没有时间学点新的东西,意外抓到一只成信工的大牛,帅此牛write-up的时候发现 [png](http://appleu0.sinaapp.com/?p=194) 取证的一片文章,于是顺便学习一下PNG图片结构问题.
因为单独讲解比较无聊,所以会用某CTF的一道水题当作例子分析图片结构

![](http://evilddog.qiniudn.com/stego_final.png)

<br/>


# 0x01  PNG文件结构分析:

PNG是一种位图文件(bitmap file)存储格式，读成“ping”。PNG用来存储灰度图像时，灰度图像的深度最多16位，存储彩色图像时，彩色图像的深度最多48位，并且还可存储多到16位的α通道数据。


##整个PNG的数据组成是如下结构:
![](http://evilddog.qiniudn.com/PNG-struct.png)

##文件头:
每一个PNG图片的[文件标志](http://err02.com/2014/11/09/file-header/)都是如下固定的,长度为八字节(00000000-0000007):
![](http://evilddog.qiniudn.com/PNG-header.png)

示例文件结构:
![](http://evilddog.qiniudn.com/png-head-example.png)

<br/>

##PNG数据块(Chunk)
有两种,一种是关键数据块,也是标准数据块,还有一种是辅助数据块也叫可选数据块.

```
关键数据块定义了4个标准数据块，每个PNG文件都必须包含它们，PNG读写软件也都必须要支持这些数据块。
PNG文件规范没有要求PNG编译码器对可选数据块进行编码和译码，但规范提倡支持可选数据块。
```

<br/>

##PNG数据块类别如下,关键数据块为加深部分:
![](http://evilddog.qiniudn.com/data-struct.png)

<br/>

##数据块统一结构:
每个数据块由四个部分组成:
![](http://evilddog.qiniudn.com/Screenshot - 2014年11月10日 - 21时15分10秒.png)

尤其要注意的是CRC(循环冗余校验码),在每个数据块中它是对Chunk Type Code 和 Chunk Data域中的数据进行计算得到,虽然不可逆,但是我们在一定条件下可以通过爆破得到,计算方式如下:

```
x32+x26+x23+x22+x16+x12+x11+x10+x8+x7+x5+x4+x2+x+1
```
下面只重点讲解关键数据块

<br/>


#0x02  IHDR

文件头数据块IHDR(header chunk)包含有PNG中储存的图像数据的基本信息,并要作为第一个数据块出现在PNG数据块中

文件结构如下:
![](http://evilddog.qiniudn.com/IHDR.png)


<br/>

即图中0000008~0000020,实例如下:
![](http://evilddog.qiniudn.com/Screenshot - 2014年11月10日 - 21时21分17秒.png)

<br/>

```
初始的4字节为数据块数据的长度:00 00 00 0D,13字节
接下来是该数据块类型:49 48 44 52,IHDR
然后是数据块数据:宽-00 00 05 00,1280像素;
              高-00 00 04 00,1024;
              像素深度:08,2^8=256,即这是一个256色的图像;
              06,颜色类型,带α通道数据的真彩色图像,8或16;
最后的是crc:BE 93 F4 43
```

<br/>


#0x03 IDAT

0000020到000336之间都是可选数据块,不再累述,直接跳到IDATA部分;

![]()


图像数据块IDAT(image data chunk)：它存储实际的数据，在数据流中可包含多个连续顺序的图像数据块。


IDAT存放着图像真正的数据信息，因此，如果能够了解IDAT的结构，我们就可以很方便的生成PNG图像,是最重要的部分


IDAT 区块是经过压缩的，所以数据不可读 ，压缩算法一般为LZ77滑动窗口算法，如果硬要看里面的数据的话，用zlib库也是可以的

想深入研究的话请看如下链接

[](http://www.web-tinker.com/article/20471.html)
[](http://jens.quicknote.de/comp/LZ77-JensMueller.pdf)

IDAT的结构也一样和其他数据块一样:长度;标识；数据块数据；CTC数据,因为第三块数据块太长了,所以就不上图了

<br/>


#0x04  IEND

图像结束数据IEND(image trailer chunk)：它用来标记PNG文件或者数据流已经结束，并且必须要放在文件的尾部。


如果我们仔细观察PNG文件，我们会发现，文件的结尾12个字符看起来总应该是这样的：

```
00 00 00 00 49 45 4E 44 AE 42 60 82
```

不难明白，由于数据块结构的定义，IEND数据块的长度总是0（00 00 00 00，除非人为加入信息），数据标识总是IEND（49 45 4E 44），因此，CRC码也总是AE 42 60 82。

<br/>


#0x05  您的好友"赛棍"已上线

前面分析的时候我们可以看得到这个图片的阿尔法通道中是有隐藏信息的

所以我们要先提取出来这个隐藏的图片信息,有两种方法:

<b>00 : [Stegsolve](http://www.wechall.net/forum-t527/Stegsolve_1_3.html)

通过Stegsolve阿尔法通道2可以很清楚地看到隐藏了二维码:


![](http://evilddog.qiniudn.com/阿尔法通道.png)


然后保存下来之后再XOR一下就能清楚的看到是一正常的二维码


![](http://evilddog.qiniudn.com/flag.png)

扫描即得flag

```
flag{hctf_3xF$235#\x5e3}
```

<b>01 : Python