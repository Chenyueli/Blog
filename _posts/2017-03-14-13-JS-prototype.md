---
layout: post
title: "深入理解js继承"
date: 2017-03-19
excerpt: "js作用域、原型链、构造函数、原型式继承、寄生式继承、寄生组合式继承"
tag:
- javascript

comments: false
---


## js继承

### 1.原型链继承

JavaScript主要通过**原型链**实现继承。原型链的构建是通过将一个类型的实例赋值给另一个构造函数的原型实现的。

	Sub.prototype = new Supper();

这样，子类型就能访问超类型的所有属性和方法。原型链的问题是对象实例共享所有继承方法和属性，不适合单独使用。


### 2. 构造函数，
即在子类型中调用超类型的构造函数。这样就可以做到每个实例都具有自己的属性，同时还能保证只使用构造函数模式来定义类型。

		function Supper(name){
			this.colors = ["red","blue","green"];
			this.name = name;
		}
		function Sub(name){
			Supper.call(this,name); 
		}
		var ins1 = new Sub("chenyueli"),
			ins2 = new Sub("pbb");
		ins1.colors.push("black");
		alert(ins1.colors + ins1.name);  //"red","blue","green","black" 只改变实例数组值
		alert(ins2.colors +ins2.name);	//"red","blue","green"  原型未改变
 
优点：子类继承父类**实例属性和方法**，可传参、可实现多继承（call多个父类引用属性)

缺点：

- 实例并不是父类的实例，只是子类的实例；
- 只能继承父类的实例属性和方法，**不能继承原型属性和方法**；
- 无法实现函数复用，**每个子类都有父类实例函数的副本**，影响性能。
### 3. 组合继承
使用最多的继承模式是**组合继承**，这种模式使用原型链继承**共享的属性和方法**，而通过借用构造函数继承 **实例属性**。

<script>
(function testCyl(){
alert("10");
})();
</script>


<script>
(function testCyl(){
function Supper(name){
this.name = name;
this.colors = ["red","blue","green"]
}
Supper.prototype.sayName = function(){
alert(this.name);
}
function Sub(name,age){
//继承属性
Supper.call(this,name)
this.age = age;
}
//继承方法
Sub.prototype = new Supper();
Sub.prototype.constructor = Sub;
Sub.prototype.sayAge = function(){
alert(this.age);
}
			
var ins1 = new Sub("cyl",18);
ins1.colors.push("black")
alert(ins1.colors);
ins1.sayName();
ins1.sayAge();
			
var ins2 = new Sub("cll",17);
alert(ins2.colors);
ins2.sayName();
ins2.sayAge();
})();
window.testCyl = testCyl;
</script>

<button onclick="testCyl()">运行</button> 

来源：<a href = "http://blog.sina.com.cn/s/blog_694c144f0101o4ol.html" target = "_blank">javascript 原型链的理解</a>，<a href = "http://www.cnblogs.com/humin/p/4556820.html" target = "_blank">JS实现继承的几种方法</a>

### 2. 构造函数、原型和实例的关系

#### js的`prototype`和`__proto__`属性

`prototype`是**函数**的内置属性

`__proto__`是**对象**的内置属性，是js内部**使用寻找原型链**的属性。IE不可以访问到对象的`__proto__`属性。

两者的关系：

	对象.__proto__ == 构造函数.prototype
	构造函数.__proto__ == Function.prototype是一个空函数

	var Person = function(){};
	var p = new Person();
	alert(p.__proto__ === Person.prototype);//true
	alert(Person.__proto__) //空函数function(){}


	new的过程拆分成以下三步：
	(1) var p={}; 也就是说，初始化一个对象p
	(2) p.__proto__ = Person.prototype;
	(3) Person.call(p); 也就是说构造p，也可以称之为初始化p


来源：<a href = "http://www.cnblogs.com/yangjinjin/archive/2013/02/01/2889103.html" target = "_blank">JS的prototype和__proto__</a>

### 3. apply(),call()的巧妙用法
定义：调用一个对象的一个方法，以另一个对象替换当前对象。 

apply的一个巧妙的用处,可以将一个数组默认的转换为一个参数列表

- 函数引用类型参数传递

一般在目标函数只需要n个参数列表,而不接收一个数组的形式,可以通过apply的方式巧妙地解决这个问题!

	var max=Math.max.apply(null,[1,23,3]) //23
	var max=Math.max.apply(Math,[1,23,3]) //23


来源：
<a href = "http://www.51xuediannao.com/qd63/index.php/page-2-104-1.html" target = "_blank">js apply和js call方法详解</a>，<a href = "http://uule.iteye.com/blog/1158829" target = "_blank">JS中的call()和apply()方法</a>


## js的解析与执行过程
### 1. js的作用域

- 函数作用域：函数内部变量只在内部作用。js满足
- 静态作用域：闭包/词法作用域，**声明时确定**的作用域。js满足
- 块作用域：{}大括号包含的。js不满足
- 动态作用域：运行时可访问内部变量,运行时确定。js不满足


javascript的作用域为词法作用域（也称静态作用域）。在词法解析阶段，就已经确定了相关的作用域。作用域还会形成一个相关的链条，我们称之为作用域链。
### 2. 全局函数预处理和执行

预处理：创建一个词法环境（LexicalEnvironment，简写为LE），扫描JS中的**用声明的方式声明的函数**(（不是函数表达式）)，**用var定义的变量**并将它们加到预处理阶段的词法环境中去。**处理函数声明有冲突时，会覆盖；处理变量声明有冲突时，会忽略。**

### 3. 函数中的解析和执行过程

函数中的解析和执行过程的区别不是很大，但是函数中有个`arguments`保存实参数量。

### 4. 使用匿名函数保护全局变量

问题：如果有多个函数都想要一个变量，

-  方法1：将变量设置为全局变量，增加了全局用量，不推荐。
-  方法2：使用匿名函数的方法，推荐。

**方法2**：

	(function(){
	    var a = 1,
	        b = 2;
	    function f(){
	        alert(a);
	    }
	    window.f = f;
	})();
	f();
	//结果为：1

来源：<a href = "http://www.cnblogs.com/foodoir/p/5977950.html" target = "_blank">js的解析与执行过程和匿名函数保护全局变量</a>，
<a href = "http://www.maiziedu.com/course/583/" target = "_blank">
JavaScript面向对象编程(麦子学院)</a>



## 事件作用域问题：

事件内定义变量，在事件结束销毁（再次进入时间时，改变）

事件外变量，累加

如果传入的是对象，则元素对象累加，将导致多个对象同时变化



1. 如何给事件传参；
2. 事件如何返回值；

### 事件的内存和性能：

js中添加到页面的时间处理程序数量将直接关系到页面的整体运行性能，原因：

1. 每个函数都是对象，都会占用内存，内存对象越多，性能越差；
2. 必须事先指定所有事件处理程序而导致：`DOM访问次数`，会延迟整个页面交互就绪时间。


解决方法：

- 方法1 ：事件委托。限制建立连接数量，占用更少内存，提高性能。
- 方法2：手动移除时间处理程序，减少链接数量，确保内存可以再次被利用。

具体实现：

- 用innerHTML删除元素前将元素解绑
- 纯Dom操作移除  removeChild()等已将事件解绑，无需再解绑

