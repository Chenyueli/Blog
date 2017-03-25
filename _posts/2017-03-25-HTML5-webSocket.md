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

	var ws = new WebSocket(“ws://echo.websocket.org”); 
	 ws.onopen = function(){ws.send(“Test!”); }; 
	 ws.onmessage = function(evt){console.log(evt.data);ws.close();}; 
	 ws.onclose = function(evt){console.log(“WebSocketClosed!”);}; 
	 ws.onerror = function(evt){console.log(“WebSocketError!”);};

实现实时消息的通知及推送，



来源：<a href = "http://www.ibm.com/developerworks/cn/java/j-lo-WebSocket/" target = "_blank">WebSocket 实战</a>