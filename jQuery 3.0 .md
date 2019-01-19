# jQuery 3.0 Beta Released

原本打算翻译一下官方的说明文档，发现已经有同学做了，好吧，我们来说说变化吧。

2006年，为局部更新而生的jQuery诞生了，作为一个工具类的技术，十年屹立不倒，难能可贵呀。而歪果仁也懂双喜临门，在这十周年之际，jQ 3.0 beta版发不了。

### make the change

- jQ 3将不再支持IE8了，如果项目要继续支持IE678，请继续使用1.12分支上的最新版，新版本与你们无缘了，别傻等兼容版了，没有的，毕竟微软今年1月12日宣布放弃IE8910。

- show() 和 hide()，老样子，改善了隐藏大量元素时的性能。（jQuery官方团队原本的想法是想通过删除和添加行内样式‘display:none’来实现显示和隐藏，呵呵哒，想简单了，毕竟还有其他的css可能将它置为隐藏）。

- data() 的注意事项，为了兼容 HTML5 dataset 规范，jQuery 团队升级了 .data() 实现，用法如：“foo-bar”=>“fooBar”。

- .width()，.height()，.css('width')，.css('height')都将返回小数。之前，jQuery 取宽高的时候会返回四舍五入之后的整数值。

- 移除废弃的事件别名，jQ 1.8之后弃用的 .load，.unload，.error 方法被正式移除。以后都请使用 .on() 注册事件侦听器。

- 使用 requestAnimationFrame 改善动画效果，IE10以下及低于4.4的Android都不兼容。

- unwrap( selector )，3.0之前，.unwrap() 方法不接受任何参数。如今，用户可以通过传入选择器来移除指定的外部容器。

- jQuery.fn.domManip 不能再使用了，它未来都只允许内部使用，不会被载入使用文档。

- jQ自定义选择器提速，提高性能。

- 更加合理的出错提示，错误不再被悄无声息地隐藏。

- Deferreds 对象增加 .catch() 方法。Promise 对象增加 .catch()方法， 作为 .then(null, fn) 的别名，专门处理失败。

- 移除 jQuery.ajax 的 Deferred 同名方法的特殊用法。

- jQuery.Deferred 现在兼容 Promises/A+。


### 最后

是否应该继续支持IE910在官方博客的评论区引起了大家的激烈讨论，哈哈，不要太搞笑。

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-30/10301045.jpg" width="500px">

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-30/12188918.jpg" width="500px">



