---
layout: post
title: REMOTE HOST IDENTIFICATION HAS CHANGED的解决方法
comments: true
date: 2017-03-14 15:28:29
tags:
	- Linux
	- SSH
	- Java
---

最近使用Mac连接Linux时, 遇到一个错误如下: 

<!--more-->


```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that the RSA host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
f0:01:6c:dc:18:5c:dc:ee:45:61:44:30:00:60:e3:r2.
Please contact your system administrator.
Add correct host key in /root/.ssh/known_hosts to get rid of this message.
Offending key in /root/.ssh/known_hosts:1
RSA host key for 192.168.1.155 has changed and you have requested strict checking.
Host key verification failed.

```

这是因为: 使用`ssh`连接的时候, 使用rsa加密, 而`~/.ssh/known_hosts`中已经存在该IP地址, 但是rsa加密对不上.

## 解决方法: `vim ~/.ssh/known_hosts`, 删除相关IP地址的rsa即可.(快捷键: dd, 可以快速删除一行)