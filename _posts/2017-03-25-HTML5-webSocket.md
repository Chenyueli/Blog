---
layout: post
title: "webSocket双向通信"
date: 2017-03-25
excerpt: "WebSocket API是下一代客户端-服务器的异步通信方法,用于双向推送消息，多用于实时性要求很高（秒级）的场合，如在实时聊天（在浏览器内）、直播平台等"
tag:
- HTML5

comments: false
---

客户端支持：IE9+,IOS 5+,Android 4.5+

WebSocket 客户端 API 示例:

	if (window.WebSocket) {
		var ws = new WebSocket(“ws://echo.websocket.org”); 
		 ws.onopen = function(){ws.send(“Test!”); }; 
		 ws.onmessage = function(evt){console.log(evt.data);ws.close();}; 
		 ws.onclose = function(evt){console.log(“WebSocketClosed!”);}; 
		 ws.onerror = function(evt){console.log(“WebSocketError!”);};
	}else{
		console.log("您的设备不支持 webSocket!");
	}



- 申请一个`WebSocket`对象，参数是需要连接的服务器端的地址，WebSocket 协议的 URL 使用 ws://开头，安全的 WebSocket 协议使用 wss://开头。
- 当 Browser 和 WebSocketServer 连接成功后，会触发 onopen 消息；
- 如果连接失败，发送、接收数据失败或者处理数据出现错误，browser 会触发 onerror 消息；
- 当 Browser 接收到 WebSocketServer 发送过来的数据时，就会触发 onmessage 消息，参数 `evt` 中包含 Server 传输过来的数据；
- 当 Browser 接收到 WebSocketServer 端发送的关闭连接请求时，就会触发 onclose 消息。
 
 
我们可以看出所有的操作都是采用异步回调的方式触发，这样不会阻塞 UI，可以获得更快的响应时间，更好的用户体验。




来源：<a href = "http://www.ibm.com/developerworks/cn/java/j-lo-WebSocket/" target = "_blank">WebSocket 实战</a>

[ 前端解决跨域问题的8种方案](http://blog.csdn.net/joyhen/article/details/21631833)

## 一、浅谈js跨域问题
所谓跨域，或者异源，是指主机名（域名）、协议、端口号只要有其一不同，就为不同的域（或源）。

### 1. cors
### 2. Jsonp跨域
在页面中使用`<script>`引入存储在其他域的脚本文件：

	<script src="http://www.a.com/index.js"></script>

除了`<script>`，还有`<img>`、`<iframe>`、`<link>`等都具有跨域加载资源的能力。

	<script>
	function handleData(data) {
	    //处理数据
	}
	</script>
	<script src="http://www.a.com/getData.do?callback=handleData"></script>

也可以使用jquery封装的方法，如$.ajax:

	<script>
	function hadnleData(data) {
	    //处理数据
	}
	$.ajax({
	    url: 'http://www.a.com/getData.do?callback=?',
	    type: "GET",
	    dataType: "jsonp",
	    jsonpCallback: "handleData"
	});
	</script>


### 3. 使用HTML5的window.postMessage方法:

	//a.com页面
	window.postMesssage(JSON.stringify({xxx:'test'},'b.com');
	//b.com页面
	window.onMessage=function(e){
	    var data = JSON.parse(e.data);
	    console.log(data); //{xxx:'test'}
	}

[浅谈js跨域问题](https://segmentfault.com/a/1190000003784372)