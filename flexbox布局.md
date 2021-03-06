# flexbox 快速布局神器

对于一个前端仔来说，实现设计师的页面是平时工作中常有的事情，而实现一个页面是从【页面布局】->【样式编写】->【添加交互】的过程，万事开头难，页面布局合理性极大的决定了后两步的实现难度及工作量。好的页面布局能给前端工作带来极大的便利。

---

依稀的记得曾经在某次面试的时候，面试官问‘页面布局的方式有哪一些？’，当时是这么答的：div+css、table两种布局方式。现在来看这份答案，我也不能说它错了，但也感觉跟没回答似的。其实更合适的回答应该是：基于盒状模型，依赖 display属性 + position属性 + float属性、table布局（不推荐使用）。

那么问题来了，我们都知道【盒子模型】对于那些特殊布局非常不方便，比如，垂直居中就不容易实现。

面试题：

      <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
      </ul>

      界面显示效果： 5 4 3 2 1

---

我们的主角✨闪亮登场~

> 在2009年，W3C提出了一种新的方案----Flex布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。


新技术的出现大家最关心的是它的兼容性如何，能不能再IE6上使用啊？移动端的表现如何呀？在应该是大家最关心的问题。

![](http://7xpwoh.com1.z0.glb.clouddn.com/16-1-8/23878516.jpg)

兼容性在移动端表性很好，无论安卓，还是iOS，甚至连有着'移动端IE6'之称的UC、QQ浏览器都能支持，所以可以放心大胆的使用。在桌面端的浏览器（除了IE）都支持挺好。

---

### 一、flex布局是什么
Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

``任何一个容器都可以指定为Flex布局``

    .box{
      display: flex;
    }

``行内元素也可以使用Flex布局``

    .box{
      display: inline-flex;
    }

``Webkit内核的浏览器，必须加上-webkit前缀``

  .box{
    display: -webkit-flex;
    display: flex;
  }

``兼容写法``

    .box{
      display: -moz-box; /* Firefox */
      display: -ms-flexbox; /* IE10 */
      display: -webkit-box; /* Safari */
      display: -webkit-flex; /* Chrome, WebKit */
      display: box;
      display: flexbox;
      display: flex;
    }

注意：设为Flex布局以后，子元素的float、clear和vertical-align属性将失效

flexbox不同阶段的语法
* display:box; 或者 box-{\*} 属性，2009年版本的Flexbox。
* display:flexbox; 或者 flex() 函数，2011年版本的Flexbox。
* display:flex; 和 flex-{\*} 属性，当前版本的Flexbox。

### 二、基本概念
>采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。

### 三、容器的属性
>容器就是设置display:flex的盒子

  * flex-direction，主轴的方向（即项目的排列方向）
  * flex-wrap， 默认情况下，item都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。
  * flex-flow，flex-direction属性和flex-wrap属性的简写形式
  * justify-content，定义了item在主轴上的对齐方式
  * align-items，定义item在交叉轴上如何对齐
  * align-content，定义了多根轴线的对齐方式

### 四、item的属性
  >容器成员

  * order，定义项目的排列顺序。数值越小，排列越靠前，默认为0
  * flex-grow，定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
  * flex-shrink，定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
  * flex-basis，定义了在分配多余空间之前，项目占据的主轴空间（main size）
  * flex，flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto
  * align-self,允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性


### 五、例子
HTML：

![](http://7xpwoh.com1.z0.glb.clouddn.com/16-1-8/63830365.jpg)

CSS：

![](http://7xpwoh.com1.z0.glb.clouddn.com/16-1-8/69984896.jpg)

效果：

![](http://7xpwoh.com1.z0.glb.clouddn.com/16-1-8/64295263.jpg)


> 这个例子完美解决了我们那道面试题，当然还有别的属性也可以实现，比如：对容器设置：flex-direction:row-reverse;

### 写在最后

了解一个东西，我们知道一个东西能解决什么问题，有什么优缺点，适合在什么场景下使用就够了，至于使用细节，在我们使用的时候自然而自然能够掌握，在此就不加以说明。
