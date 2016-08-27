---
layout: post
title: linux系统防火墙简单设置
category: 随笔
published: true
date: 2012-09-16
---

Linux还是比较常用的，于是我研究了一下Linux关闭防火墙命令，在这里拿出来和大家分享一下，希望你能学会Linux关闭防火墙命令 。

<!--more-->

### 永久性生效，重启后不会复原

开启： chkconfig iptables on

关闭： chkconfig iptables off

### 即时生效，重启后复原

开启： service iptables start

关闭： service iptables stop

----

需要说明的是对于Linux下的其它服务都可以用以上命令执行开启和关闭操作。

在开启了防火墙时，做如下设置，开启相关端口，

修改/etc/sysconfig/iptables 文件，添加以下内容：
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT