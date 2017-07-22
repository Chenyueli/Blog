---
layout: post
title: "性能优化（2）——基础知识梳理 "
date: 2017-07-06
excerpt: "性能指标；检测工具；浏览器工作原理；常用优化总结；"
tag:
- 性能

comments: false
---

# 网站性能衡量指标：
网页性能衡量指标有很多，倘若能够把握关键的几个，集中优化，性能自然也就上去了。

### 白屏时间
用户从打开页面开始到页面开始有东西呈现为止，这过程中占用的时间就是白屏时间。chrome 高版本有 `firstPaintTime` 接口来获取这个耗时，但大部分浏览器并不支持，必须想其他办法来监测。 通过获取头部资源加载完的时刻来近似统计白屏时间。尽管并不精确，但却考虑了影响白屏的主要因素：首字节时间和头部资源加载时间。
### 首屏时间
用户浏览器首屏内所有内容都呈现出来所花费的时间。
如果在首屏中充满了大尺寸图片或者客户端与后端建立连接次数较多，首屏 时间也会相应被拖长。研究认为，用户最满意的网页打开时间是<b style = 'color:red'>2秒以内</b>。作为一个相对简单的页面，我们就应该最可能将首屏时间甚至加载时间控制在2秒以内，让用户体验到最佳的页面体验。
### 用户可操作时间
用户可以进行正常的点击、输入等操作。默认统计domready事件，因为通常会在这时候绑定事件操作。

### 页面总下载时间
页面所有资源都加载完成并呈现出来所花的时间，即页面 onload 的时间。为了保障页面的加载速度，很多内容不会在页面打开的时候全部加载到客户端。
### 流畅度
这里提到的流畅度是等待过程中的视觉缓冲，通常为保障，必须减少卡顿，想办法分拆执行时间超过<b style = "color:red"> 80ms </b>的代码程序，这并不简单。

### 自定义的时间
对于开发人员来说，完全可以自定义一些时间点，例如：某个组件 init 完成的时间、某个重要模块加载的时间等等。

## 小结
页面优化的切入点很多，我们不一定能够面面俱到，但是对于一个承载较大流量的页面来说，下面几条必须有效执行：

1. 首屏一定要快
2. 滚屏一定要流畅
3. 能不加载的先别加载
4. 能不执行的先别执行
5. 渐进展现、圆滑展现

当然，性能优化的切入角度不仅仅是上几个方面，对照 Chrome 的 `Timeline` 柱状图和折线图，我们还可以找到很多优化的点。

# 测试工具
页面性能的评估与监控有很多成熟优秀的工具，合理利用已有工具能达到事半功倍的效果。下面简单介绍几个常用的工具：

## page speed
基于firebug的web页面优化的评测工具，同时还有支持chrome的插件,chrome应用商店可安装;

其实这个更像是代码的<b style='color:red'>白盒测试分析工具</b>，根据一定的规范来检测网页的优化程度，而不是实际的去监听和过滤页面访问所花费的时间。这只是一个最佳实践规则和建议的检测工具；如果想看页面访问时间的细节，firebug和chrome的开发人员工具本身就已经有了。

## WebPagetest
开源的网站性能评价系统，无需注册便可测试。http://www.webpagetest.org/
可以查看首屏时间、加载时间、渲染时间、优化方向等；
指标说明：

### 数据指标说明
<img src = "{{ site.url }}/assets/img/post/webpagetest1.png" width="300" alt="数据指标说明" >

### 优化等级说明
<img src = "{{ site.url }}/assets/img/post/webpagetest2.png" width="300" alt="优先等级" >

# 浏览器的工作原理

要考虑网站的性能，自然和浏览器的工作原理分不开。让我们一起来回顾一下这些经典问题：

## 一. 在浏览器输入一个URL后，会发生什么？


### 1. 在浏览器里输入要网址: facebook.com
### 2. 浏览器查找域名的IP地址
通过访问的域名找出其IP地址。DNS（域名系统）查找过程如下：
1.浏览器缓存->操作系统缓存->路由器缓存->ISP DNS缓存->递归搜索（ 你的ISP的DNS服务器从跟域名服务器开始进行递归搜索，从.com顶级域名服务器到Facebook的域名服务器。一般DNS服务器的缓存中会有.com域名服务器中的域名，所以到顶级服务器的匹配过程不是那么必要了。）

### 3. 浏览器给web服务器发送一个HTTP请求

因为像Facebook主页这样的动态页面，打开后在浏览器缓存中很快甚至马上就会过期，毫无疑问他们不能从中读取。


GET 这个请求定义了要读取的URL： “http://facebook.com/”

	GET http://facebook.com/ HTTP/1.1
	 Accept: application/x-ms-application, image/jpeg, application/xaml+xml, [...]
	 User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; [...]
	 Accept-Encoding: gzip, deflate
	 Connection: Keep-Alive
	 Host: facebook.com
	 Cookie: datr=1265876274-[...]; locale=en_US; lsd=WW[...]; c_user=2101[...]

GET 这个请求定义了要读取的URL： “http://facebook.com/”。 浏览器自身定义 (User-Agent 头)， 和它希望接受什么类型的相应 (Accept andAccept-Encoding 头). Connection头要求服务器为了后边的请求不要关闭TCP连接。


请求中也包含浏览器存储的该域名的cookies。可能你已经知道，在不同页面请求当中，cookies是与跟踪一个网站状态相匹配的键值。这样cookies会存储登录用户名，服务器分配的密码和一些用户设置等。Cookies会以文本文档形式存储在客户机里，每次请求时发送给服务器。

### 4. facebook服务的永久重定向响应
facebook服务器发回给浏览器的响应

	HTTP/1.1 301 Moved Permanently
	 Cache-Control: private, no-store, no-cache, must-revalidate, post-check=0,
	 pre-check=0
	 Expires: Sat, 01 Jan 2000 00:00:00 GMT
	 Location: http://www.facebook.com/
	 P3P: CP="DSP LAW"
	 Pragma: no-cache
	 Set-Cookie: made_write_conn=deleted; expires=Thu, 12-Feb-2009 05:09:50 GMT;
	 path=/; domain=.facebook.com; httponly
	 Content-Type: text/html; charset=utf-8
	 X-Cnection: close
	 Date: Fri, 12 Feb 2010 05:09:51 GMT
	 Content-Length: 0

服务器给浏览器响应一个301永久重定向响应，这样浏览器就会访问“http://www.facebook.com/” 而非“http://facebook.com/”。

为什么服务器一定要重定向而不是直接发会用户想看的网页内容呢？这个问题有好多有意思的答案。

其中一个原因跟搜索引擎排名有 关。你看，如果一个页面有两个地址，就像http://www.igoro.com/ 和http://igoro.com/，搜索引擎会认为它们是两个网站，结果造成每一个的搜索链接都减少从而降低排名。而搜索引擎知道301永久重定向是 什么意思，这样就会把访问带www的和不带www的地址归到同一个网站排名下。

还有一个是用不同的地址会造成缓存友好性变差。当一个页面有好几个名字时，它可能会在缓存里出现好几次。

### 5. 浏览器跟踪重定向地址

现在，浏览器知道了“http://www.facebook.com/”才是要访问的正确地址，所以它会发送另一个获取请求：

	GET http://www.facebook.com/ HTTP/1.1
	 Accept: application/x-ms-application, image/jpeg, application/xaml+xml, [...]
	 Accept-Language: en-US
	 User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; [...]
	 Accept-Encoding: gzip, deflate
	 Connection: Keep-Alive
	 Cookie: lsd=XW[...]; c_user=21[...]; x-referer=[...]
	 Host: www.facebook.com

头信息以之前请求中的意义相同。

### 6. 服务器“处理”请求,发回一个HTML响应
服务器接收到获取请求，然后处理并返回一个响应。

	HTTP/1.1 200 OK
	 Cache-Control: private, no-store, no-cache, must-revalidate, post-check=0,
	 pre-check=0
	 Expires: Sat, 01 Jan 2000 00:00:00 GMT
	 P3P: CP="DSP LAW"
	 Pragma: no-cache
	 Content-Encoding: gzip
	 Content-Type: text/html; charset=utf-8
	 X-Cnection: close
	 Transfer-Encoding: chunked
	 Date: Fri, 12 Feb 2010 09:05:55 GMT
	 
	 2b3Tn@[...]

内容编码头告诉浏览器整个响应体用gzip算法进行压缩。解压blob块后，你可以看到如下期望的HTML：

关于压缩，头信息说明了是否缓存这个页面，如果缓存的话如何去做，有什么cookies要去设置（前面这个响应里没有这点）和隐私信息等等。

请注意报头中把Content-type设置为“text/html”。报头让浏览器将该响应内容以HTML形式呈现，而不是以文件形式下载它。浏览器会根据报头信息决定如何解释该响应，不过同时也会考虑像URL扩展内容等其他因素。

### 7. 浏览器开始显示HTML

在浏览器没有完整接受全部HTML文档时，它就已经开始显示这个页面了：

### 8. 浏览器发送获取嵌入在HTML中的对象
在浏览器显示HTML时，它会注意到需要获取其他地址内容的标签。这时，浏览器会发送一个获取请求来重新获得这些文件。

这些地址都要经历一个和HTML读取类似的过程。所以浏览器会在DNS中查找这些域名，发送请求，重定向等等...

但 不像动态页面那样，静态文件会允许浏览器对其进行缓存。有的文件可能会不需要与服务器通讯，而从缓存中直接读取。服务器的响应中包含了静态文件保存的期限 信息，所以浏览器知道要把它们缓存多长时间。还有，每个响应都可能包含像版本号一样工作的ETag头（被请求变量的实体值），如果浏览器观察到文件的版本 ETag信息已经存在，就马上停止这个文件的传输。

注明：chrome下提供一个命令chrome://cache，可以查看到保留下来的缓存
<img src = "{{ site.url }}/assets/img/post/browsercache.png" width="300" alt="浏览器缓存" >
<img src = "{{ site.url }}/assets/img/post/browsercache2.png" width="300" alt="浏览器缓存" >

### 9. 浏览器发送异步（AJAX）请求
What really happens when you navigate to a URL：
[http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)

## 二、 HTML页面加载和解析流程

1. 用户输入网址（假设是个html页面，并且是第一次访问），浏览器向服务器发出请求，服务器返回html文件；
2. 浏览器开始载入html代码，发现＜head＞标签内有一个＜link＞标签引用外部CSS文件；
3. 浏览器又发出CSS文件的请求，服务器返回这个CSS文件；
4. 浏览器继续载入html中＜body＞部分的代码，并且CSS文件已经拿到手了，可以开始渲染页面了；
5. 浏览器在代码中发现一个＜img＞标签引用了一张图片，向服务器发出请求。此时浏览器不会等到图片下载完，而是继续渲染后面的代码；
6. 服务器返回图片文件，由于图片占用了一定面积，影响了后面段落的排布，因此浏览器需要回过头来重新渲染这部分代码；
7. 浏览器发现了一个包含一行Javascript代码的＜script＞标签，赶快运行它；
8. Javascript脚本执行了这条语句，它命令浏览器隐藏掉代码中的某个＜div＞ （style.display=”none”）。突然少了这么一个元素，浏览器不得不重新渲染这部分代码；
9. 终于等到了＜/html＞的到来，浏览器泪流满面……
10. 等等，还没完，用户点了一下界面中的“换肤”按钮，Javascript让浏览器换了一下＜link＞标签的CSS路径；
11. 浏览器召集了在座的各位＜div＞＜span＞＜ul＞＜li＞们，“大伙儿收拾收拾行李，咱得重新来过……”，浏览器向服务器请求了新的CSS文件，重新渲染页面。

### 回流与重绘

1. 当render tree中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建。这就称为回流(reflow)。每个页面至少需要一次回流，就是在页面第一次加载的时候。在回流的时候，浏览器会使渲染树中受到影响的部分失效，并重新构造这部分渲染树，完成回流后，浏览器会重新绘制受影响的部分到屏幕中，该过程成为重绘。

2. 当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。

注意：回流必将引起重绘，而重绘不一定会引起回流。

**回流何时发生**：

当页面布局和几何属性改变时就需要回流。下述情况会发生浏览器回流：

1. 添加或者删除可见的DOM元素；
2. 元素位置改变；
3. 元素尺寸改变——边距、填充、边框、宽度和高度
4. 内容改变——比如文本改变或者图片大小改变而引起的计算值宽度和高度改变；
5. 页面渲染初始化；
6. 浏览器窗口尺寸改变——resize事件发生时；

**如何减少回流、重绘**

## 三、 如何加快HML页面加载速度
了解了浏览器的原理后，对如何提升加载解析渲染

- 页面减肥

页面的肥瘦是影响加载速度最重要的因素。删除不必要的空格、注释；将inline的script和css移到外部文件；可以使用HTML Tidy来给HTML减肥，还可以使用一些压缩工具来给JavaScript减肥

- 减少文件数量

减少页面上引用的文件数量可以减少HTTP连接数。许多JavaScript、CSS文件可以合并最好合并；

- 减少域名查询

DNS查询和解析域名也是消耗时间的，所以要减少对外部JavaScript、CSS、图片等资源的引用，不同域名的使用越少越好

- 缓存重用数据

- 优化页面元素加载顺序

首先加载页面最初显示的内容和与之相关的JavaScript和CSS，然后加载HTML相关的东西。像什么不是最初显示相关的图片、flash、视频等很肥的资源就最后加载。

- 减少inline JavaScript的数量

浏览器parser会假设inline JavaScript会改变页面结构，所以使用inline JavaScript开销较大。不要使用document.write()这种输出内容的方法，使用现代W3C DOM方法来为现代浏览器处理页面内容

- 使用现代CSS和合法的标签

使用现代CSS来减少标签和图像，例如使用现代CSS+文字完全可以替代一些只有文字的图片；使用合法的标签避免浏览器解析HTML时做“error correction”等操作，还可以被HTML Tidy来给HTML减肥

- 指定图像和tables的大小

如果浏览器可以立即决定图像或tables的大小，那么它就可以马上显示页面而不要重新做一些布局安排的工作；这不仅加快了页面的显示，也预防了页面完成加载后布局的一些不当的改变image使用height和width；table使用table-layout: fixed并使用col和colgroup标签指定columns的width

- 根据用户浏览器明智的选择策略
IE、Firefox、Safari等等等等
来源：http://www.51testing.com/html/38/225738-220986.html

http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/
浏览器渲染原理（难懂经典） 值得一看

## 小结
1.白屏时间

减少head中HTTP请求数量，减少Head 加载时间，从而降低白屏时间。
（包括：避免重定向、利用缓存、使用CDN、合并文件减少加载数、压缩文件）

2.首屏时间

提高页面渲染速度（CSS放在头部、书写高效CSS，减少DOM操作次数：减少重绘和回流，按需加载/分页，减少dom元素的数量）


参考来源：
1. <a href = 'http://www.cnblogs.com/chenxizhang/category/194099.html'>优化网站设计</a>
2. <a href = 'http://www.zhangxinxu.com/wordpress/2010/09/%E7%B2%BE%E7%AE%80%E9%AB%98%E6%95%88%E7%9A%84css%E5%91%BD%E5%90%8D%E5%87%86%E5%88%99%E6%96%B9%E6%B3%95/'>精简高效CSS命名准则方法</a>
3. <a href = http://fex.baidu.com/blog/2014/05/build-performance-monitor-in-7-days/'>7天打造前端性能监控系统</a>

入门：
<a href = 'http://mangguo.org/browser-cache-mechanism-detailed/'>浏览器缓存机制</a>
<a href = 'http://www.ecdoer.com/post/dns.html'>DNS解析过程原理</a>
进阶：
<a href = 'https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/'>浏览器的工作原理（经典／难懂）</a>



