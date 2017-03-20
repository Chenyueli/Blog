---
layout: post
title: "网站性能优化"
date: 2017-03-19
excerpt: "yahoo性能优化14条："
tag:
- javascript
- HTML
- CSS

comments: false
---
来源：[ 网站优化 14条--雅虎十四条优化原则](http://blog.csdn.net/u010648555/article/details/50721751)

[YaHoo Web优化的14条法则](http://www.cnblogs.com/silverLee/archive/2009/11/05/1596453.html)

<pre>
1. 尽可能地减少HTTP的请求				content
2. 使用CDN					server
3. 添加Experes/cache-control头			server
4. Gzip组件					server
5. 将CSS样式放在页面的上方			CSS
6. 将脚本移动到底部				Javascript
7. 避免使用CSS的Expressions			CSS
8. 将JS和CSS独立成外部文件			JS/CSS
9. 减少DNS查询					content
10. 压缩JS和CSS(包括内联的）			JS/CSS
11. 避免重定向					server
12. 移除重复的脚本				JS
13. 配置实体标签（ETags)				CSS
14. 使AJAX缓存					JS
</pre>


在firefox下有一个插件yslow，集成在firebug中，你可以用它很方便地来看看自己的网站在这几个方面的表现。

### 1. 尽可能地减少HTTP的请求
- 合并css,js
- 背景图片css sprites

首页css和js直接写在页面文件里面，而不是外部引用。因为首页的访问量太大了，这么做可以减少两个请求书。国内很多门户都是这么做的。

减少HTTP请求次数是性能优化的起点。这最提高首次访问的效率起到很重要的作用。40-60%的日常访问是首次访问，因此为首次访问者加快页面访问速度是用户体验的关键。

将首页的几十个小图标合并为一个，通过CSS控制它们的显示，减少了HTTP请求数。

### 5. 将CSS样式放在页面的上方
浏览器在CSS完全加载完毕之后再去渲染


### 8. 将JS和CSS独立成外部文件
把css和js写在页面内容可以减少2次请求，但也增 大了页面的大小。如果已经对css和js做了缓存，那也就没有2次多余的http请求了。当然，我在前面中也说过，有些特殊的页面开发人员还是会选择内联 的css和js文件。有些特殊的页面开发人员还是会选择内联样式。


### 9. 减少DNS查询
在域名和ip地址之间的转换工作称为域名解析，也称DNS查询。

一次DNS的解析过程会消耗20-120毫秒的 时间,在dns查询结束之前，浏览器不会下载该域名下的任何东西。所以减少dns查询的时间可以加快页面的加载速度。yahoo的建议一个页面所包含的域 名数尽量控制在2-4个。


### 10. 压缩JS和CSS
压缩js和css的左右很显然，减少页面字节数。容量小页面加载速度自然也就快。


### 12. 移除重复的脚本
避免重复的代码,开发可重用代码

# 二、个人总结

## html
减少冗余标签、

## css
- 压缩，减少字节数
- 合并文件、图片，减少首页HTTP请求
- 优化：减少嵌套层，减少CSS表达式计算
- 

## Javascript
- 压缩
- 组件化，减少冗余代码

