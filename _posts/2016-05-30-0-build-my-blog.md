---
layout: post
title: "个人建站笔记"
date: 2016-5-30
excerpt: "本文记录了如何在github pages上建立个人网站。github pages,markdown,jekyll。"
project: true
tag:
- HTML
- sass
- CSS3
- javascript
- jquery
- 项目实战

comments: false
---

# 一、笔记

（前端框架：jquery,sass）

## 1、jQuery menu菜单部件
http://www.runoob.com/jqueryui/example-menu.html

	<nav id="dl-menu" class="dl-menuwrapper" role="navigation">
			<button class="dl-trigger">Open Menu</button>
			<ul class="dl-menu">
				<li><a href="{{ site.url }}/">Home</a></li>
				<li>
					<a href="#">Posts</a>
					<ul class="dl-submenu">
						<li><a href="{{ site.url }}/posts/">All Posts</a></li>
						<li><a href="{{ site.url }}/tags/">All Tags</a></li>
					</ul>
				</li>
			</ul><!-- /.dl-menu -->
		</nav><!-- /.dl-menuwrapper -->
		


	// dl-menu options
	$(function() {
	  $( '#dl-menu' ).dlmenu({
	    animationClasses : { classin : 'dl-animate-in', classout : 'dl-animate-out' }
	  });
	});

## jQuery回到顶部插件 
jquery.goup.min.js

## 2、网站动画效果
<a href = "http://daneden.me/animate" target = "_blank">来源：animate.css</a>

例：左侧淡入

	.fadeInLeft{
	animation-name:fadeInLeft;
	-webkit-animation-name:fadeInLeft;
	}
	@keyframes fadeInLeft {
	  from {
	    opacity: 0;
	    -webkit-transform: translate3d(-100%, 0, 0);
	    transform: translate3d(-100%, 0, 0);
	  }
	
	  to {
	    opacity: 1;
	    -webkit-transform: none;
	    transform: none;
	  }
	}

## 2、SASS语法
http://www.w3cplus.com/sassguide/syntax.html

http://sass.bootcss.com/docs/sass-reference/#cache_location-option

## 3、CSS开发样式

- elements.scss 
- nav.scss
- normalize.scss
- 


自适应布局：
.container{
width:80%;
max-width:550px;


}

初始：

	*{box-sizing:border-box;}


- .btn 统一站内button样式
- border-top-right-radius:3px;


		<a href="#CSS">跳到CSS</a>
		<h2 id = "CSS">CSS</h2> 


### 4、CSS3 rem（ie8+）

浏览器默认的字体大小都是16px
- px  
- em	继承父级
- rem	只会相对html


	html,body{
	font-size:100%;
	}
	a{
	font-size:1.25rem;
	}
	h1{
		font-size:2em;		
	}

<a href= "http://www.tuicool.com/articles/eY7NZn" target = "_blank">前端rem单位的正确使用姿势</a>，
<a href= "http://www.jb51.net/css/145926.html" target = "_blank">font-size:100%的目的和作用是什么</a>


# 二、如何在github page 上建站

##### github pages（使用 GitHub 的免费主机）

GitHub Pages免费的静态站点，三个特点：免费托管、自带主题、支持自制页面和Jekyll。

理想写博环境，git+github+markdown+jekyll；

GitHub-Pages无基本的评论，访问统计


#### jekyll模板转化引擎（将纯文本转化为静态网站和博客）

Jekyll是一种简单的、适用于博客的、静态网站生成引擎，支持Markdown格式。基于ruby。它使用一个模板目录作为网站布局的基础框架。

jekyll与github的关系：GitHub Pages一个由 GitHub 提供的用于托管项目主页或博客的服务，jekyll是后台所运行的引擎。

来源：

<a href= "http://jekyll.com.cn/" target = "_blank">jkeyll</a>，
<a href= "http://www.jianshu.com/p/05289a4bc8b2" target="_blank">如何搭建一个独立博客</a>
，<a href= "http://www.pchou.info/ssgithubPage/2013-01-03-build-github-blog-page-01.html" target="_blank">一步步在GitHub上创建博客主页</a>
，<a href = "http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html" target="_blank">通过GitHub Pages建立个人站点（详细步骤）</a>