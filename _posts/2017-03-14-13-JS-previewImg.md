---
layout: post
title: "HTML5之FileReader的使用"
date: 2017-03-19
excerpt: "JS实现图片上传和预览"
tag:
- javascript

comments: false
---
## 一、FileReader
FileReader API 用于读取文件，即把文件内容读入内存。它的参数是 File 对象或 Blob 对象。

对于不同类型的文件，FileReader 提供不同的方法读取文件。


- readAsText(Blob|File, opt_encoding)：返回文本字符串。默认情况下，文本编码格式是 UTF-8，可以通过可选的格式参数，指定其他编码格式的文本。
- readAsDataURL(Blob|File)：返回一个基于 Base64 编码的 data-uri 对象
- readAsArrayBuffer()：返回一个 ArrayBuffer 对象。

FileReader 对象采用异步方式读取文件，可以为一系列事件指定回调函数。



- onabort 方法：读取中断或调用 reader.abort() 方法时触发。
- onerror 方法：读取出错时触发。
- onload 方法：读取成功后触发。
- onloadend 方法：读取完成后触发，不管是否成功。触发顺序排在 onload 或 onerror 后面。
- onloadstart 方法：读取将要开始时触发。
- onprogress 方法：读取过程中周期性触发。（可以用来获取文件读取的进度）

## 一、FileReader实现小文件直接嵌入文档（IE9+)

调用FileReader对象的方法：

readAsDataURL：将文件读取为一段以 data: 开头的字符串，这段字符串的实质就是 Data URL，Data URL是一种**将小文件直接嵌入文档**的方案。这里的小文件通常是指图像与 html 等格式的文件。

处理事件:

onload:文件读取成功完成时触发
	
	<img src = "" id = "show"/>
	<input type = "file" id = "img" onchange = "showPreview(this)"/>

		//js读取图片并转成base64编码,并实现图片预览
        function showPreview(source) {  
            var file = source.files[0];  
            if(window.FileReader) {  
                var fr = new FileReader();  
                fr.onloadend = function(e) {  
                    document.getElementById("show").src = e.target.result;
                    alert(e.target.result); //base64编码
                };  
                fr.readAsDataURL(file);  
            }  
        }




[HTML5之FileReader的使用](http://blog.csdn.net/yaoyuan_difang/article/details/38582697)

## 二、图片预览兼容
 path = document.selection.createRange().text;//IE
path=window.URL.createObjectURL(imgFile.files[0]);// FF 7.0以上


 					var path;
	                if(document.all)//IE
	                {
	                    imgFile.select();
	                    path = document.selection.createRange().text;
	                    document.getElementById("imgPreview").innerHTML="";
	                    document.getElementById("imgPreview").style.filter = "progid:DXImageTransform.Microsoft.AlphaImageLoader(enabled='true',sizingMethod='scale',src=\"" + path + "\")";//使用滤镜效果
	                }
	                else//FF
	                {
	                    path=window.URL.createObjectURL(imgFile.files[0]);// FF 7.0以上
	                    //path = imgFile.files[0].getAsDataURL();// FF 3.0
	                    document.getElementById("imgPreview").innerHTML = "<img id='img1' width='120px' height='100px' src='"+path+"'/>";
	                    //document.getElementById("img1").src = path;
	                }

兼容最新firefox、chrome和IE的javascript图片预览实现代码：（传给后台url,后台可读取文件）

	<!DOCTYPE html>
	<html>
	<head>
	    <meta charset="UTF-8">
	    <title>js图片上传预览</title>
	    <script>
	        function PreviewImage(imgFile)
	        {
	            var filextension=imgFile.value.substring(imgFile.value.lastIndexOf("."),imgFile.value.length);
	            filextension=filextension.toLowerCase();
	            if ((filextension!='.jpg')&&(filextension!='.gif')&&(filextension!='.jpeg')&&(filextension!='.png')&&(filextension!='.bmp'))
	            {
	                alert("对不起，系统仅支持标准格式的照片，请您调整格式后重新上传，谢谢 !");
	                imgFile.focus();
	            }
	            else
	            {
	                var path;
	                if(document.all)//IE
	                {
	                    imgFile.select();
	                    path = document.selection.createRange().text;
	                    document.getElementById("imgPreview").innerHTML="";
	                    document.getElementById("imgPreview").style.filter = "progid:DXImageTransform.Microsoft.AlphaImageLoader(enabled='true',sizingMethod='scale',src=\"" + path + "\")";//使用滤镜效果
	                }
	                else//FF
	                {
	                    path=window.URL.createObjectURL(imgFile.files[0]);// FF 7.0以上
	                    //path = imgFile.files[0].getAsDataURL();// FF 3.0
	                    document.getElementById("imgPreview").innerHTML = "<img id='img1' width='120px' height='100px' src='"+path+"'/>";
	                    //document.getElementById("img1").src = path;
	                }
	            }
	        }
	    </script>
	</head>
	<body>
	<input type="file" onchange='PreviewImage(this)' />
	<br />
	<div id="imgPreview" style='width:120px; height:100px;'>
	    <img id="img1" src="" width="120" height="100" />
	</div>
	</body>
	</html> 


更多参考：
[HTML5 File API — 让前端操作文件变的可能](http://www.cnblogs.com/zichi/p/html5-file-api.html)