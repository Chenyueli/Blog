---
layout: post
title: "CSS3语法总结"
date: 2017-03-14
excerpt: ""
tag:
- CSS3

comments: true
---

Internet Explorer 10、Firefox 以及 Opera 支持 @keyframes 规则和 animation 属性。
注意：Internet Explorer 9 及更早 IE 版本不支持 CSS 动画。
# 一、CSS的动画属性

一些 CSS 属性是可以有动画效果的，这意味着它们可以用于动画和过渡。
动画属性可以逐渐地从一个值变化到另一个值，比如尺寸大小、数量、百分比和颜色。

CSS的动画属性：
来源：[菜鸟教程CSS动画](http://www.runoob.com/cssref/css-animatable.html)
<pre>
backgound、background-color、background-size
border、border-raius、border-bottom-width、border-color
box-shadow
columns、column-count
flex、flex-shrink
font、font-size、font-weight
height、max-width、min-height、left、padding、line-height、margin
letter-spacing、word-space、text-indent
opacity
transform
z-index
</pre>

# 二、CSS3动画
通过 CSS3，我们能够创建动画，这可以在许多网页中取代动画图片、Flash 动画以及 JavaScript。
注释：Internet Explorer 9，以及更早的版本，不支持 @keyframe 规则或 animation 属性。
## 1. @keyframes 规则

语法：
<pre>@keyframes animationname {keyframes-selector {css-styles;}}
</pre>
animationname：必需。定义动画的名称。

keyframes-selector：必需。动画时长的百分比。
合法的值：
0-100%
from（与 0% 相同）
to（与 100% 相同）

css-styles	：必需。一个或多个合法的 CSS 样式属性。


<pre>
<code>
//一个例子：
@keyframes mymove
{
0%   {top:0px;}
25%  {top:200px;}
50%  {top:100px;}
75%  {top:200px;}
100% {top:0px;}
}
</code>
</pre>

## 2. CSS3动画属性animation

用法：
<pre>
animation: name duration timing-function delay iteration-count direction;

div{
animation : myfirst 5s linear 2s infinite alternate;
}

@keyframes myfirst
{
0%   {top:0px;}
25%  {top:200px;}
50%  {top:100px;}
75%  {top:200px;}
100% {top:0px;}
}
</pre>

**所有动画属性**的简写属性，用于设置6个动画属性：

- animation-name 规定需要绑定到选择器的keyframe名称  keyframename/none 名称/无动画效果
- animation-duration 动画时间 s或ms 默认为0，无动画
- animation-delay 动画开始前的延迟，默认为0
- animation-direction 是否轮流反向播放动画 normal/alternate 正常播放、反向播放
- animation-iteration-count 规定动画播放次数 n/infinite
- animation-timing-function 规定动画的速度曲线
   linear(匀速）ease(默认，动画先低速-加快-慢) ease-in（低速开始） ease-out(低速结束） ease-in-out (低速开始和结束）
- animation-play-state 动画运行或暂停 默认running|paused

兼容性案例:
<pre>
div{
animation : myfirst 5s linear 2s infinite alternate;
-moz-animation: myfirst 5s linear 2s infinite alternate;/* Firefox */
-webkit-animation: myfirst 5s linear 2s infinite alternate; /* Safari 和 Chrome */
-o-animation: myfirst 5s linear 2s infinite alternate;/* Opera */
}

@keyframes myfirst
{
0%   {top:0px;}
25%  {top:200px;}
50%  {top:100px;}
75%  {top:200px;}
100% {top:0px;}
}
@-moz-keyframes myfirst /* Firefox */
{
0%   {top:0px;}
25%  {top:200px;}
50%  {top:100px;}
75%  {top:200px;}
100% {top:0px;}
}
@-webkit-keyframes myfirst /* Safari 和 Chrome */
{
0%   {top:0px;}
25%  {top:200px;}
50%  {top:100px;}
75%  {top:200px;}
100% {top:0px;}
}
@-o-keyframes myfirst /* Opera */
{
0%   {top:0px;}
25%  {top:200px;}
50%  {top:100px;}
75%  {top:200px;}
100% {top:0px;}
}
</pre>

# 三、CSS3过渡属性：transition
语法：
<pre>
transition: property duration timing-function delay;

div{
width:100px;
transition:width:2s;
-moz-transition:width 2s; /* Firefox 4 */
	-webkit-transition:width 2s; /* Safari and Chrome */
	-o-transition:width 2s; /* Opera */
}
div:hover{
width:300px;
}
</pre>
- transition-property:规定设置过渡效果的CSS属性的名称
- transition-duration:过渡效果时间 s/ms
- transition-timing-function 速度曲线
- transition-delay 定义过渡效果何时开始
- 
# 三、CSS3 2D转换：transform
translate(),rotate(),scale(),skew(),matrix()
**平移**、**旋转**、**放大**、**翻转**

- translate(50px,100px) 把元素从左侧移动 50 像素，从顶端移动 100 像素。
- rotate(30deg) 把元素顺时针旋转 30 度。
- scale(2,4) 把宽度转换为原始尺寸的 2 倍，把高度转换为原始高度的 4 倍。
- skew(30deg,20deg) 围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 20 度。
- matrix(n,n,n,n,n,n)	

语法：

<pre>
div{
	transform: skew(30deg,20deg);
	-ms-transform: skew(30deg,20deg);	/* IE 9 */
	-webkit-transform: skew(30deg,20deg);	/* Safari and Chrome */
	-o-transform: skew(30deg,20deg);	/* Opera */
	-moz-transform: skew(30deg,20deg);	/* Firefox */
}
</pre>
注意：Chrome 和 Safari 需要前缀 -webkit-。

# 四、CSS3字体 @font-face
可用CSS3 @font-face 规则自定义字体。

<pre>
@font-face
{
font-family: myFirstFont;
src: url('Sansation_Bold.ttf'),
     url('Sansation_Bold.eot'); /* IE9+ */
font-weight:bold;
}

div{
font-family:myFirstFont;
}
</pre>

- font-family:必需。规定字体的名称
- URL 	必需。定义字体文件的 URL。
- font-style  字体样式；默认是 "normal"  italic斜体字  oblique使倾斜
- font-weight 字体粗细；默认normal,bold 100 200 -900

italic和oblique都是向右倾斜的文字, 但区别在于Italic是指斜体字，而Oblique是**倾斜的文字**。
可以理解成Italic是使用文字的斜体，Oblique是**让没有斜体属性的文字倾斜**！

# 五、CSS3背景
## background-size

设置背景图像的尺寸——宽度和高度
语法：
<pre>
background-size: length|percentage|cover|contain;
</pre>

- auto默认加载图像的大小
- percentage:父元素的百分比
- cover:把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。图像某些部分也许无法显示在背景定位区域中。
- contain:把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。

只设置一个值，则第二个值为auto
[图例](http://www.w3school.com.cn/tiy/c.asp?f=css_background-size&p=7)

## backgroun-origin
background-origin 属性规定 background-position 属性相对于什么位置来定位。
语法：
<pre>
background-origin: padding-box|border-box|content-box;
</pre>
图示：[3种参考定位](http://www.w3school.com.cn/tiy/c.asp?f=css_background-origin)


相对于内容框来定位背景图像：
<pre>
div
{
background-image:url('smiley.gif');
background-repeat:no-repeat;
background-position:left;
background-origin:content-box;
}
</pre>