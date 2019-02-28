# JS和JQuery中的事件委托

## 什么是事件委托

事件委托通俗来讲就是利用冒泡的原理，把事件追加到父级，触发执行效果。

比如当一个父节点下的多个子节点绑定相同的事件时，可以利用事件冒泡机制，将事件委托给父元素执行，这时候只需要绑定一个事件便可以触发子元素的事件。

优点：
* 只需注册一个事件，节省大量内存空间；
* 可以实现 当新增子元素时不需再为其绑定事件，尤其是动态部分的内容，比如Ajax不需再为新增的元素进行绑定和对删除的元素解除绑定。

缺点：
* 事件委托是基于事件冒泡的，如果不支持事件冒泡则不能进行事件委托
* 如果层级过多，冒泡过程中可能受阻

使用场景：
* 为多个元素绑定相同事件
* 为尚不存在的元素绑定事件

## 原生JS实现事件委托

``` html
<ul id="ul">
  <li>111</li>
  <li>222</li>
  <li>333</li>
</ul>
```

``` javascript
const ulElem = document.getElementById("ul");
ulElem.onclick = function(e) {
  const e = e || window.event;
  const target = e.target ||  e.srcElement;
  if (target.nodeName.toLowerCase() === 'li') {
    alert(123);
  }
}
```

## JQuery中的事件委托

```javascript
$("ul").on('click', 'li', function() {
  alert(123);
});
```

原文链接：[https://www.jianshu.com/p/85d0cd34b28c](https://www.jianshu.com/p/85d0cd34b28c)