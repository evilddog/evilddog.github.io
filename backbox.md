title: Backbox
date: 2014-11-02 17:02:35
tags: BackBox
categories: Linux
---
backbox的一些配置
<!--more -->

<hr/>
hhhhhhhh最近又在做死，kali被折腾坏掉了，在虚拟机中产生尝试了一下backbox

因为基于Ubuntu14.04优化了Gnome的桌面，用起来很流畅，占用资源也小，再坏掉我就换CentOS(╯°Д°)╯︵ ┻━┻于是写下配置记录下某些蛋疼的设置


###sublime-text-2

从官网下载之后
```
    tar xf Sublime Text 2.0.1.tar.bz2
```
cp到合适的地方
```
    sudo mv Sublime\ Text\ 2 /usr/lib/
```
如果想在终端中使用sublime
```
    sudo ln -s /usr/lib/Sublime\ Text\ 2/sublime_text /usr/bin/sublime
```
在粘贴到文件中配置
```
    [Desktop Entry]
    Version=1.0
    Name=Sublime Text 2
    Only KDE 4 seems to use GenericName, so we reuse the KDE strings.

    # From Ubuntu's language-pack-kde-XX-base packages, version 9.04-20090413.
    GenericName=Text Editor
    Exec=sublime
    Terminal=false
    Icon=/usr/lib/Sublime Text 2/Icon/48x48/sublime_text.png
    Type=Application
    Categories=TextEditor;IDE;Development
    X-Ayatana-Desktop-Shortcuts=NewWindow
           
    [NewWindow Shortcut Group]
    Name=New Window
    Exec=sublime -n
    TargetEnvironment=Unity
```
