链接：http://segmentfault.com/a/1190000003060827

原文：https://github.com/h5bp/Front-end-Developer-Interview-Questions 


###常用问题
- 你昨天/这周都有学什么？
- 是什么激发让你对写代码感兴趣(你喜欢写代码的动力是什么？)？
- 列举一项你最近(在项目)中碰到的挑战，你是如何解决的？
- 当你在建一个web应用程序或者网站时候，你会考虑哪些方面(UI，安全，性能，SEO，可维护性还是技术等)
说说你所喜欢的开发环境(OS，IDE...)？
- 如果你有五个不同的样式表，你怎么样最好地整合应用到一个页面上去呢？
- 你能描述下渐进增强和优雅降级的区别么？
- 怎么对一个站点（页面）资源进行优化处理？
- 浏览器从同一个站点一次能下载多少资源？（例外情况） （不清楚TODO）
- 说出三条方法去减少页面加载时间(感知到的或者真是的加载时间)
- 如果你在一个项目中别人都使用tabs而你使用space，你会怎么做？
- 描述下你怎么创建一个简单的幻灯片播放页面？
- 如果你今年能学习掌握一门技术(语言)，你觉得会是什么呢？
- 解释标准和标准机构的重要性？
- 什么是文档样式闪烁(Flash of Unstyled Content)？如何避免
- 解释ARIA和screenreaders，怎么让一个网站可理解？
- 说下相比于JavaScript动画实现，CSS的动画实现有什么优点和缺点？
- CORS是什么？它解决什么问题？

###HTML问题
- doctype的作用？
- 标准模式和怪异模式的差异？
- HTML和XHTML的差异？
- 像application/xhtml+xml这样的服务页面会有什么问题么？
- 你怎么实现一个页面的多种语言(切换)?
- 哪些方面的东西是你在设计和开发多语言网站时必须要小心谨慎考虑到的？
- 哪些data-的属性是有效的？
- HTML5作为一个开放的Web平台，HTML5的构建块是什么？
- 描述下cookie sessionStorage localStorage的区别？
- 描述下\<script> \<script async> \<script defer>的区别？
- 为什么一般总是把CSS link放置于<head></head>，而把script放在</body>前？有什么例外么
- 什么是渐进渲染？
- 你之前使用过什么不同的HTML模板语言么？

###CSS问题
- CSS中class和ID的区别？
- CSS中resetting和normalizing的区别？你会选哪个，为什么？
- 描述下float和它的作用原理
- 描述下z-index以及层叠上下文是怎么形成的？
- 描述下BFC(Block Formatting Context)和作用原理？
- 有哪些清除技术(?)，它们分别适用于什么上下文环境？
- 说下CSS Sprites（CSS压缩）,怎么在一个页面或者站点上实现？
- 你最喜欢的图片替换技术，你是在什么时候用的？
- 你怎么着手处理特定的浏览器样式问题（浏览器兼容）？
- 你是怎么样让你你的页面在一些不支持一些特性的浏览器中跑起来的？用什么样的技术/处理程序去实现？
- 有哪些方法在视觉上隐藏内容元素（以及让其仅对screener reader有效？）
- 你用过栅格系统么，如果用过，喜欢哪一种呢？
- 你用过/实践过media queries或者mobile specific布局/CSS?
- 熟悉SVG么？
- 怎么优化web页面的打印？
- 一些书写高效CSS的"gotchas"是什么？
- CSS预处理器的优点与缺点？说下你用过的喜欢的和不喜欢的预处理器。
- 你怎么实现非标准字体的网页设计排版？
- 解释下浏览器是怎么确定元素和CSS选择器匹配起来的？
- 说一下伪元素与其对应的作用功能？
- 说一下你对盒模型的理解以及你怎么让浏览器通过CSS在不同的盒模型下渲染你的布局？
- box-sizing:border-box是什么？它的好处是什么？
- 列举下你记得的display属性？
- inline和inline-block的区别？
- relative fixed absolute以及static来定位元素的区别？
- CSS中的C是层叠的意思，对于被指定样式的优先级是怎么确定的呢？（举一些例子）？你是怎么样很好地利用这个机制？
- 在本地环境或者生产环境中，你用过哪些CSS框架？你是怎么改进和优化的呢？
- 你玩过新的CSS Flexbox或是Grid specs?
- 响应式设计(responsive design)和自适应设计(adaptive design)的区别？
- 你做过retina graphic（视网膜图像）的处理么？是的话，你是用什么技术的呢？
- 你在什么情况下使用translate()代替absolute positioning绝对定位，反之，为什么？

###JS问题
- 解释下事件委托？
- 解释下this在JavaScript中怎么工作的（机制）？
- 解释下prototypal inheritance原型机城的工作原理？
- 对于AMD和CommonJS的比较和看法？
- 解释下以下代码为什么不能作为IIFE(匿名函数自调用)运行

		function foo{}();
		
	你需要怎么改使之成为一个IIFE？
		
- null undefined undeclared这些变量有什么区别？怎么这些检查变量类型？
- 什么是闭包？你是怎么使用的，为什么使用?
- 匿名函数典型的应用案例？
- 你是怎么组织你的代码的？（模块化还是类型继承）
- 宿主对象(host object)和本地对象(native object)的区别
- 以下代码有什么区别？

		funciton Person(){};
		var person = Person();
		var person = new Person();
- call和.apply区别？
- 解释下Function.prototype.bind?
- 什么时候使用document.write()?
- feature detection feature inference以及UA String(用户代理)的区别？
- 尽你所知地解释下AJAX？
- 解释下JSONP的工作原理（为什么它不算是AJAX）
- 用过JavaScript Template（模板）么？如果用过，哪个库？
- 解释下hosting?
- 描述下事件冒泡(event bubbling)?
- attribute和property的区别？
- 为什么说扩展内置JavaScript对象(built-in Javascript Object)不是个好主意？
- load和document ready事件区别？
- ==和===的区别？
- 解释下关于JavaScript的同源策略(same-origin policy)?
- 实现如下代码？
	
		depulicate([1,2,3,4,5]); //[1,2,3,4,5,1,2,3,4,5]
		
- 为什么称之为Ternary expression(三元表达式/三目运算符？)，Ternary指什么？
- 什么是use strict(使用严格模式)？它的优点和缺点？
- for()循环100次，3的倍数输出fizz，5的倍数输出buzz，3和5的倍数输出fizzbuzz?为什么一般来说好的做法是使全局作用域保持原样而不去改变之。
- 为什么你会使用类似于load(功能)的事件？它会有什么缺点么？你知道什么其他替代方法么，为什么你用那些替代方法？
- 解释下什么是单页面应用(a single page app)以及怎么进行SEO优化（make one SEO-friendly）?
- 使用Promises以及/或者他们的polyfills的经验程度？（新技术么，完全没了解）
- 使用Promises代替callbacks的利弊(优缺点)？
- 编写一种(其他)语言编译成JavaScript的写法(writing JavaScript code in a language that compiles to JavaScript)有什么优点和缺点？
- 你用什么技术和工具调试JavaScript代码？
- 你使用哪种语言构造器(language constructions)来重复遍历对象属性和数组对象？
- 解释下mutable和immutable对象的区别
- 举一个JavaScript中immutable对象的例子？immutability的利弊？怎么用代码实现immutability?
- 解释下同步方法(synchronous function)和异步方法(asynchronous function)的区别？
- 什么是event loop?

		`call stack`和`take queue`的区别？
		
###测试问题

- 测试代码的利弊是什么？
- 你用什么工具来进行代码功能测试？
- 单元测试和功能/集成测试的区别？
- 代码风格检查工具(a coding style linting tool)的目的/作用？

###性能问题
- 你用什么工具去发现检查代码中的性能缺陷？
- 有哪些方法去优化网页的滚动性能（website's scrolling performance）?
- 解释下layout painting以及compositing的区别？

###网络问题

###代码问题
	// Q1: foo的值
	var foo = 10 + '20';
	
	// Q2: 如何实现如下函数？
	add(2,5);//7
	add(2)(5);// 7
	
	// Q3: 下面字符串返回值？
	"i'm a lasagna hog".split("").reverse().join("");
	
	// Q4: window.foo值？
	(window.foo || (window.foo = "bar"));
	
	// Q5: 下面两个alert()结果
	var foo = "Hello";
	(function() {
	var bar = " World";
	alert(foo + bar);
	})();
	alert(foo + bar);
	
	// Q6: foo.length值
	var foo = [];
	foo.push(1);
	foo.push(2);
	
	// Q7: foo.x值
	var foo = {n: 1};
	var bar = foo;
	foo.x = foo = {n: 2};
	
	// Q8:下列代码输出
	console.log('one');
	setTimeout(function() {
	console.log('two');
	}, 0);
	console.log('three');
	

