---
layout: post
title: "JavaScript函数中的参数类型"
date: 2017-03-16
excerpt: "弱类型语言的函数参数类型验证。"
tag:
- javascript

comments: false
---


## 一、函数参数类型验证

JavaScript是一种弱类型语言，由于**声明形参无须定义数据类型**，所以导致调用这些函数时可能出现问题。：如果弱类型语言的函数需要接受参数，则应先判断参数类型，并判断参数是否包含了需要的参数。

 		<script>
        window.onload = function(){

	        var twoSum = function(nums, target) {
	            if(isaNumArr(nums) && !isNaN(parseInt(target))){//判断参数类型
	                    var nums = nums,
	                    target = target;
	                    var len = nums.length,
	                    indexArray = [];
	                    if(len>0){
	                    for(var i = 0;i<len;i++){
	                    for(var j = len-1;j>0;j--){
	                        console.log(nums[i] +"+" + nums[j] +"=" + (nums[i]+nums[j]));
	                            if(nums[i]+nums[j] == target && i != j){
	                            indexArray.push([nums[i],nums[j]]);
	                        }
	                     }
	                    }
	                    return indexArray;
	             }
	             }else{
	                console.log("TypeError：nums:array array[num],target:number");
	                return false;
	            }
	         }
	
			//判断是否为数值型数组
	        function isaNumArr(arr){
	            var arr = arr;
	            if(arr instanceof Array){
	                return arr.every(function(item){
	                  return (typeof item == "number")
	            })
	            }
	        }
	
	
			//测试用例
	        var nums = [1,2,3,4,5,6],
	        target = 7;
	        var indexArray = twoSum(nums,target);
	        console.log(indexArray);
	
	
	        var nums = [1,6,2,3,4,5],
	        target = 7;
	        var indexArray = twoSum(nums,target);
	        console.log(indexArray);
	
	
	        var nums = ["chenyueli","1","6"],
	        target = 7;
	        var indexArray = twoSum(nums,target);
	        console.log(indexArray); //false        
	        
	
	        var nums = [1,6,2,3,4,5],
	        target = "7";
	        var indexArray = twoSum(nums,target);
	        console.log(indexArray); //false        
        }
        </script>

总结：如何检测参数数据类型是否符合？

1. `typeof` 可用于判断基本类型(string,number,boolean,undefined)，复杂数据类型object(包括null)和function
2. `instanceof` 可用于判断数组，继承

## 二、Javascript的数据类型笔记
1.数值转换（将非数值转换为数值）： 

- Number(任意类型),
- parseInt，parseFloat(字符串转数值)  


2.`NaN`与任意值不相等

- NaN == NaN ->false  
- isNaN(NaN)->true

`parseInt()` 函数可解析一个字符串，并返回一个整数(或NaN)。

		parseInt(string, radix)
radix:	可选。表示要解析的数字的基数，默认十进制。


	isNaN(parseInt(""))  ->true
	parseInt("12ab")	->12


3.javascript中的隐式转换
 <pre>
基本类型的转换
1.字符串加数字,数字就会转成字符串。
2.数字减字符串，字符串转成数字。如果字符串不是纯数字就会转成NaN。
3.乘，除，大于，小于跟减的转换也是一样。
== 
1.undefined等于null
2.字符串和数字比较时，字符串转数字
3.数字为布尔比较时，布尔转数字
4.字符串和布尔比较时，两者转数字

	// ==
	undefined == null;    //true
	'0' == 0;        　　  //true,字符串转数字
	0 == false;           //true,布尔转数字
	'0' == false;    　　　//true,两者转数字
	null == false;     　 //false
	undefined == false; 　//false

引用类型的转换：引用类型和基本类型的比较就相对复杂一些，先要把引用类型转成基本类型，再按上述的方法比较。

1.引用类型转布尔全是true。比如空数组，只要是对象就是引用类型，所以[]为true。用if(引用类型)都为true.
2.引用类型转数字或者字符串就要用valueOf()或者toString();1.对象转数字时，调用valueOf();2.对象转字符串时，调用toString();

	0 == [];        // true, 0 == [].valueOf(); -> 0 == 0;
	'0' == [];      // false, '0' == [].toString(); -> '0' == '';
	2 == ['2'];     // true, 2 == ['2'].valueOf(); -> 2 == '2' -> 2 == 2;
	'2' == [2];     // true, '2' == [2].toString(); -> '2' =='2';
	[] == ![];      //true, [].valueOf() == !Boolean([]) -> 0 == false -> 0 == 0;



转为布尔值：if()
undefined,null,0,"" ->false
[],{},其他 ->true

用"=="
"0" == false
0 == false;
'0' == 0; 
undefined == null; 
</pre>
<a href = "http://www.2cto.com/kf/201502/377443.html">JavaScript的隐式转换</a>
### window.onload 加载
一般在文档加载完成后执行javaScript脚本，否则可能**无法获取DOM对象**，为了避免这种情况的发生，可以使用以下两种方式:

1.将脚本代码放在网页的底端，这样在运行脚本代码的时候，可以确保要操作的对象已经加载完成。（推荐）

2.通过`window.onload`来执行脚本代码。

	window.onload = function(){}

具体参考：[window.onload用法详解](http://www.softwhy.com/forum.php?mod=viewthread&tid=6191)

来源：
[JavaScript函数中的参数类型](http://blog.csdn.net/u012868077/article/details/51588145)
