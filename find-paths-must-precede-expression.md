title: 关于find命令 -name 的报错问题
date: 2015-01-25 14:43:32
tags: find
categories: Linux

---
关于find一些坑
<!--more-->
今天在用find命令时候的时候一直会造成

```shell
find: paths must precede expression: 2.txt
Usage: find [-H] [-L] [-P] [-Olevel] [-D help|tree|search|stat|rates|opt|exec] [path...] [expression]
```

命令如下:
```shell
$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  tmp  tool  Videos
$ sudo find /home/evilddog/ -type f -name *.txt -exec ls -l '{}' \; > 1.txt
$ sudo find /home/evilddog/ -type f -name *.txt -exec ls -l '{}' \; > 2.txt
$ sudo find /home/evilddog/ -type f -name *.txt -exec ls -l '{}' \; > 3.txt
find: paths must precede expression: 2.txt
Usage: find [-H] [-L] [-P] [-Olevel] [-D help|tree|search|stat|rates|opt|exec] [path...] [expression]
```
比较头疼,后来查了一个Stackoverflow才知道
**-name**后面的参数 ***.txt** 并不一定会直接传给 find命令，而是现在当前目录下进行匹配,分析下前面的三条命令

* 首先当前目录下没有*.txt的文件,-name会把匹配条件直接传递给find命令,所以会得到我们想要的结果,然后将结果新建文件1.txt并且输出到1.txt文件
* 在执行第二条命令时,因为当前目录下已经存在1.txt文件所以,fin命令替换成
```shell
sudo find /home/evilddog/ -type f -name 1.txt -exec ls -l '{}' \; > 2.txt
```
* 所以在执行第三条命令的时候因为默认匹配当前目录下的文件之后再将匹配条件传递给find命令,所以会报参数错误

所以我们只要把 **-name** 后的匹配条件用引号括起来就好了** '*.txt '**
<hr/>

###PS : find常用命令参数:

**-cmin n** 
> 匹配的文件和目录的内容或属性最后修改时间正好在 n 分钟之前。指定少于 n 分钟之前,使用 -n,指定多于 n 分钟之前,使用 +n。

**-cnewer file**
> 匹配的文件和目录的内容或属性最后修改时间早于那些文
件。

**-ctime n**
> 匹配的文件和目录的内容和属性最后修改时间在 n\*24 小时之前。

**-empty**
> 匹配空文件和目录。

**-group name**
> 匹配的文件和目录属于一个组。组可以用组名或组 ID 来表示。

**-iname pattern**
> 就像 -name 测试条件,但是大小写敏感。

**-inum n**
> 匹配的文件的 inode 号是 n。这对于找到某个特殊 inode 的所有硬链接很有帮助。

**-mmin n**
> 匹配的文件或目录的内容被修改于 n 分钟之前。

**-mtime n**
> 匹配的文件或目录的内容被修改于 n\*24 小时之前。

**-name pattern**
> 用指定的通配符模式匹配的文件和目录。

**-newer file**
> 匹配的文件和目录的内容早于指定的文件。当编写 shell 脚本,做文件备份时,非常有帮助。每次你制作一个备份,更新文件(比如说日志),然后使用 find 命令来决定自从上次更新,哪一个文件已经更改了。

**-nouser**
> 匹配的文件和目录不属于一个有效用户。这可以用来查找属于删除帐户的文件或监测攻击行为。

**-nogroup**
> 匹配的文件和目录不属于一个有效的组。

**-perm mode**
> 匹配的文件和目录的权限已经设置为指定的 mode。mode可以用八进制或符号表示法。

**-samefile name**
>相似于 -inum 测试条件。匹配和文件 name 享有同样 inode号的文件。

**-size n**
> 匹配的文件大小为 n。

** -type c **
> 匹配的文件类型是 c。

** -user name **
> 匹配的文件或目录属于某个用户。这个用户可以通过用户名或用户 ID 来表示。
