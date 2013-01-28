---
layout: post
title: 最易于使用的asp.net ajax框架
category: 编程
published: true
---

项目地址: [http://www.houfeng.net/page/AjaxEngine.aspx](http://www.houfeng.net/page/AjaxEngine.aspx)

###2009年中
工作之余写了一个简单非常方便使用Ajax框架（应该还称不是框架）！并用一个小仓库管理项目 
2009年末，确实是年末2009-12-29，写了一篇文章，介如年中所写的“框架”;
文章地址：[http://www.cnblogs.com/houfeng/archive/2009/12/29/1635350.html](http://www.cnblogs.com/houfeng/archive/2009/12/29/1635350.html)
 

###2010年初
移植了一个java版本，并送于一个朋友，java版本之后我没维护，java版本被朋友用于了几个政府项目！具说现在他还在用着，但我已不知道“它”长成什么样了！ 
还是2010年，用于了当时参与的一个OA项目，及一个短信平台项目，此时AjaxEngine已添加很多功能：
可以让所有微软官方asp.net控件具备Ajax能力。
可以轻松在前端页面调用页面中服务端方法，并异步更新页面呈现。
可以在服务端轻松调用客户端脚本。
 
###2011年
AjaxEngine被用于国内某大型证券企业OA的部分模块！
还是2011年，优化了性能，增加了快速编写JsonHandler相关功能！
 
###2012年
正式开源！

###为什么开源？
本来不是什么高深的东西，为什么如数家珍呢？不如分享！
因为AjaxEngine虽实现了相关功能，使用也非常简单方便！但也一直想着好好优化一下，但总不知是真没时间还是偷懒，以致于到现在AjaxEngine中还有09年我刚毕业时写的较为拙劣的代码！希望更多的人参于，优化AjaxEngine
AjaxEngine的思路还是较为巧妙的！
希望让更多的coder快速完成ajax开发！
希望更多的人提出议见以改进AjaxEngine！
 
###AjaxEngine的使用不用任何配置，使用步骤如下：
1. 添加对AjaxEngine的引用
2. 页面继承AjaxPageBase，或实现IAjaxPage 即可
3. JsonHandler继承JsonHandlerBase即可
4. 用AjaxMethod标记相关法！
 
###DEMO

页面使用Demo:

{% highlight csharp %}
 	using System;
 	using AjaxEngines;
 	namespace AjaxEngine.Demo
 	{
     	public partial class _Default : AjaxPageBase
     	{
         	protected void Page_Load(object sender, EventArgs e)
         	{
         	}
         	protected void Button1_Click(object sender, EventArgs e)
         	{
             	this.AjaxEngine.ShowMessageBox("houfeng");
         	}
         	/// <summary>
         	/// 参数和返回值都必须是string类型
         	/// </summary>
         	/// <param name="x"></param>
         	/// <param name="y"></param>
         	/// <returns></returns>
         	[AjaxMethod]
         	public string  Add(string x, string y)
         	{
             	return (x + y).ToString();
         	}
     	}
 	}
{% endhighlight %}
 
Handler使用Demo:

服务端:
{% highlight csharp %}
     using AjaxEngines;
     using AjaxEngines.JsonHandlers;
     namespace AjaxEngine.Demo
     {
         /// <summary>
         /// Handler1 的摘要说明
         /// </summary>
       public class Handler1 : JsonHandlerBase
       {
           [AjaxMethod]
           public int add(int x, int y)
           {
               return x + y;
           }
       }
     }
{% endhighlight %}

客户端:
{% highlight javascript %}
     $.getJSON("Handler1.ashx", { method: "add", x: 1, y: 2 }, function (dt) {
           alert(dt);
     });
{% endhighlight %}

