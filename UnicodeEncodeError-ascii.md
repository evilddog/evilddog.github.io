title: "UnicodeEncodeError: 'ascii' codec can't encode characters"
date: 2015-02-08 19:43:06
tags: Python
categories: Python
---
Requests的一个小bug
<!--more-->

今天写spider遇到一个非常蛋疼的事情

明明在文件头标注了
```python
# -*- coding: utf-8 -*-
```
但是在运行文件的时候依然会报错

```shell
UnicodeEncodeError: 'ascii' codec can't encode characters in position 601-609: ordinal not in range(128)
```

而且在不同的库有的报错,有的不报错先贴代码

<h3>这是用requests库写的脚本</h3>
```python
#! /usr/bin/env python
# -*- coding: utf-8 -*-

import requests, os
os.system("> 1.html")

def write():
    with open('1.html', 'wb') as f:
        url = 'http://www.baidu.com'
        r = requests.get(url)
        r.encoding = 'utf-8'
        a = r.text
        f.write(a)

if __name__ == '__main__':
    write()
```

<h3>这是用自带urllib2标准库的版本</h3>
```python
#! /usr/bin/env python
# -*- coding: utf-8 -*-

import urllib2, os

os.system("> 2.html")

def write():
    with open('2.html', 'wb') as f:
        url = 'http://www.baidu.com'
        request = urllib2.Request(url)
        receive = urllib2.urlopen(request)
        b = receive.read()
        f.write(b)

if __name__ == '__main__':
    write()
```

报错的就是第三方库requests版本

按照别人的方法是在文件中加入
```python
import sys

reload(sys)
sys.setdefaultencoding('utf-8')

#还要先reload(sys)一下才能使用setdefaultencoding()简直蛋碎一地
```

然后程序可以正常运行,但是我们看一下urllib2的版本的defaultencoding依然也是ascii

至今原因不明

