# JavaScript 高级程序设计（第3版）读书笔记

## 第1章 JavaScript 简介
	

完整的JS实现包含：

* 核心（ECMAScript）ECMA-262
* 文档对象模型（DOM）
* 浏览器对象模型（BOM）

第1版、第3版

第5版 实际上是 2009年12月3日发布的 ECMAScript 3.1，4.0变更太大被废弃。

## 第2章 在 HTML 中使用 JavaScript

* `<script>` 元素
  * async 可选，异步下载脚本，并不保证执行顺序
  * defer 可选，延迟执行,HTML5的实现会忽略（IE8之后完全支持H5的方式）
  * type 可选，默认`text/javascript`，可以省略
  * 不要在代码任何地方出现`alert("</script>")`,会被认为代码段结束，使用转义符解决`alert("<\/script>")`
  * 嵌入代码与外部文件：外部文件的优点：可维护性、可缓存、适应未来
* 文档模式,html5:`<!doctype html>` 

## 第3章 基本概念

* `'use strict';`
* `;`建议任何时候不要省略它，可以避免很多错误，可以放心的压缩代码，也能增进代码性能
* 最佳实践是始终在控制语句使用代码块`if(test){...}`
* 关键字保留字（略）
* 可以变更变量所保存的类型，但不建议这么做（性能降低）
* 5中基本类型：Undefined、Null、Boolean、Number、String，1种复杂类型：Object
* typeof:'undefined','boolean','string','number','object','function'
* 即便未初始化变量会被赋 undefined 值，但显式的初始化变量依然是明智的选择
* null 是空对象，`typeof null` 返回 `'object'`
* Number 
	* 支持八进制 `var num = 070 //56`，十六进制 `var num = 0xA //10`，科学计数法 `3.1225e7` 
	* 浮点数精度问题 `0.1 + 0.2`结果不是`0.3`,而是`0.300000000000000004`
	* Number.MIN_VALUE, Nubmer.MAX_VALUE, Infinity, isFinite()
	* NaN (Not a Number),任何数除以 0 返回 NaN
	* `isNaN()` 任何不能转换成数值的都返回 `true`,如果是对象，先调用 valueOf(),如果还不能转成数值则再调用toString()
	* 数值转换，Number(),parseInt(), parseFloat()，Es3中 `070` 被当做八进制，Es5不会，为避免错误，始终给parseInt()传递第二个参数；
* String 
	* 转义符，`.length`获得字符长度，一旦创建就不能修改，修改其实是销毁重建
	* 转换字符串，除了`null`和	`undefined`几乎每个值都有 `.toString()`方法,数值可以输出成二进制、八进制和十六进制 toString( [ 2 | 8 | 10 | 16 ] )
* Object

## 第4章 变量、作用域和内存问题
### 变量和作用域
* js变量与其他语言变量有很大区别，它是松散类型的，是特定时间保存特定值的一个名字而已
* 变量包含两种类型：基本类型、应用类型
	* 基本类型(5)：Undefined, Null, Boolean, Number, String
	* 引用类型：变量存储着指向保存在内存中的对象的指针
* 动态属性：应用类型的变量可以动态添加属性
* 复制：基本类型在内存中建立副本；引用类只是复制指针，指针指向内存中的同一个对象
* 参数：如同复制，将外部变量复制给函数内部变量
* 监测：`typeof`检查出字符串、数值、布尔值、undefined、object,`instanceof`根据原型链识别
	* typeof 监测正则表达式，Safari5 Chrome7之前版本返回“function”,IE,Firefox返回“object”
* 执行环境：（execution context），每个执行环境都有关联的变量对象（variable object），所有的变量和函数都保存在这个对象中，无法访问但确实存在。每个函数都有自己的执行环境，
* 如果是函数，则将其活动对象（activation object）作为变量对象（vo），最初只有 arguments 一个对象(在全局环境中不存在)，
* 作用域链（scope chain）的下一个变量对象来自于包含它的环境，层层嵌套，一直到全局执行环境。每次进入新的执行环境都会创建一个用于搜索变量和函数的作用域链
* 标示符解析式沿着作用域链一级一级搜索的过程，直到找到为止。找不到通常会导致错误
* 延长作用域链：try-catch语句的catch块，with语句
* 如果初始化变量没有用 `var`，该变量会被自动添加到全局环境，但在严格模式下会导致错误。建议始终使用 var
### 垃圾收集
* 两种方式：标记清除（mark-and-sweep）、引用计数（reference counting）
* 在循环引用的情况下，引用计数的方式会造成内存无法回收，需要手动把引用方设置成null手工断开引用，IE9以上改进BOM和DOM，避免了此问题
* 现代浏览器都已使用标记清除法
* 内存管理：解除引用（dereferencing）：一旦数据不再用，最好设置成null来释放引用，以便垃圾收集器在下一个周期执行时回收



## 第5章 引用类型
### Object
* 创建 `var p = new Object(); p.num = 3` or `var p = { num : 3 }`
* 通过对象字面量创建 **不会** 调用 Object构造函数；
* 对象字面量也是想函数传递大量可选参数的
* 可以用 typeof 检测属性是否存在, `if(typeof args.age == 'number'){...}`
* 访问属性有两种方式`person.name` or `person['name']`, 除非必要，否则建议用点表示法
### Array
* 每一项可以保存任何类型的数据，数组的大小是可以的动态调整的
* 用 `new Array()`创建，new 可以省略，`Array(3)`创建长度为3的数组，`Array('a','b','c')`
* 用字面量创建`var values = [1,2,3]`
* 获取值 `values[2] // 3`
* `length`属性获取长度，也可以通过直接设置length 改变数组长度。(截取或添加undefined)。利用这一点可以将length 设为 0 清空数组
* 检测数组 `if(value instanceof Array){...}`,此方法无法检查跨框架（iframe）的数组
* Es5中 使用`Array.isArray()`检测，支持的浏览器：IE9+，Firefox 4+, Safari 5+, Opera 10.5+ 和 Chrome
* 转换 toLocaleString(), toString(), valueOf(),转换过程会调用每一项的对应转换函数；另外也可以用 join()方法
* 栈 LIFO (Last-In-First-Out 后进先出),`push('a')`返回长度,`pop()` 返回移除的项
* 队列 FIFO (First-In-First-Out 先进先出) shift()+push() or unshift()+pop();
* 重排 reverse(), sort(),sort默认情况下将调用没一项的toString()方法，然后比较字符串。也可以通过传入的比较函数进行排序。如果只是想颠倒顺序，reverse()更快一些
* 比较函数：
	
		function compare(value1, value2) {
            if (value1 < value2) {
                return -1;
            } else if (value1 > value2) {
                return 1;
            } else {
                return 0;
            }
        }
        
        // 对于数值类型或其valueOf()返回数值类型的，两个值相减即可
        function compare(value1, value2) {
            return value2 - value1
        }
* 数组操作：
	* 合并 `concat()` 返回副本数组，并将参数添加到结果数组中
	* 切片 `slice()`, 返回2个参数之间的数组副本，第二个参数省略，返回从第一个参数到结尾的数组副本，如果为负数则反方向算位置
	* 剪辑 `splice()` 返回被移除的数组，直接改变原素组
		
			// 删除
			arr.splice(1,2)
			
			// 插入
			arr.splice(2,0,'a','b')
			
			// 替换
			arr.splice(2,1,'a')
* 查位置 `indexof()`,`lastIndexof()` ,使用严格相等比较，IE9+, FF2+, Safari 3+, Opera 9.5+, Chrome
* 迭代方法 Es5
	* 判断 `every()`,`some()`
	* 过滤 `filter()`
	* `map()`
	* `forEach()` （理论上同for循环遍历数组，但forEach()是异步的）
* 归并方法 Es5
	* `reduce()`,`reduceRight()`
	
### Date 类型
* UTC (Coordinated Universal Time, 国际协调时间) 1970年1月1日零时开始经过的毫秒数
* `new Date()` 返回日期对象
*  返回毫秒数 `Date.parse('2015-5-12T21:25:31')`,`Date.UTC(2015,4,12,21,25,31)` 注意第二个参数月份是从0开始计算
* Date 构造函数也支持上面两种参数传入，不同的是 日期和时间是基于本地时间
* Es5新增 `Date.now()`，在不支持now的浏览器可以用 `+new Date()` 等同于 `new Date().valueOf()` 
* `toLocaleString()`,`toString()`,`valueOf()`
* 格式化：`toDateString()`,`toLocalDateString()`,`toTimeString()`,`toLocaleTimeString()`,`toUTCString()`
* 取值、赋值方法 `getTime(),setTime()`...)

### RegExp 类型


## 第6章 面向对象的程序设计


## 第7章 函数表达式


## 第8章 BOM


## 第9章 客户端检测


## 第10章 DOM


## 第11章 DOM扩展


## 第12章 DOM2 和 DOM3


## 第13章 事件


## 第14章 表单脚本


## 第15章 使用 Canvas 绘图


## 第16章 HTML5 脚本编程


## 第17章 错误处理与调试


## 第18章 JavaScript 与 XML


## 第19章 E4X


## 第20章 JSON


## 第21章 Ajax 与 Comet


## 第22章 高级技巧


## 第23章 离线引用与客户端存储


## 第24章 最佳事件


## 第25章 新兴的 API