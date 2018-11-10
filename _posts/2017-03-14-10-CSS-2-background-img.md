---
layout: post
title: "认真理解背景图片 background-img"
date: 2017-03-14
excerpt: ""
tag:
- CSS
- CSS3

comments: true
---

# 一、现象
1. 图片的内部样式会覆盖内联样式 height width
2. 图片的max-width脂类的只是给图片设置一个阈值，不会覆盖内联样式
3. **宽度和高度，只设定一个，另一个会按照图片默认的宽高比例自动调整**
4. 同时设置宽高，图片比例变了，图片失真，除非设置比例一样。

# 二、响应式背景图

**响应式图片的思路就是宽高参照容器大小。 **

1. 媒体查询，根据设置分辨率加载相应大小的图片哦。节省带宽。   
2. background-size:100%; background-repeat:none;background-attachent:fixed;

<pre>
//一个移动端背景图片的应用实例 640px 已验证
body {
    background: url("../img/bg.jpg");
	background-repeat: none;
	background-size: 100%;
    background-attachment: fixed;
}
</pre>
background-attachment设置或检索背景图像是随对象内容滚动还是固定的；
fixed：固定

<pre>
//方案2：待验证
body {
    background: url("../img/bg.jpg");
	background-repeat: none;
	background-size: cover;
    background-attachment: fixed;
	background-position:center;
}
</pre>

附：背景图片中：

width:100%； 相对父容器，图片可缩可放:  

**max-width:100%;相对图片本身，图片只缩不放： **

[background-size图例](http://www.w3school.com.cn/tiy/c.asp?f=css_background-size&p=7)


# 三、background 图像相关的一些常用的属性

## 1、background-size
background-size: length|percentage|cover|contain;//默认auto图像大小

## 2、background-repeat
background-repeat: repeat|repeat-x|repeat-y|no-repeat|inherit;//默认repeat垂直方向和竖直方向重复

## 3、background-attachment
background-attachment: scroll|fixed|inherit;//默认背景图像随页面滚动
## 4、background-origin
background-origin: padding-box|border-box|content-box;默认相对内边距padding定位
## 5、background-position
background-position:center;|top center|center|bottom left|默认0%,0%，左上角位置

注意：需要把 background-attachment 属性设置为 "fixed"，才能保证该属性在 Firefox 和 Opera 中正常工作。