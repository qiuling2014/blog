# js实现Event Bus

```javascript
  class EventBus {
    constructor() {
      this.events = {};
    }

    emit(type, data) {
      const args = [...arguments].slice(1);
      if (type && this.events[type] && typeof this.events[type] === 'function') {
        this.events[type].call(this, ...args);
      } else {
        throw new Error('出错了')
      }
    }

    on(type, fn) {
      if (type && typeof fn === 'function') {
        this.events[type] = fn;
      } else {
        throw new Error('出错了')
      }
    }
  }
```

* 发布-订阅的优势很明显，做到了时间上的解耦和对象之间的解耦，从架构上看，MVC，MVVM都少不了发布-订阅的参与，我们常用的Vue也是基于发布-订阅的，最近会抽时间写下vue的源码实现，同样的node中的EventEmitter也是发布订阅的，之前也手写过它的实现。
* 发布-订阅同时也是有缺点存在的，创建订阅者本身要消耗一定的时间和内存，而且当你订阅一个消息以后，可能此消息最后都未发生，但是这个订阅者会始终存在于内存中。如果程序中大量使用发布-订阅的话，也会使得程序跟踪bug变得困难。
