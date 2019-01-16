# 使用CreateJs实现拼图小游戏

<img src="https://s2.ax1x.com/2019/01/16/kS8E6A.md.jpg" width="300" >

## 初识CreateJs
### CreateJs包含四部分：
* EaselJS：用于 Sprites 、动画、向量和位图的绘制
* TweenJS：用于做动画效果
* SoundJS：音频播放引擎
* PreloadJS：网站资源预加载

> 我们游戏功能较为简单，只需要用到`EaselJS`

### EaselJS常用API
* 画图片用 (`Bitmap`)
* 画图形，比如矩形，圆形等用 (`Shape`) 【类似于改变坐标x，y，增加阴影 Shadow ，透明度 Alpha ，缩小放大 ScaleX/ScaleY 都可以做到】
* 画文字，用 (`Text`)
* 还有容器 `Container` 的概念，容器可以包含多个显示对象

### EaselJS 绘图的大致流程

创建显示对象 → 设置参数 → 调用方法绘制 → 添加到舞台 → 刷新舞台

```javascript
const canvas = document.querySelector('#canvas');
//创建舞台
const stage = new createjs.Stage(canvas);
//创建一个Shape对象，此处也可以创建文字Text,创建图片Bitmap
const rect = new createjs.Shape();
//用画笔设置颜色，调用方法画矩形，矩形参数：x,y,w,h
rect.graphics.beginFill("#f00").drawRect(0, 0, 100, 100);
//添加到舞台
stage.addChild(rect);
//刷新舞台
stage.update();
```

> 问题：修改对象参数，要应用到舞台上，需要刷新舞台，如果对象频繁发生改变，将会很烦人，代码会比较冗余。

### Tick 事件

```javascript
createjs.Ticker.addEventListener(“tick”, tick);
function tick(e) {
    if (e.paused !== 1) {
        //处理
        stage.update();  //刷新舞台
    }else {}
}
createjs.Ticker.paused = 1;  //在函数任何地方调用这个，则会暂停tick里面的处理
createjs.Ticker.paused = 0;  //恢复游戏
```

> 在暂停的时候，页面仍会触发 Tick 事件，如果需要完全移除 Tick 事件，则使用

```javascript 
createjs.Ticker.removeEventListener("tick", tick);
```

### 滑动事件

EaselJS 事件默认是不支持 `Touch` 设备的，需要以下代码才支持：

```javascript
createjs.Touch.enable(stage);
```

对于`Bitmap`，`Shape`等对象，都可以直接使用 `addEventListener` 或者 `on` 进行事件监听。

```javascript 
bitmap = new createjs.Bitmap('');
// 绑定touch
bitmap.on('mousedown'，handle);
bitmap.on('pressup'，handle);
```

通过开始点的坐标（stageX，stageY）、结束点的坐标（stageX，stageY），计算滑动方向。

## 绘制图像并对图像进行操作

绘制图片
```javascript
const imgPath = 'http://www.abc.com/img/bg.jpg';
const img = new Image();
img.src = imgPath;
img.onload = function () {
  const canvas = document.querySelector('#canvas');
  //创建舞台
  const stage = new createjs.Stage(canvas);
  const bg = new createjs.Bitmap(imgPath);
  stage.addChild(bg);
  stage.update();
}
```

> 由于我们没有做资源预加载（PreloadJS），资源需要在图片资源加载成功后进行`new`，否则不会有图像在画布上。

仅仅绘制图片是不够的， CreateJS 提供了几种处理图片的方法：

* 给图片增加遮罩层，使用 `mask` 属性，可以只显示图片和 `shape` 相交的区域

```javascript
const imgPath = 'http://www.abc.com/img/bg.jpg';
const img = new Image();
img.src = imgPath;
img.onload = function () {
  const canvas = document.querySelector('#canvas');
  //创建舞台
  const stage = new createjs.Stage(canvas);
  const bg = new createjs.Bitmap(imgPath);
  const shape = new createjs.Shape();
  shape.graphics.beginFill("#000").drawCircle(0, 0, 100);
  bg.mask = shape;     //给图片bg添加遮罩
  stage.addChild(bg);
  stage.update();
}
```

常用应用场景：用来剪裁图片，比如显示圆形的图片等

* 使用 `Rectangle` 剪裁图片
使用 `EaselJS` 内置的 `Rectangle` 对象来创建一个选取框，显示图片的某各部分。

```javascript
const imgPath = 'http://www.abc.com/img/bg.jpg';
const img = new Image();
img.src = imgPath;
img.onload = function () {
  const canvas = document.querySelector('#canvas');
  //创建舞台
  const stage = new createjs.Stage(canvas);
  const bg = new createjs.Bitmap(imgPath);
  var rect = new createjs.Rectangle(0, 0, 120, 120);
  bg.sourceRect = rect;

  stage.addChild(bg);
  stage.update();
}
```
## 拼图游戏
> 九宫格，一张图片被划分为九小块，其中一块固定为白色，随机打乱顺序，根据手势滑动方向，交换白色小块位置，直到所有小方块顺序完全正确，游戏结束。

* 图片分割为九个小方块，随机打乱顺序
* 监听滑动事件，找到白色方块交互的目标对象
* 方块位置更新
* 判断小方块顺序是否正确

<img src="https://s2.ax1x.com/2019/01/16/kS8pTK.png" width="200" hegiht="200" align=center />

初始化数据

```javascript
function initData(length) {
  const arr = [];
  for (let i = 0; i < length; i++) {
    arr[i] = i;
  }
  arr.sort(() => (0.5 - Math.random())); // 随机打乱顺序，该顺序为小方块显示顺序
  return {
    arr, // 九宫格中，每个格显示的方块的编号
    blank: 8, // 白色小方块编号
    blankIndex: arr.indexOf(8) // 白色小方块位置
  };
}
```

默认参数
```javascript
// 参数
const opt = {
  canvasWidth: 540, // 图片宽度
  canvasHeight: 540, // 图片高度
  width: 180, // 每个小方块的宽度
  height: 180, // 每个小方块的高度
  gap: 4, // 空隙
  imgPath: "./1.jpg", // 图片
  data: initData(9) 
}
const arr = []; // 存放小方块
```

图片分割为九个小方块，随机打乱顺序
```javascript
const canvas = document.querySelector('#canvas');
//创建舞台
const stage = new createjs.Stage(canvas);
// 获取当前位置显示的图片编号
opt.data.arr.forEach((item, index) => {
  let bg;
  // 方块在画布上的坐标计算
  const x = (index % 3) * opt.width + (index % 3) * opt.gap;
  const y = Math.floor(i / 3) * opt.height + Math.floor(index / 3) * opt.gap;
  // 判断是否白色小方块
  if (opt.data.blankIndex === index) {
    bg = new createjs.Shape();
    bg.graphics.beginFill("#fff").drawRect(0, 0, opt.width, opt.height); // 绘制白色正方形
  } else {
    bg = new createjs.Bitmap(opt.imgPath);
    // 根据位置编号裁剪图片，参数为起点坐标x、y，宽，高
    const rect = new createjs.Rectangle((item % 3) * opt.width, Math.floor(item / 3) * opt.height, opt.width, opt.height);
    bg.sourceRect = rect;
  }
  bg.x = x;
  bg.y = y;
  bg.name = index; // 记录方块编号
  this.arr.push(bg);
  stage.addChild(bg);
});
```

检测是否完成
```javascript
  // arr为存放小方块的数组
  function check() {
    for (let i = 0; i < arr.length; i++) {
      if (arr[i].name !== i) {
        return false;
      }
    }
    return true;
  }
```

核心逻辑已经完成，剩下就是绑定touch事件，位置交换而已。

## 总结

* 了解了CreateJS的组成部分，及分工；
* EaselJS常用API介绍，绘图流程；
* 使用CreteJS实现一个小游戏；




