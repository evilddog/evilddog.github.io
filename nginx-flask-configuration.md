title: CentOS-6.7部署Nginx + uWSGI + Flask
date: 2015-04-12 21:41:54
tags: Linux
categories: Flask
---
CentOS-6.7部署Nginx + uWSGI + Flask
<br/>

<!--more -->

#**0x00**
这个坑太痛苦了......一堆乱七八糟的博客瞎扯淡,最后还是自己翻官方文档解决了

<br/>
<br/>

#**0x01**

安装各种组件和环境

```shell
[root@aliyun ~]# yum -y install gcc gcc-c++ make autoconf automake
[root@aliyun ~]# yum -y install zlib zlib-devel openssl openssl-devel pcre pcre-devel
[root@aliyun ~]# yum -y install python-devel python-setuptools libxml2-devel
[root@aliyun ~]# yum install nginx
[root@aliyun ~]# pip install virtualenv uwsgi
[root@aliyun ~]# pip install flask
```

<br/>
<br/>

#**0x02**

在/var/run创建一个简单的flask项目
```shell
[root@aliyun ~]# mkdir /var/run/flask-uwsgi && cd /var/run/flask-uwsgi
[root@aliyun ~]# virtualenv env
[root@aliyun ~]# . env/bin/activate
[root@aliyun ~]# mkdir app && cd app
[root@aliyun ~]# vim __init__.py
```

主程序
```python
#! /usr/bin/python

from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello!"

if __name__ == "__main__":
    app.run()

```

启动程序
```shell
[root@aliyun ~]# cd .. && vim run.py
```
```python
#! /usr/bin/python

from app import app

if __name__ == "__main__":
    app.run(host='0.0.0.0', debug='True')
```

<br/>
<br/>

#**0x03**
在nginx配置目录**/etc/nginx**下增加flask和uwsgi的配置文件
```python

server {
  listen  80;
  server_name 121.42.158.198; 
  location / {
    include      uwsgi_params;
    uwsgi_pass   127.0.0.1:9090;
    uwsgi_param UWSGI_PYHOME /var/run/flask-uwsgi/env;	#flask项目环境目录
    uwsgi_param UWSGI_CHDIR  /var/run/flask-uwsgi;	#flask项目启动文件目录
    uwsgi_param UWSGI_SCRIPT run:app;	#run是我的启动文件名无后缀
  }
}
```

然后在nginx默认配置中导入配置
```shell
...
    include flask_uwsgi.conf;
...
```

<br/>
<br/>

#**0x04**
最后方便flask服务开启我们创建一个启动脚本
/usr/local/bin/uwsgiserver
```shell
#!/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
 
# Check if user is root
if [ $(id -u) != "0" ]; then
    printf "Error: You must be root to run this script!\n"
    exit 1
fi
 
if [ "$1" = "start" ]; then
    psid=`ps aux|grep "uwsgi"|grep -v "grep"|wc -l`
    if [ $psid -gt 2 ];then
        echo "uwsgi is running!"
    exit 0
    else
        /usr/bin/uwsgi -s 127.0.0.1:9090 -M -p 4 -t 30 --limit-as 128 -R 10000 -d /var/log/uwsgi.log
        /usr/bin/nginx -s reload
    fi
    echo "Start uwsgi service [OK]"
elif [ "$1" = "stop" ];then
    killall -9 uwsgi
    echo "Stop uwsgi service [OK]"
elif [ "$1" = "restart" ];then
    killall -9 uwsgi
    /usr/bin/uwsgi -s 127.0.0.1:9090 -M -p 4 -t 30 --limit-as 128 -R 10000 -d /var/log/uwsgi.log
    /usr/bin/nginx -s reload
    echo "Restart uwsgi service [OK]"
else
    echo "Usages: uwsgiserver [start|stop|restart]"
fi
```

完成

```shell
启动flask-uwsgi：uwsgiserver start
启动nginx：service nginx start
```

