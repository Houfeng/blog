---
layout: post
title: 禁用CentOS Linux系统的图形界面
category: 随笔
published: true
---

如何禁用Centos GUI图形界面？

<!--more-->

vi /etc/inittab

把默认的

id:5:initdefault:

改成

id:3:initdefault:

这样登录的默认界面就是字符界面了， 需要gui时， startx 即可。