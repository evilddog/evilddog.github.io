title: Python Requests
date: 2015-02-08 20:51:18
tags: Request
categories: Python
---
Python第三方Requests库
<!--more-->

urllib urllib2 urllib3 .....太恶心...  没有Requests方便,遂记下

###HTTP操作请求
```shell
>>> import requests
>>> r = requests.get('http://httpbin.org/get')
>>> r = requests.post("http://httpbin.org/post")
>>> r = requests.put("http://httpbin.org/put")
>>> r = requests.delete("http://httpbin.org/delete")
>>> r = requests.head("http://httpbin.org/get")
>>> r = requests.options("http://httpbin.org/get")
```

###GET传递参数
```python
import requests
r = requests.get(url='http://www.itwhy.org')    # 最基本的GET请求
print(r.status_code)    # 获取返回状态
r = requests.get(url='http://dict.baidu.com/s', params={'wd':'python'})   #带参数的GET请求
print(r.url)
print(r.text) 
```


###发送JSON编码的数据
```python
import requests
import json 
r = requests.post('https://api.github.com/some/endpoint', data=json.dumps({'some': 'data'}))
print(r.json())
```


###上传文件
```python
import requests
 
url = 'http://httpbin.org/post'
files = {'file': open('/home/evilddog/1.jpg', 'rb')}
#files = {'file': ('fuck.jpg', open('/home/evilddog/1.jpg', 'rb'))}     #显式的设置文件名
r = requests.post(url, files=files)
print(r.text)
```
###甚至把字符串当文件上传
```python
import requests
 
url = 'http://httpbin.org/post'
files = {'file': ('test.txt', b'Hello Requests.')}     #必需显式的设置文件名
r = requests.post(url, files=files)
print(r.text)
```


###检测HTTP状态码
```shell
>>> r = requests.get('http://httpbin.org/get')
>>> r.status_code
>>> r.status_code == requests.codes.ok    #requests.codes.ok表示HTTP为200
```

###获取HTTP-header
```shell
>>> r = requests.get('http://httpbin.org/get')
>>> r.headers
CaseInsensitiveDict({'content-length': '286', 'server': 'nginx', 'connection': 'keep-alive', 'access-control-allow-credentials': 'true', 'date': 'Sun, 08 Feb 2015 13:14:09 GMT', 'access-control-allow-origin': '*', 'content-type': 'application/json'})
```

###访问headers内容
```shell
>>> r.headers['Content-Type']
'application/json; charset=utf-8'    #不存在会显示None
```


###发送包含cookie打请求
```shell
>>> url = 'http://httpbin.org/cookies'
>>> cookies = dict(cookies_are='working')
r = requests.get(url, cookies=cookies)
print(r.json())
```

###追踪重定向与请求历史
```shell
>>> r = requests.get('http://google.com')
>>> r.url
u'http://www.google.com/'
>>> r.history
(<Response [301]>,)      #Response.history 是一个:class:Request 对象的列表，为了完成请求而创建了这些对象。这个对象列表按照从最老到最近的请求进行排序。
```


###GET和OPTIONS禁用重定向处理
```shell
>>> r = requests.get('http://github.com', allow_redirects=False)
>>> r.status_code
301
>>> r.history
[]
```


###POST，PUT，PATCH，DELETE或HEAD启用重定向
```shell
>>> r = requests.post('http://github.com', allow_redirects=True)
>>> r.url
'https://github.com/'
>>> r.history
[<Response [301]>]
```


###设置超超时时间
```shell
>>> requests.get('http://github.com', timeout=0.001)
```

###SSL证书验证
```shell
>>> requests.get('https://github.com', verify=True)     #设置为False，Requests忽略对SSL证书的验证
```
```shell
>>> requests.get('https://kennethreitz.com', cert=('/path/server.crt', '/path/key'))      #指定一个本地证书用作客户端证书，可以是单个文件（包含密钥和证书）或一个包含两个文件路径的元组:
```

###指定userAgent
```python
mport requests
import json 
data = {'some': 'data'}
headers = {'content-type': 'application/json',
           "Mozilla/5.0 (X11; Linux x86_64; rv:37.0) Gecko/20100101 Firefox/37.0"}
r = requests.post('https://api.github.com/some/endpoint', data=data, headers=headers)
print(r.text)
#因为requests没有urllib2那种可以指定userAgent打方法,所以可以通过添加user-agent到headers字典打方式指定UA头
```

