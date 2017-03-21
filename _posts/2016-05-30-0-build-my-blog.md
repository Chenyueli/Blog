---
layout: post
title: "个人建站笔记"
date: 2016-5-30
excerpt: "本文记录了如何在github pages上建立个人网站。git,markdown,bootstrap,jekyll。"
project: true
tag:
- HTML
- sass
- javascript
- jquery
- 项目实战

comments: false
---

## 建站git,markdown,bootstrap,jekyll
##### github pages

GitHub Pages免费的静态站点，三个特点：免费托管、自带主题、支持自制页面和Jekyll。

理想写博环境，git+github+markdown+jekyll；

GitHub-Pages无基本的评论，访问统计


#### jekyll模板转化引擎

Jekyll是一种简单的、适用于博客的、静态网站生成引擎，支持Markdown格式。基于ruby。它使用一个模板目录作为网站布局的基础框架。

jekyll与github的关系：GitHub Pages一个由 GitHub 提供的用于托管项目主页或博客的服务，jekyll是后台所运行的引擎。

来源：
<a href= "http://www.jianshu.com/p/05289a4bc8b2" target="_blank">如何搭建一个独立博客</a>
，<a href= "http://www.pchou.info/ssgithubPage/2013-01-03-build-github-blog-page-01.html" target="_blank">一步步在GitHub上创建博客主页</a>
，<a href = "http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html" target="_blank">通过GitHub Pages建立个人站点（详细步骤）</a>

## 前端笔记和总结

## jQuery menu菜单部件
http://www.runoob.com/jqueryui/example-menu.html



## 动画

animate.sass

http://opensource.org/licenses/MIT

## SASS语法
http://www.w3cplus.com/sassguide/syntax.html

## CSS开发样式
CSS

初始：

	*{box-sizing:border-box;}


- .btn 统一站内button样式
- border-top-right-radius:3px;


		<a href="#CSS">跳到CSS</a>
		<h2 id = "CSS">CSS</h2> 


### CSS3 rem（ie8+）

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

