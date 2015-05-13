title: linux-shell-wale
date: 2015-02-16 16:11:52
tags: Shell
categories: Linux
---

常用shell命令收藏

<!--more-->

```shell
#删除大小为0 的文件
find . -type f -size 0 -exec rm -rf {} \;
find . type d -size 0 -delete

#查看进程,按内存大小反向排序
ps -e -o "%C : %p : %z : %a" | sort -k5 -nr

#按照cup利用率反向排序
ps -e -o "%C : %p : %z : %a" |sort -nr | head -n 10

#查看某个进程uid
ps  -elf | grep "firefox"

#打开句柄数
lsof | wc -l

#按大小排序子目录
du -a --max-depth=1 | sort -nr | head

#生成evilddog的md5值
echo -n "evilddog" | md5sum | awk '{print $1}'

#添加,切换jdk版本
update-alternatives --install /usr/bin/java java /opt/jdk/jdk1.7.0_71/bin/java 700 
update-alternatives --config java

#使用adb显示E/MainActivity有关调试信息
adb logcat -v time|sed -n '/E\/MainActivity/p'

```