---
layout: post
title: 移动web开发局部区域滚动问题
category: 编程
published: true
---

###问题
开发一个针对pc的web应用时，在需要不随内容滚动的页头或面页脚时，通常的做法是如下样式：
{% highlight css %}
.ui-head{ position:fixed; }
{% endhighlight %}

但是如上的样式放到手机上，我测试过android2.x、android4.x、ios4.x皆不支持position:fixed;

但经过测试，发现ios5.x已经支持position:fiexd,但通过position:fiexd一般只能实现页整的整体滚动时页头或页脚的固定，在实现页面某一小区域的滚动效果还是不太方便。

另外一种方式，是指定一个元素（通常是DIV）的高度或宽度，然后使定义如下样式：
{% highlight css %}
.ui-content {height:300px;overflow:auto;}
{% endhighlight %}

但经过测试，目前的主流的设备均不支持 overflow 样式。


###通过webkit特有的样式实现
经测试ios5.1已支持如下样式定义：
{% highlight css %}
-webkit-overflow-scrolling: touch;
{% endhighlight %}

通过如上定义，ios5.x可以样式（不用js）实现局部区域滚动；但目前我试过android2.x及android4.x发现依然不支持些样式。

###通过javascript实现
为了实现兼容主流平台及浏览器，常见的做法（很多web开发框架就是依赖javascript实现），如轻量易用的优秀开源组件 iScroll 4 

而通过js实现局部滚动，一般如下几种方式：

1. 定义样式 overflow:hidden， 然后处理touchstart、touchmove、touchend事件，控制scrollTop完成滚动功能，此种方式较为简洁，并易于实现。
2. 定义样式 overflow:hidden，并且要求容器内的元素必须有一个内层嵌套外层单一元素比如：
{% highlight html %}
<div class="ui-scroll">
	<div class="ui-scroll-inner"> </div>
</div>
{% endhighlight %}

然后去控制内层元素的margin-top或margin-left。此种方式稍现麻烦，但可以使用css3的transform2d 控制内层元素translate来实现（此种方法在某些设备上能获得较好的性能）。