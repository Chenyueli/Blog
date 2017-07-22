---
layout: post
title: "使用Imutable.js提高React性能 "
date: 2017-07-09
excerpt: "Virtual DOM; Immutable; "
tag:
- 性能

comments: false
---
## 一、Imutable.js克服react性能瓶颈

### 1. React Visual Dom
React的核心技术之一Virtual DOM

Virtual DOM包含：

1. Javascript DOM模型树（VTree），类似文档节点树（DOM）
2. DOM模型树转节点树方法（VTree -> DOM）
3. 两个DOM模型树的差异算法（diff(VTree, VTree) -> PatchObject）
4. 根据差异操作节点方法（patch(DOMNode, PatchObject) -> DOMNode）

<a href = 'http://www.cnblogs.com/justany/archive/2015/04/08/4401118.html'>VirtualDOM</a>
### 2. shouldComponentUpdate()

熟悉 React 的都知道，React 做性能优化时有一个避免重复渲染的大招，就是使用 shouldComponentUpdate()，它默认返回 true，执行 render() 方法，然后做 Virtual DOM 比较，并得出是否需要做真实 DOM 更新，这里往往会带来很多无必要的渲染并成为性能瓶颈。人为设置返回`false`，阻止组件render,可大大提高性能。 对<b style='color:red'>基本类型的state和props</b>，有

	shouleComponentUpdate(nextProps, nextState) {
      return this.props.value !== nextProps.value;
	}

> 值得注意的是，react会非常频繁的调用该函数，所以如果你打算自己实现该函数的逻辑，请尽可能保证性能。

但是如果你的组件所拥有的属性或状态不是基础类型呢，而是<b style='color:red'>复合类型</b>呢？比方说是一个js对象，{foo: 'bar'}：

	// 假设 this.props.value 是 { foo: 'bar' }
	// 假设 nextProps.value 是 { foo: 'bar' },
	// 但是nextProps和this.props对应的引用不相同
	this.props.value !== nextProps.value; // true

要想修复这个问题，简单粗暴的方法是我们直接比对foo的值，如下：

	shouldComponentUpdate: function(nextProps, nextState) {
	      return this.props.value.foo !== nextProps.value.foo;
	}

当然我们也可以在 shouldComponentUpdate() 中使用使用 deepCopy 和 deepCompare 来避免无必要的 render()，但 deepCopy 和 deepCompare 一般都是非常耗性能的。

	npm i --save lodash

	import '_' from 'lodash';
	let data = _.cloneDeep(this.state.data);

> 因为JavaScript存储对象都是存地址的，所以浅复制会导致 obj 和 obj1指向同一块内存地址，大概的示意图如下。而深复制一般都是开辟一块新的内存地址，将原对象的各个属性逐个复制出去。


### 3. Immutable不可变数据

2.1 Persistent data structure
Immutable.js提供了7种不可变的数据类型: `List、Map Stack OrderedMap Set OrderedSet Record`。对Immutable对象的操作均会返回新的对象，例如:

	var obj = {count: 1};
	var map = Immutable.fromJS(obj);
	var map2 = map.set('count', 2);
	
	console.log(map.get('count')); // 1
	console.log(map2.get('count')); // 2

2.2 structural sharing

当我们对一个Immutable对象进行操作的时候，ImmutableJS基于哈希映射树(hash map tries)和vector map tries，只clone该节点以及它的祖先节点，其他保持不变，这样可以共享相同的部分，大大提高性能。

	var obj = {
	  count: 1,
	  list: [1, 2, 3, 4, 5]
	}
	var map1 = Immutable.fromJS(obj);
	var map2 = map1.set('count', 2);
	
	console.log(map1.list === map2.list); // true

### 4. 在React中的实践
React是一个UI = f(state)库，为了解决性能问题引入了virtual dom，virtual dom通过diff算法修改DOM，实现高效的DOM更新。

听起来很完美吧，但是有一个问题: 当执行setState时，即使state数据没发生改变，也会去做virtual dom的diff，因为在React的声明周期中，默认情况下shouldComponentUpdate总是返回true。那如何在shouldComponentUpdate进行state比较?

React的解决方法: 提供了一个PureRenderMixin, PureRenderMixin对shouldComponentUpdate方法进行了覆盖，但是PureRenderMixin里面是浅比较:

	var ReactComponentWithPureRenderMixin = {
	  shouldComponentUpdate: function(nextProps, nextState) {
	    return shallowCompare(this, nextProps, nextState);
	  },
	};
	
	function shallowCompare(instance, nextProps, nextState) {
	  return (
	    !shallowEqual(instance.props, nextProps) ||
	    !shallowEqual(instance.state, nextState)
	  );
	}

浅比较只能进行简单比较，如果数据结构复杂的话，依然会存在多余的diff过程，说明PureRenderMixin依然不是理想的解决方案。

Immutable来解决: 因为Immutable的结构不可变性&&结构共享性，能够快速进行数据的比较:

	shouldComponentUpdate: function(nextProps, nextState) {
	  return deepCompare(this, nextProps, nextState);
	},
	  
	function deepCompare(instance, nextProps, nextState) {
		return !Immutable.is(instance.props, nextProps) || 
			!Immutable.is(instance.state, nextState);
	}

Immutable 则提供了简洁高效的判断数据是否变化的方法，只需 === 和 is 比较就能知道是否需要执行 render()，而这个操作几乎 0 成本，所以可以极大提高性能。修改后的 shouldComponentUpdate 是这样的：

	import { is } from 'immutable';
	
	shouldComponentUpdate: (nextProps = {}, nextState = {}) => {
	  const thisProps = this.props || {}, thisState = this.state || {};
	
	  if (Object.keys(thisProps).length !== Object.keys(nextProps).length ||
	      Object.keys(thisState).length !== Object.keys(nextState).length) {
	    return true;
	  }
	
	  for (const key in nextProps) {
	    if (!is(thisProps[key], nextProps[key])) {
	      return true;
	    }
	  }
	
	  for (const key in nextState) {
	    if (thisState[key] !== nextState[key] || !is(thisState[key], nextState[key])) {
	      return true;
	    }
	  }
	  return false;
	}

使用 Immutable 后，如下图，当红色节点的 state 变化后，不会再渲染树中的所有节点，而是只渲染图中绿色的部分：

### 5. React中引入Immutable.js带来的问题

- 源文件过大: 源码总共有5k多行，压缩后有16kb
- 类型转换: 如果需要频繁地与服务器交互，那么Immutable对象就需要不断地与原生js进行转换，操作起来显得很繁琐
- 侵入性: 例如引用第三方组件的时候，就不得不进行类型转换；在使用react-redux时，connect的shouldComponentUpdate已经实现，此处无法发挥作用。

[为什么需要Immutable.js](http://zhenhua-lee.github.io/react/Immutable.html)

[React性能优化](https://segmentfault.com/a/1190000006254212)


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