title: 浅谈浏览器的同源策略 
date: 2014-10-13 13:58:01
tags: 同源策略
categories: 浏览器安全
---
Same Origin Policy of Exploer

<!--more-->

#0x01  概念

同源策略(Same Origin Policy，SOP)，由Netscape提出的一个著名的安全策略,也称为单源策略(Single Origin policy).

它是一种用于Web浏览器编程语言（如JavaScript和Ajax）的安全措施，以保护信息的保密性和完整性。

简单的说就是限制了一个源（origin）中加载文本或脚本与来自其它源（origin）中资源的交互方式。


判定依据 : 如果两个页面拥有相同的协议（protocol），端口（如果指定），和主机，那么这两个页面就属于同一个源（origin）。

![](http://evilddog.qiniudn.com/2014-10-13%2018%3A16%3A53%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

如下两个网页：

	http://www.example.com/sam.html
	https://www.example.com/chen.html

它们的协议不一，一个是 http，一个是 https，所以不同源。同理可推端口、主机。

#0x02  解决方法

##主域相同而子域不同

**对于主域相同而子域不同的例子，可以通过设置document.domain的办法来解决。**

因为javascript允许子域之间使用顶级域名。

具体的做法是可以在http://www.a.com/a.html和http://script.a.com/b.html两个文件中分别加上document.domain = ‘a.com’

某一页面的domain默认等于window.location.hostname。主域名是不带www的域名，例如a.com，主域名前面带前缀的通常都为二级域名或多级域名

例如www.a.com其实是二级域名。 domain只能设置为主域名，不可以在b.a.com中将domain设置为c.a.com。

然后通过a.html文件中创建一个iframe，去控制iframe的contentDocument，这样两个js文件之间就可以“交互”了。

但是依然要注意设置这个属性之后，子域之间可以方便的通信，需注意的是协议和端口号必须相同。


- www.a.com上的a.html

```
    	document.domain = 'a.com';
    	var ifr = document.createElement('iframe');
    	ifr.src = 'http://script.a.com/b.html';
    	ifr.style.display = 'none';
    	document.body.appendChild(ifr);
    	ifr.onload = function(){
    		var doc = ifr.contentDocument || ifr.contentWindow.document;
    		// 在这里操纵b.html
    		alert(doc.getElementsByTagName("h1")[0].childNodes[0].nodeValue);
    	};
```

- script.a.com上的b.html

```
    	document.domain = 'a.com';
```

当然如果二级域名相同的情况下,如果其中一个被攻击了,那么另外一个就危险了  : )


##对于主域不相同的跨域的情况

<br/>
<hr/>

### 1.使用HTML5 postMessage——*HTML方法*
<hr/>
**[postMessage()](https://developer.mozilla.org/en/dom/window.postmessage)是HTML5的一个新功能，由于比较新，所以在IE6和IE7中不支持。**

不同域的跨域方法就是利用postMessage():

函数解释:
    otherWindow.postMessage(message, targetOrigin);
- otherWindow: 对接收信息页面的window的引用。可以是页面中iframe的contentWindow属性；window.open的返回值；通过name或下标从window.frames取到的值。
- message: 所要发送的数据，string类型。
- targetOrigin: 用于限制otherWindow，“*”表示不作限制


a.com中的代码 :

```
    <iframe id="ifr" src="b.com/index.html"></iframe>
    <script type="text/javascript">
    	window.onload = function() {
    		var ifr = document.getElementById('ifr');
    		var targetOrigin = 'http://b.com';
    		// 若写成'http://b.com/c/proxy.html'效果一样
    		// 若写成'http://c.com'就不会执行postMessage了
    		ifr.contentWindow.postMessage('I was there!', targetOrigin);
    	};
    </script>
```


b.com/index.html中的代码：
 
```
    <script type="text/javascript>
    	window.addEventListener('message', function(event){
    		// 通过origin属性判断消息来源地址
    		if (event.origin == 'http://a.com') {
    			alert(event.data); // 弹出"I was there!"
    			alert(event.source); // 对a.com、index.html中window对象的引用
    			// 但由于同源策略，这里event.source不可以访问window对象  
    		}
    	}, false);
    </script>
```


<br/>
<hr/>

### 2.DOM创建script——*Javasript方法*
<hr/>
**浏览器默认禁止了跨域访问，但并不禁止在页面中引用其他域的JS文件，并可以自由执行引入的JS文件中的function（包括操作cookie、Dom等等）**
根据这一点，可以方便地通过创建script节点的方法来实现完全跨域的通信。	
     	
```
    	js.onload = js.onreadystatechange = function() {
    		if (!this.readyState || this.readyState === 'loaded' || this.readyState === 'complete') {
    			// callback在此处执行
    			js.onload = js.onreadystatechange = null;
  		}
    	};
```



<br/>
<hr/>

### 3.利用iframe和location.hash——*Javascipt方法*
<hr/>
**location是javascript里边管理地址栏的内置对象，比如location.href就管理页面的url，用location.href=url就可以直接将页面重定向url。而location.hash则可以用来获取或设置页面的标签值。比如http://a.com/#aaaaa 的location.hash="#aaaaa"**


这个方法虽然比较麻烦，但是可以解决跨域的脚步置换问题，因为变hash并不会导致页面刷新，所以可以利用hash值来进行数据传递，当然数据容量是有限的。

域名a.com下的文件cs1.html要和cnblogs.com域名下的cs2.html传递信息。

cs1.html首先创建自动创建一个隐藏的iframe，iframe的src指向cnblogs.com域名下的cs2.html页面，这时的hash值可以做参数传递用。

cs2.html响应请求后再将通过修改cs1.html的hash值来传递数据（由于两个页面不在同一个域下**IE、Chrome不允许修改parent.location.hash的值**，所以要借助于a.com域名下的一个代理iframe；**Firefox可以修改**）。

同时在cs1.html上加一个定时器，隔一段时间来判断location.hash的值有没有变化，一点有变化则获取获取hash值。

a.com 下的cs1.html文件

```
    	function startRequest(){
    		var ifr = document.createElement('iframe');
    		ifr.style.display = 'none';
    		ifr.src = 'http://www.cnblogs.com/lab/cscript/cs2.html#paramdo';
    		document.body.appendChild(ifr);
    	}
    	function checkHash() {
    		try {
    			var data = location.hash ? location.hash.substring(1) : '';
    			if (console.log) {
    				console.log('Now the data is '+data);
    			}
    		} catch(e) {};
    	}
    	setInterval(checkHash, 2000);
    	cnblogs.com域名下的cs2.html:
    	//模拟一个简单的参数处理操作
    	switch(location.hash){
    		case '#paramdo':
    		callBack();
    		break;
    		case '#paramset':
    		//do something……
    		break;
    	}
    
    	function callBack(){
    		try {
    		parent.location.hash = 'somedata';
    		} catch (e) {
    			// ie、chrome的安全机制无法修改parent.location.hash，
    			// 所以要利用一个中间的cnblogs域下的代理iframe
    			var ifrproxy = document.createElement('iframe');
    			ifrproxy.style.display = 'none';
    			ifrproxy.src = 'http://a.com/test/cscript/cs3.html#somedata'; // 注意该文件在"a.com"域下
    			document.body.appendChild(ifrproxy);
    		}
    	}
```

a.com下的域名cs3.html

    //因为parent.parent和自身属于同一个域，所以可以改变其location.hash的值
    parent.parent.location.hash = self.location.hash.substring(1);

<br/>
<hr/>

###4.利用windows.name实现跨域数据传输——*Javascript方法*
<hr/>
**该方法是利用iframe的src属性由外域转向本地域，跨域数据即由iframe的window.name从外域传递到本地域。这个就巧妙地绕过了浏览器的跨域访问限制，但同时它又是安全操作**

有三个页面：

- a.com/app.html：应用页面。
- a.com/proxy.html：代理文件，一般是一个没有任何内容的html文件，需要和应用页面在同一域下。
- b.com/data.html：应用页面需要获取数据的页面，可称为数据页面。

基本操作：

* 0x01. 在应用页面（a.com/app.html）中创建一个iframe，把其src指向数据页面（b.com/data.html）。
数据页面会把数据附加到这个iframe的window.name上，data.html代码如下： 
```
    	<script type="text/javascript">
    		window.name = 'I was there!';    // 这里是要传输的数据，大小一般为2M，IE和firefox下可以大至32M左右
    		// 数据格式可以自定义，如json、字符串
    	</script>
```


* 0x02. 在应用页面（a.com/app.html）中监听iframe的onload事件，在此事件中设置这个iframe的src指向本地域的代理文件（代理文件和应用页面在同一域下，所以可以相互通信）。app.html部分代码如下： 

```
    	<script type="text/javascript">
    		var state = 0, 
    		iframe = document.createElement('iframe'),
    		loadfn = function() {
    			if (state === 1) {
    				var data = iframe.contentWindow.name;    // 读取数据
    				alert(data);    //弹出'I was there!'
    			} else if (state === 0) {
    				state = 1;
    				iframe.contentWindow.location = "http://a.com/proxy.html";   
    			 	// 设置的代理文件
    			}  
    		};
    		iframe.src = 'http://b.com/data.html';
    		if (iframe.attachEvent) {
    			iframe.attachEvent('onload', loadfn);
    		} else {
    			iframe.onload  = loadfn;
    		}
    		document.body.appendChild(iframe);
    	</script>
```

* 0x03.获取数据以后销毁这个iframe，释放内存；这也保证了安全（不被其他域frame js访问）。
    
```
    	<script type="text/javascript">
    		iframe.contentWindow.document.write('');
    		iframe.contentWindow.close();
    		document.body.removeChild(iframe);
    	</script>
```

<br/>
<hr/>

###5.同步XMLHttpRequest请求——**XML方法**
<hr/>
简单的同步XMLHttpRequest请求：

```
    var x = new XMLHttpRequest();
    x.open("POST", "/some_script.cgi", false);
    x.setRequestHeader("X-Random-Header", "Hi mom!");
    x.send("...POST payload here...");
    alert(x.responseText);
```

