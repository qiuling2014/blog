## Promise

> promise意为`承诺`，是对未来的一种许诺，这个未来就是指稍后某一刻将要执行的js代码，而在js中”稍后执行”意味着它不会阻塞当前js代码，也可以说这段代码是异步的。

#### javascript的线程模型
js是运行在单线程里的，称为UI线程，这个线程不光用于运行js代码，还负责事件的处理和UI的绘制

```js
for (var i = 0; i < 3; ++i) {  
  setTimeout( function(){  
    console.info( 'setTimeout: ' + i);  
  }, 16);  
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
#### 异步编程

- 多线程语言异步编程：新创建一个线程，让某段代码片段运行在新创建的线程中，从而使当前线程继续向下执行。
- js异步编程：js的异步是指那些`稍后执行`的代码片段，因为是`稍后执行`，所以不会阻塞当前代码的执行。
