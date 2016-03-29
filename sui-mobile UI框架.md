##SUI Mobile UI库


随着移动web开发的越来越火爆，移动端的UI库也越来越多，我所知道的就有 GMU、Amaze UI、Frozen UI、WeUI、SUI Mobile等。

### 比较

###### Frozen UI
	
	自述：简单易用，轻量快捷，为移动端服务的前端框架。
	
	主页：http://frozenui.github.io/
	
	开发团队：QQVIP FD Team
	
	Github：https://github.com/frozenui/frozenui
	
	Demo：http://frozenui.github.io/frozenui/demo/index.html

	
###### WeUI

	自述：WeUI是一套同微信原生视觉体验一致的基础样式库，为微信 Web 开发量身设计，可以令用户的使用感知更加统一。
	
	开发团队：Wechat Official Design Team
	
	Github：https://github.com/weui/weui
	
	Demo：http://weui.github.io/weui/
	

###### SUI Mobile

	主页：http://m.sui.taobao.org/
	
	自述：轻量、小巧、精美的UI库，方便迅速搭建手机H5应用，也非常适合开发跨平台Web App。
	
	开发团队：阿里巴巴共享业务事业部UED团队
	
	Github：https://github.com/sdc-alibaba/SUI-Mobile
	
	Demo：http://m.sui.taobao.org/demos/
	
	兼容：兼容到 iOS 6+ 以及 Android 4.0+
	
	基于 Framework7 进行开发，并参考 Ratchet、Fastclick 开源库。

###### GMU

	主页：http://gmu.baidu.com/
	
	自述：GMU是基于zepto的mobile UI组件库，提供webapp、pad端简单易用的UI组件。
	
	开发团队：百度前端
	
	Github：https://github.com/gmuteam/GMU
	
	Demo：http://gmu.baidu.com/demo/

	兼容：支持iOS4+， android2.1+
	
	最后更新：2013.10.24，长时间未维护
	
###### Amaze UI

	主页：http://amazeui.org/
	
	自述：中国首个开源 HTML5 跨屏前端框架。
	
	开发团队：云适配
	
	Github：https://github.com/amazeui/amazeui
	
	Demo：http://amazeui.org/getting-started
	
	
其中GMU库由于在实际项目中有多次使用到了，所以会比较的熟悉，该库由于长时间没有维护了，已经越来越不适应现在的项目的了，比如该库是在较低版本的zepto（也停止维护了）上搭建的，引用的是IScroll V4版本等，而且组件库已经很难应付设计师新的交互。

WeUI迅速火起来是在2016微信公开课上，为帮助网页开发者实现与微信客户端一致的视觉体验，并降低设计和开发成本，我理解是如果做微信web项目且团队中没有设计师，WeUI是个不错的选择，至少能让你的web风格看起来和微信是统一。WeUI官方仅仅提供了基础组件，但另外有第三方的扩展，如：jQuery-weui、react-weui、vue-weui等，很好的扩充了使用的场景，可以很方便的在不同环境下使用。
	
最近有项目要做移动端，为了让移动web具有的native的感觉，考察决定使用sui-mobile UI库，下面详细介绍一下该UI库。

### SUI Mobile 
SUI Mobile（以下简称SUI） 是一套基于 Framework7 开发的UI库。它非常轻量、精美，只需要引入我们的CDN文件就可以使用，并且能兼容到 iOS 6.0+ 和 Android 4.0+，非常适合开发跨平台Web App。（注：Framework7是一个开源免费的框架可以用来开发混合移动应用（原生和HTML混合）或者开发 iOS & Android 风格的WEB APP）

###### 为什么是SUI呢？

SUI的路由跳转可以不走浏览器原生跳转，可以实现更加类似于native的页面跳转效果，效果如图：

 <img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/22799471.jpg" width=200">
 
SUI提供强大的配置参数，让你能随心所欲的切换路由跳转方式。

> Router默认开启，会自动拦截所有链接的Touch行为，如果希望一个链接走浏览器原生跳转而不使用router，可以在链接上增加 class="external" 或者自定义属性,如 \<a href="xxx" external>xxx\</a>. 详细配置参考文档。


另外还有一个强大的功能是你可以在同一个HTML文件中内联编写多个页面。每一个页面都是一个 .page 容器，如果有多个页面，则需要用 .page-current 标记出第一次进入的时候应该显示的页面。

每一个 .page 容器都应该有一个全局唯一的id，路由器会使用这个id作为唯一标示，如果没有设置id，可能会导致路由出错。你只需要这样写一个链接就可以跳转到id对应的内联页面 \<a href='#{page-id}'>跳转\</a>.

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/97539936.jpg" width="500">

###### 怎么做到的？

ajax + history.pushState = [pjax](https://github.com/welefen/pjax)


路由功能将接管页面的链接点击行为，最后达到动画切换的效果，具体如下：

1. 链接对应的是另一个页面，那么则尝试 ajax 加载，然后把新页面里的符合约定的结构提取出来，然后做动画切换；如果没法 ajax 或结构不符合，那么则回退为普通的页面跳转
2. 链接是当前页面的锚点，并且该锚点对应的元素存在且符合路由约定，那么则把该元素做动画切入
3. 浏览器前进后退（history.forward/history.back）时，也使用动画效果
4. 如果链接有 back 这个 class，那么则忽略一切，直接调用 history.back() 来后退

###### 源码分析

- 事件监听，事件委托，当点击a标签时，阻止默认事件，含有back class时，做返回操作，获取参数是否忽视缓存

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/43449552.jpg" width="500">

- 切换到 url 指定的块或文档
- 如果 url 指向的是当前页面，那么认为是切换块；
- 否则是切换文档


<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/39101670.jpg" width="500">

- 载入显示一个新的文档


<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/15594124.jpg" width="500">

- ajax 加载 url 指定的页面内容

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/23097121.jpg" width="500">

利用缓存来做具体的切换文档操作

- 确定待切入的文档的默认展示 section
- 把新文档 append 到 view 中
- 动画切换文档
- 如果需要 pushState，那么把最新的状态 push 进去并把当前状态更新为该状态

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/70096985.jpg" width="500">

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-29/71298676.jpg" width="500">


###### 示例
大块代码不好展示，原理同上，简化版。


