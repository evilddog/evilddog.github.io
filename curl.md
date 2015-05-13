title: cURL 常用命令
date: 2014-11-05 10:36:54
tags: curl
categories: Tools
---
一个神器
<!--more-->

#0x00   简介
curl是利用URL语法在命令行方式下工作的开源文件传输工具。

#0x01   常用命令


##--- 下载单个文件

下面的命令将URL的内容和显示在终端

```shell
curl http://www.baidu.com
```
<br/>

##--- 通过-o/-O选项保存下载的文件到指定的文件中：

- -o：将文件保存为命令行中指定的文件名的文件中
- -O：使用URL中默认的文件名保存文件到本地

```shell
#将文件下载到本地并命名为mygettext.html
curl -o mygettext.html http://www.gnu.org/software/gettext/manual/gettext.html
# 将文件保存到本地并命名为gettext.html
curl -O http://www.gnu.org/software/gettext/manual/gettext.html
```

可以用转向字符">"对输出进行转向输出

<br/>

##--- 通过-L选项进行重定向

默认情况下cURL不会发送HTTP Location headers(重定向).当一个被请求页面移动到另一个站点时，会发送一个HTTP Loaction header作为请求，
然后将请求重定向到新的地址上。

```shell
~$ curl http://www.google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>302 Moved</TITLE></HEAD><BODY>
<H1>302 Moved</H1>
The document has moved
<A HREF="http://www.google.co.jp/?gfe_rd=cr&amp;ei=g9BZVLnLGsum8weI3YDoDA">here</A>.
</BODY></HTML>
```

上述输出说明所请求的档案被转移到了http://www.google.co.jp


```shell
# 让curl使用地址重定向，此时会查询google.com.hk站点
~$ curl -L http://www.google.com
```

<br/>

##--- 断点续传
通过使用-C选项可对大文件使用断点续传功能


```shell
# 当文件在下载完成之前中断
$ curl -O http://www.baidu.com/index.html
##############             20.1%
# 通过添加-C选项继续对该文件进行下载，已经下载过的文件不会被重新下载
curl -C - -O http://www.gnu.org/software/gettext/manual/gettext.html
###############            21.1%
```

<br/>

##--- 对cURL使用网络限速

通过--limit-rate选项对CURL的最大网络使用进行限制


```shell
下载速度限制在1000B/s
curl --limit-rate 1000B -O http://www.gnu.org/software/gettext/manual/gettext.html
```

通过-z下载指定时间内修改过的文件

```shell
# 若yy.html文件在2014/12/21之后有过更新才会进行下载
curl -z 21-Dec-14 http://www.baidu.com/index.html
```
<br>

##---CURL授权

curl -z 21-Dec-11 http://www.example.com/yy.html

```shell
curl -u username:password URL
# 通常的做法是在命令行只输入用户名，之后会提示输入密码，这样可以保证在查看历史记录时不会将密码泄露
curl -u username URL
```
<br/>

##---从FTP服务器下载文件

CURL同样支持FTP下载，若在url中指定的是某个文件路径而非具体的某个要下载的文件名，CURL则会列出该目录下的所有文件名而并非下载该目录下的所有文件

```shell
# 列出public_html下的所有文件夹和文件
curl -u ftpuser:ftppass -O ftp://ftp_server/public_html/
# 下载xss.php文件
curl -u ftpuser:ftppass -O ftp://ftp_server/public_html/xss.php
```
<br/>

##---上传文件到FTP服务器

通过 -T 选项可将指定的本地文件上传到FTP服务器上

```shell
# 将myfile.txt文件上传到服务器
curl -u ftpuser:ftppass -T myfile.txt ftp://ftp.testserver.com
# 同时上传多个文件
curl -u ftpuser:ftppass -T "{file1,file2}" ftp://ftp.testserver.com
# 从标准输入获取内容保存到服务器指定的文件中
curl -u ftpuser:ftppass -T - ftp://ftp.testserver.com/myfile_1.txt
```
通过使用 -v 和 -trace获取更多的链接信息

<br/>

##---通过字典查询单词

```shell
# 查询bash单词的含义
curl dict://dict.org/d:bash
# 列出所有可用词典
curl dict://dict.org/show:db
# 在foldoc词典中查询bash单词的含义
curl dict://dict.org/d:bash:foldoc
```
<br/>

##---为CURL设置代理

-x 选项可以为CURL添加代理功能

```shell
# 指定代理主机和端口
curl -x proxysever.test.com:3128 http://google.co.in
```
<br/>

##---保存与使用网站cookie信息

```shell
# 将网站的cookies信息保存到sugarcookies文件中
curl -D sugarcookies http://localhost/sugarcrm/index.php
# 使用上次保存的cookie信息
curl -b sugarcookies http://localhost/sugarcrm/index.php
```
<br/>

##---传递请求数据

默认curl使用GET方式请求数据，这种方式下直接通过URL传递数据
可以通过 --data/-d 方式指定使用POST方式传递数据

```shell
# GET
curl -u username https://api.github.com/user?access_token=XXXXXXXXXX
# POST
curl -u username --data "param1=value1&param2=value" https://api.github.com
# 也可以指定一个文件，将该文件中的内容当作数据传递给服务器端
curl --data @filename https://github.api.com/authorizations
```

<del>默认情况下，通过POST方式传递过去的数据中若有特殊字符，首先需要将特殊字符转义在传递给服务器端，如value值中包含有空格，则需要先将空格转换成%20<del/>

```shell
curl -d "value%201" http://hostname.com
```

在新版本的CURL中，提供了新的选项 --data-urlencode，通过该选项提供的参数会自动转义特殊字符。

```shell
curl --data-urlencode "value 1" http://hostname.com
```

除了使用GET和POST协议外，还可以通过 -X 选项指定其它协议，如：

```shell
curl -I -X DELETE https://api.github.cim
```

上传文件

```shell
curl --form "fileupload=@filename.txt" http://hostname/resource
```
