## 前言

我接触rem之前是在先知道em的，刚在一个项目中使用了em作为单位，各种计算，为了方便记忆转化，需要提前列举出px转em的转化表😭，如图：

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-30/33810962.jpg" width="200px" />

rem是（font size of the root element），官方解释：

<img src="http://7jpp2v.com1.z0.glb.clouddn.com/QQ%E5%9B%BE%E7%89%8720160313181850.png" width="500px" />

意思就是根据网页的根元素来设置字体大小。和em（font size of the element）的区别是，em是根据其父元素的字体大小来设置，而rem是根据网页的跟元素（html）来设置字体大小的。

举个🍐：
	
	html{
	  font-size: 16px;
	}
	
	p{
	  font-size: 0.75rem; //字体大小将显示为12px，0.75*16=12px
	}
	
> 注：若不修改相关的字体配置，浏览器默认字体大小为16px，chrome浏览器最小字体大小为12px

让我们来看看rem的兼容性，如下图所示：

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-30/62395448.jpg" width="500px">

在移动端上的表现

- ios：6.1系统以上都支持
- android：2.1系统以上都支持

大部分主流浏览器都支持，可以安心的往下看了。


## 使用rem进行屏幕适配


#### 为什么要对屏幕进行适配呢？

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-30/63167257.jpg" width="200px"/>
<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-3-30/72210233.jpg" width="200px"/>

该项目并没有进行屏幕适配，尺寸字体大小都是使用的px做单位，我们可以看到在iPhone 6P上字体观感明显比iPhone5上的小很多，这是由于DPR的不同导致。

所以如果我们在小尺寸屏幕上设置的字体大小，到了大尺寸屏幕上就会不合适了，我们需要动态修改字体的大小，让大尺寸屏幕的字体大小也正常合理的显示。


#### rem适配

原理：通过动态修改根节点字体大小来实现屏幕适配


	media-query{
	
	}
	p{
	  font-size: 0.75rem;
	}


#### rem数值计算

如果利用rem来设置css的值，一般要通过一层计算才行，比如如果要设置一个长宽为100px的div，那么就需要计算出100px对应的rem值是 100 / 16 =6.25rem，这在我们写css中，其实算比较繁琐的一步操作了。

为了方便起见，可以将html的font-size设置成100px，这样在写单位时，直接将数值除以100在加上rem的单位就可以了。

注：需要引入sass环境（less是没有自定义函数的功能😭）

	@function px2rem($px){
	  $rem : 16px;
	  @return ($px/$rem) + rem;
	}
	
	p{
	  height: px2rem(100px);
	  width: px2rem(100px);
	}
	
	
#### rem基准值计算

rem基准值就是根节点设置的字体大小，动态调整的参考值。如上例中的$rem: 16px

我们写的页面需要适应各种大小屏幕的设备，但设计师只会出一份设计效果图，假如我们拿到的设计图是基于iPhone5设计的:

	rem = window.innerWidth  / 320 * 10

320px是iPhone5的屏幕大小，iPhone5屏幕基准设置为1rem = 10px；便于计算。


#### 动态设置html的font-size

现在关键问题来了，我们该如何通过不同的屏幕去动态设置html的font-size呢，这里一般分为两种办法：

1.利用css的media query来设置

	@media (min-device-width : 375px) and (max-device-width : 667px) and (-webkit-min-device-pixel-ratio : 2)
	{
      html{
        font-size: 37.5px;
      }
	}

2.利用javascript来动态设置 根据我们之前算出的基准值，我们可以利用js动态算出当前屏幕所适配的font-size

	document.getElementsByTagName('html')[0].style.fontSize = window.innerWidth / 320 * 10 + 'px';
	
> 由此可见我们可以通过设置不同的html基础值来达到在不同页面适配的目的，当使用js来设置时，需要绑定页面的resize事件来达到变化时更新html的font-size，以便用户横屏时也能适配屏幕。



#### 进阶篇

有一天我们的设计师跑过来找我，给了我两张图，一张是他的设计图，另一种是页面实现了后在他iPhone 6sp手机上的截图，然后他把图片放到最大，说：‘我设计上这根线是1个像素，而你的实现怎么就变成了3个像素呢？’，为什么呢？

我们都知道，一般我们拿到的设计图都是物理像素大小的，而我们要实现的尺寸大小是逻辑像素大小，比如：设计师出的iPhone5设计图尺寸会是 1136*960，而我们手机网页显示的逻辑像素大小为 568\*480，这样由于高清屏像素压缩比（DPR）决定的。

iPhone5、iPhone6这种高清屏的DPRS是2，iPhone6s plus这种高清屏的DPR是3。

这就是为什么我们的设计师懵逼了😄。

我们可以通过js的window.devicePixelRatio获取到当前设备的dpr，拿到了dpr之后，我们就可以在viewport meta头里，取消让浏览器自动缩放页面，而自己去设置viewport的content例如（这里之所以要设置viewport是因为我们要实现border1px的效果，假如我给border设置了1px，在scale的影响下，高清屏中就会显示成0.5px的效果）

	meta.setAttribute('content', 'initial-scale=' + 1/dpr + ', maximum-scale=' + 1/dpr + ', minimum-scale=' + 1/dpr + ', user-scalable=no');

设置完之后配合rem，修改

	@function px2rem($px){
	  $rem : 32px;
	  @return ($px/$rem) + rem;
	}

	将$rem设置为原来值得dpr倍。


这样就可以完全按照视觉稿上的尺寸来了。不用在/2了，这样做的好处是：

1.解决了图片高清问题

2.解决了border 1px问题（我们设置的1px，在iphone上，由于viewport的scale是0.5，所以就自然缩放成0.5px）

再举几个在线的🍐：

[网易新闻](http://3g.163.com/touch/news/subchannel/all?version=v_standard)

[聚划算](https://jhs.m.taobao.com/m/index.htm#!all)

#### rem也不是万能的

<img src="http://7xpwoh.com1.z0.glb.clouddn.com/16-1-11/75557705.jpg" width="400px" />

问题描述：在移动端rem和sprite图一起使用时，会出现定位不准确的情况，如上图所示，我一度认为这是一个无解的问题，没有找到什么好的解决方案。贴上一篇解决方案，感谢原作者。

[文章链接](https://github.com/banricho/webLog/issues/1)


#### 参考资料
[http://www.alloyteam.com/2016/03/mobile-web-adaptation-tool-rem/#prettyPhoto](http://www.alloyteam.com/2016/03/mobile-web-adaptation-tool-rem/#prettyPhoto)








