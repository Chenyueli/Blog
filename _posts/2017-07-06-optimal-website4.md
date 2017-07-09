---
layout: post
title: "打造高性能前端页面（四） "
date: 2017-07-09
excerpt: "书写高效CSS "
tag:
- 性能

comments: false
---

## CSS命名

	外套 wrap ------------------用于最外层
	头部 header ----------------用于头部
	主要内容 main ------------用于主体内容（中部）
	左侧 main-left -------------左侧布局
	右侧 main-right -----------右侧布局
	导航条 nav -----------------网页菜单导航条
	内容 content ---------------用于网页中部主体
	底部 footer -----------------用于底部
http://www.divcss5.com/jiqiao/j4.shtml

## 单页应用隔离CSS
webpack打包多个CSS文件和一个CSS文件

 Extract Text Plugin


	Extract text from a bundle, or bundles, into a separate file.

Install

	# for webpack 2
	npm install --save-dev extract-text-webpack-plugin
	# for webpack 1
	npm install --save-dev extract-text-webpack-plugin@1.0.1

Usage

	const ExtractTextPlugin = require("extract-text-webpack-plugin");
	
	module.exports = {
	  module: {
	    rules: [
	      {
	        test: /\.css$/,
	        use: ExtractTextPlugin.extract({
	          fallback: "style-loader",
	          use: "css-loader"
	        })
	      }
	    ]
	  },
	  plugins: [
	    new ExtractTextPlugin("styles.css"),
	  ]
	}


> For webpack v1, see the README in the webpack-1 branch.[webpack-1 branch.md ](https://github.com/webpack-contrib/extract-text-webpack-plugin/blob/webpack-1/README.md)

It moves all the required *.css modules in entry chunks into a separate CSS file. So your styles are no longer inlined into the JS bundle, but in a separate CSS file (styles.css). If your total stylesheet volume is big, it will be faster because the CSS bundle is loaded in parallel to the JS bundle.


[https://github.com/webpack-contrib/extract-text-webpack-plugin](https://github.com/webpack-contrib/extract-text-webpack-plugin)

