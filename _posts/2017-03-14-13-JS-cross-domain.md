---
layout: post
title: "浅谈js跨域问题"
date: 2017-03-25
excerpt: "前端跨域请求JS"
tag:
- HTML5

comments: false
---

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