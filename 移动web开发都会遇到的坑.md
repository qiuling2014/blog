# 移动web开发都会遇到的坑

## 一、技巧篇

### 1、自适应第一招
```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no" />
```	

### 2、IOS允许全屏浏览
```html
<meta content=”yes” name=”apple-mobile-web-app-capable” /> 
```

### 3、忽略数字是电话号码
```html
<meta content=”telephone=no” name=”format-detection” /> 
```

### 4、去除Android对邮箱地址的识别
```html
<meta content=”email=no” name=”format-detection” /> 
```

### 5、去除URL控件条
```javascript
setTimeout(scrollTo,0,0,0); 
```

### 6、禁止旋转设备
```
	No way，IOS和Android在浏览器里面没法禁止浏览器旋转。
```

### 7、关闭IOS键盘自动大写

我们知道在iOS中，当虚拟键盘弹出时，默认情况下键盘是开启首字母大写的功能的，根据某些业务场景，可能我们需要关闭这个功能，移动版本webkit为 input元素提供了autocapitalize属性，通过指定autocapitalize=”off”来关闭键盘默认首字母大写。

### 8、iOS中如何彻底禁止用户在新窗口打开页面
```css
-webkit-touch-callout:none;
```

### 9、IOS禁止用户复制或者保存图片
```css
-webkit-touch-callout:none;
```

### 10、IOS禁止选中文字
```css
-webkit-user-select:none
```

### 11、IOS获取滚动条高度

桌面浏览器中想要获取滚动条的值是通过document.scrollTop和document.scrollLeft得到的，但在iOS中你会发现这两 个属性是未定义的，为什么呢？因为在iOS中没有滚动条的概念，在Android中通过这两个属性可以正常获取到滚动条的值，那么在iOS中我们该如何获 取滚动条的值呢？
通过window.scrollY和window.scrollX我们可以得到当前窗口的y轴和x轴滚动条的值。

### 12、解决边框溢出
```css
width:100%;
-webkit-box-sizing:border-box;
```

### 13、输入框获取焦点后默认弹出数字键盘

```html
//记得使用type="tel"，而不是type="number"
<input type="tel" placeholder=""/>
```
	
### 14、input type="number"去除上下箭头

chrome下：
```css
input::-webkit-outer-spin-button,input::-webkit-inner-spin-button{  
	-webkit-appearance: none !important;
	margin: 0;
}
```	
firefox下：
```css
input[type="number"]{-moz-appearance:textfield;}
```

### 15、清楚系统默认样式
webkit上的input,button,及select的默认样式可以通过以下代码禁用，然后自定义。
```css
-webkit-appearance:none;
```

## 二、遇到的坑
### 1、页面高度渲染错误	
在各移动端浏览器中经常会出现这种页面高度100%的渲染错误，页面低端和系统自带的导航条重合了，高度的不正确我们需要重置修正它，通过javascript代码重置掉：

```javascipt
document.documentElement.style.height = window.innerHeight + 'px';
```

### 2、ios fix 定位问题
问题：如果一个应用了css3 transform的元素里面包含fixed定位的元素，在webkit核心的浏览器下，该fixed定位的元素会失效，其实际定位效果类似绝对定位，会跟着滚动条的滚动而滚动！
解决方案：避免在应用了css3 transform的元素内部嵌套fixed定位的元素。

### 3、iOS 动态修改title不生效

### 4、对于单位vw，ios微信可以正确支持，Android则不行

### 5、border-radius和border的计算，ios是先对元素进行剪切在添加边框，Android则是先加border再进行剪切。
