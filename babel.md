# babel

> babel处理闭包问题

```javascript
// es6
for (let i = 0; i < 5; i++) {
  setTimout(() => {
    console.log(i);
  }, 0)
}
```

```javascript
// babel转es5
"use strict";

var _loop = function _loop(i) {
  setTimout(function () {
    console.log(i);
  }, 0);
};

for (var i = 0; i < 5; i++) {
  _loop(i);
}
```

