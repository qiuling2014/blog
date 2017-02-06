## js中的getter与setter


> 在看Vue的API时，里面提到修改Model层，会实时更新View视图，底层原理利用的是ES5的getter和setter方法，通过 Object.defineProperty 把属性全部转为 getter/setter。故温故一遍getter和setter定义属性的方法。

#### 通过对象字面量定义get和set方法

有个注意的地方，get与set的函数体都不能再定义本身该属性，否则执行的时候会陷入死循环，抛出栈溢出。

- 使用get语法时，不能带参数；然而set必须有一个明确的参数。
- 在对象字面量中，同一个属性不能有两个get，也不能既有get又有属性键值

```javascript
//不允许使用
{ 
  get x() { },
  get x() { }
} 
{ 
  x: …,
  get x() { }
}
```

- 在同一个对象中，不能为一个已有真实值的变量使用 set ，也不能为一个属性设置多个 set。

```javascript
//不允许使用
{ 
  set x(v) { }, 
  set x(v) { } 
} 
{ 
  x: …, 
  set x(v) { } 
}
```

- get和set都能用delete方法删除

```javascript
var o = {
  set current (str) {
    return this.log[this.log.length] = str;
  },
  log: []
}
```
> 此处current的值为undefined

```javascript
var o = {
  set current (str) {
    this.current = str 
    return this.log[this.log.length] = str;
  },
  log: []
};
o.current = 'a' //抛出错误,进入死循环
//Uncaught RangeError: Maximum call stack size exceeded
```

#### 使用 Object.defineProperty 方法
- 与对象字面量不同，使用 Object.defineProperty 方法可以为任何已存在的属性重新定义get与set方法。
- get的返回值直接为该属性的值。
- 可以定义configurable、enumerable、writable的值，默认都为false。

```javascript
var o = { a:0 }

Object.defineProperty(o, "b", { get: function () { return this.a + 1; } });

console.log(o.b) // Runs the getter, which yields a + 1 (which is 1)
```

[原文链接](http://blog.csdn.net/wkyseo/article/details/53996012)
