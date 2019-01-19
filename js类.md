```javascript
function Fruit(name) {
  if (!(this instanceof Fruit)) {
    return new Fruit(name);
  }
  this.name = name;
}
var apple = Fruit('apple');
console.log(apple.name);
```
#### new 的过程

```js
var Person = function(){};
var p = new Person();

```
new的过程拆分成以下三步：
(1) var p={}; 也就是说，初始化一个对象p
(2) p.__proto__ = Person.prototype;
(3) Person.call(p); 也就是说构造p，也可以称之为初始化p

补充：function call 、function apply
