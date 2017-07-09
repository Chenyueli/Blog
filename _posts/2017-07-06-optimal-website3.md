---
layout: post
title: "打造高性能前端页面（三） "
date: 2017-07-09
excerpt: "webpack按需加载 node.js缓存；webP图片； 高效CSS书写 "
tag:
- 性能

comments: false
---
## webpack2.2 按需加载

Webpack 也可以实现懒加载入口文件，意味着应用的一部分只在需要的时候加载，一个典型的例子是用户只有访问一些应用特定的部分，典型的例子是 Twitter.com，你不会一直访问你的个人页，所以为什么要加载那部分的代码？这里有个主要的要求：

	require.ensure(dependencies: String[], callback: function(require), chunkName: String)

> **依赖 dependencies**

这是一个字符串数组，通过这个参数，在所有的回调函数的代码被执行前，我们可以将所有需要用到的模块进行声明。

> **回调 callback**

当所有的依赖都加载完成后，webpack会执行这个回调函数。require 对象的一个实现会作为一个参数传递给这个回调函数。因此，我们可以进一步 require() 依赖和其它模块提供下一步的执行。

> **chunk名称 chunkName**

chunkName 是提供给这个特定的 require.ensure() 的 chunk 的名称。通过提供 require.ensure() 不同执行点相同的名称，我们可以保证所有的依赖都会一起放进相同的 文件束(bundle)。

	require.ensure([], function(require){
		    require('b');
		});

注明：require.ensure加载模块，并不执行，如需执行，在内部函数中加上default即可

	require('b').default

> CommonJS: The require.ensure method ensures that every dependency in dependencies can be synchronously required when calling the callback. An implementation of the require function is sent as a parameter to the callback.

	require.ensure(["module-a", "module-b"], function() {
	  var a = require("module-a");
	  // ...
	});
> Note: require.ensure only loads the modules, it doesn’t evaluate them.

[webpack2.2 中文文档](http://www.css88.com/doc/webpack2/guides/code-splitting-require/)

[webpack 原文](https://webpack.github.io/docs/code-splitting.html#commonjs-require-ensure)

## webpack+react-router实现动态加载

<img src = "{{ site.url }}/assets/img/post/requireEnsure.png" width="300" alt="webpack+react-router" >