---
layout: post
title: "移动前端开发系列笔记（1）——移动端开发入门"
date: 2017-03-16
excerpt: "移动前端的一些基本概念"
tag:
- javascript
- CSS
- jquery

comments: true
---
来源：http://junmer.github.io/mobile-dev-get-started/#/

# 基本概念

Native ：本地应用，使用Java/Objective-C/Swift开发

Web App：网页应用HTML5开发

Hybrid App：混合应用（native,web)

Web App 不需要安装，开发成本低，维护简单，跨平台性能优
但体验感较差；

Native 需要安装，开发成本高，维护更新复杂，跨平台性能较差，**但体验感很好！**

Hybrid App 介于两则之间。 需要安装，开发成本居中，维护简单，跨平台性能良好。

#视觉

## 在移动浏览器中使用viewport源标签控制布局
一个典型的针对移动端优化的站点包含类似下面的内容：
<code>
```
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1"/>
```
</code>

width属性控制视口的宽度。可以像width=600这样设为确切的像素数，或者设为device-width这一特殊值来指代比例为100%时屏幕宽度的CSS像素数值。

initial-scale  ：初始缩放比例

maximum-scale、minimum-scale ：允许用户缩放的最大、最小比例

user-scalable：用户是否可以手动缩放。


## 横屏/竖屏

<pre>
window.addEventListener('orientationchange',function(){

//rerender something
});

console.log(window.orientation)//0 90 180 -90顺时针角度

style media = "all and (orientation:portrait)" type = "text/css"
/*竖屏
/style

style media = "all and (orientation:portrait)" type = "text/css"
/*横屏
/style
</pre>


## Flex伸缩布局
<pre>
.parent{
display:-webkit-flex;
display:flex;
}


.initial{
-webkit-flex:initial;
		flex:initial;
width:200px;
min-width:100px;
//空间足够是，宽度为200px,空间不足时为100px，不会更窄
}

.none{
-webkit-flex:none;
		flex:none;
width:200px;
//无论窗口如何变化，宽度一直是200px
}

.flex1{
-webkit-flex:1;
		flex:1;
//占满剩余宽度的1/3
}
.flex2{
-webkit-flex:2;
		flex:2;
//占满剩余宽度的2/3
}

</pre>

## 软键盘

打开数字键盘：
input type = "tel"

## 隐藏地址栏：
setTimeout(funciton（）{window.scrollTo(0,1);},0);

## APPLE-TOUCH-ICON
在iPhone,iPad,iTouch的safari上可以使用添加到主屏按钮将网站添加到主屏幕上
link rel="apple-touch-icon" href="apple-touch-icon-iphone.png" 
link rel="apple-touch-icon" sizes="72x72" href="apple-touch-icon-ipad.png" 
link rel="apple-touch-icon" sizes="114x114" href="apple-touch-icon-iphone4.png" 


# 交互

## Tap
click 有300ms左右延迟，
tap可解决click的延迟，还可以防止穿透（跨页面穿透除外）

## scroll（未经验证）

由于区域滚动 overflow:auto 不靠谱
引入
iscrol 和
saber-scroll
https://github.com/ecomfe/saber-scroll


## 手势
tap:长按或选择一个控制或选项

double tap:之间快速双击页面

drag:指尖在页面上滚动或滑动

flick：指尖快速滑动页面

## CSS3
1. 合理使用渐变、圆角、阴影（别太多，低端机hold不住）
2. 代替js动画：性能好，兼容性好， why not?
3. translate3d：开启GPU硬件加速，提升动画渲染性能

## LocalStorage
.tpl data js img

每个域的最大长度为**5MB**

## 避免
iframe:卡cry,viewport失效，ios宽高失效，fixed定位错误

fixed+input

## 调试

1. 扫浏览器二维码码调试


# Mobile Web 适配总结
来源：https://www.w3ctech.com/topic/979

适配使页面在不同尺寸的手机设备上，页面保持统一效果的等比缩放（看起来差不多）。
## 固定高度，宽度自适应
<pre>
meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1"
</pre>
以较小宽度（如320px)的视觉稿作为参照进行布局，垂直高度、间距使用px,em定值，水平混合使用定值和百分比或者利用弹性。

优点：实现简单，兼容性较好。

缺点：320px 过于窄小，不利于页面的设计；只能设计横向拉伸的元素布局，存在很多局限性。

后两个适配 js 需尽可能早进入，减少（避免）viewport 变化引起的重绘。
## 固定宽度，viewport缩放
精准还原视觉稿

## 利用Rem布局
依照某特定宽度设定 rem 值（即 html 的 font-size），页面任何需要弹性适配的元素，尺寸均换算为 rem 进行布局；


# 移动端开发和Web前端开发的区别是什么
## 普通PC端开发和移动端开发的区别

1. 相较而言，移动端需要兼容的东西少了。

PC端开发，IE6-11,firefox,chrome,safari兼容性

mobile的网页开发，只需考虑webkit内核的浏览器和chrome，uv,qq,小米手机浏览器

2. 移动端的网页开发比PC端网页开发更简单一些。页面笑了，CSS、html少了，交互简单一些，滑动，触屏，手势


# 移动开发规范
http://alloyteam.github.io/Spirit/modules/Standard/





tips:

**iOS中如何禁止用户保存或复制图片**

为一个img标签指定-webkit-touch-callout为none也会禁止设备弹出列表按钮，这样用户就无法保存＼复制你的图片了。


**iOS中如何禁止用户选中文字**

我们通过指定文字标签的-webkit-user-select属性为none便可以禁止iOS用户选中文字。



**iOS中如何获取滚动条的值**

桌面浏览器中想要获取滚动条的值是通过document.scrollTop和document.scrollLeft得到的，但在iOS中你会发现这两个属性是未定义的，为什么呢？因为在iOS中没有滚动条的概念，在Android中通过这两个属性可以正常获取到滚动条的值，那么在iOS中我们该如何获取滚动条的值呢？

通过window.scrollY和window.scrollX我们可以得到当前窗口的y轴和x轴滚动条的值。


**如何解决盒子边框溢出**


当你指定了一个块级元素时，并且为其定义了边框，设置了其宽度为100％。在移动设备开发过程中我们通常会对文本框定义为宽度100％，将其定义为块级元素以实现全屏自适应的样式，但此时你会发现，该元素的边框(左右)各1个像素会溢了文档，导致出现横向滚动条，为解决这一问题，我们可以为其添加一个特殊的样式-webkit-box-sizing:border-box;用来指定该盒子的大小包括边框的宽度。

### 附：移动端Web app开发

移动端Web app在页面头部加入下面这句话：
<pre>
meta name="apple-mobile-web-app-capable" content="yes"
</pre>
这个meta的作用是让普通网页被添加到主屏幕后，拥有一些类native的功能，如隐藏ios的上下状态栏，实现全屏，禁止弹性拖拽，修改顶部颜色等。

另外，手机淘宝、手机美团、手机微博的网页版，大家打开的时候，不是全屏的，但是用起来，开发者把他们伪装得很想这种web app的交互体验而已。(下滑后上状态栏被隐藏)

综上，可以总结为web app

#### 对js和css的选择
主流移动端框架有jq mobile ,zepto, backbone ,angular


如zepto中有几个很有用的事件，swipe,swipeLeft,right,up,down和tap

做了许多兼容处理，稍显多余，统计数据显示目前只需要考虑webkit和几个特定国产浏览器。


