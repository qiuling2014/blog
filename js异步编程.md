# 异步编程

## 回调函数（Callback）

回调函数应该是大家经常使用到的，以下代码就是一个回调函数的例子：

```javascript
ajax(url, () => {
  // 处理逻辑
})
```

但是回调函数有一个致命的弱点，就是容易写出回调地狱（Callback hell）。假设多个请求存在依赖性，你可能就会写出如下代码：

```javascript
ajax(url, () => {
  // 处理逻辑
  ajax(url1, () => {
    // 处理逻辑
    ajax(url2, () => {
      // 处理逻辑
    })
  })
})
```

```js
for (var i = 0; i < 3; ++i) {  
  setTimeout( function(){  
    console.info( 'setTimeout: ' + i);  
  }, 0);  
  console.info( 'for循环: ' + i)  
}
// ====== 运行结果 =======
// for循环：0
// for循环：1
// for循环：2
// setTimeout：3
// setTimeout：3
// setTimeout：3
```

## Promise
> promise意为`承诺`，是对未来的一种许诺，这个未来就是指稍后某一刻将要执行的js代码，而在js中”稍后执行”意味着它不会阻塞当前js代码，也可以说这段代码是异步的。

Promise有三种状态: 
* 等待中（pending）
* 完成了 （resolved）
* 拒绝了（rejected）

一旦从等待状态变成为其他状态就永远不能更改状态了。

Promise 实现了链式调用，也就是说每次调用 `then` 之后返回的都是一个 `Promise`，并且是一个全新的 `Promise`，原因也是因为状态不可变。如果你在 `then` 中 使用了 `return`，那么 return 的值会被 `Promise.resolve()` 包装。

```javascript
Promise.resolve(1).then(res => {
  console.log(res) // => 1
  return 2 // 包装成 Promise.resolve(2)
}).then(res => {
  console.log(res) // => 2
});
```

当然了，Promise 也很好地解决了回调地狱的问题，可以把之前的回调地狱例子改写为如下代码：

```javascript
ajax(url)
  .then(res => {
    console.log(res)
    return ajax(url1)
  }).then(res => {
    console.log(res)
    return ajax(url2)
  }).then(res => console.log(res)
```

Promise存在一些缺点的，比如无法取消Promise，错误需要通过回调函数捕获。

> 待补充async await