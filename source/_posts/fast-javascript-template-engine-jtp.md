---
layout: post
title: 最快,最简洁的javascript模板引擎jtp
category: 编程
published: true
date: 2012-10-13
---

### 欢迎来到jtp的世界
>欢迎使用jtp! 最轻量，简洁，高效的javascript模板引擎！
>如同“jtp”的名字，“轻量、简洁、高效”是jtp的哲学！

<!--more-->

### 简介
```
+ 轻量，jtp是目前能见到最轻量的javascript模板引擎，只有一个不足1.5k的文件。
+ 简洁，jtp的语法非常简单，对于一个熟悉html、js的开发人员来说学习难度为0。
+ 高效，jtp支持模板预编译，快于任何一个你所见过的javascript模板引擎。
+ 另外，jtp同时支持在浏览器环境使用及服务端javascript环境(Node.js)使用。
```

### 许可协议
>[使用jtp请您遵守LGPL协议，否则您将会被起诉。（点击可查看LGPL协议）](http://www.gnu.org/licenses/lgpl.html)

### 在线demo
>[体验在线demo](http://code.houfeng.net/demos/jtp/)

### 支持
```
+ 您可以发邮件到 admin@xhou.net
+ 或者访问 http://www.houfeng.net
+ 关注微博 http://weibo.com/houfeng
```

### 使用指南

##### 如何使用？
```
+ 下载jtp
+ 将jtp.js或jtp-min.js放到项目中合适的位置。
+ 在相关页面用<script src='jtp的url'></script>引入jtp。
```

#### 解析(jtp.parse)
代码:

	var html='<div>My name is <@$(this.name)@></div>';
	var rs=jtp.parse(html,{name:'jtp'});


结果: 

	<div>My name is jtp</div>


#### 编译(jtp.complite)
代码:

	var html='<div>My name is <@$(this.name)@></div>';
	var fn=jtp.complite(html);
	var rs=fn({name:'jtp'});


结果:

	<div>My name is jtp</div>