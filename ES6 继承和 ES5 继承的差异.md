# ES6继承和ES5继承的差异

## ES5继承

### 基于构造函数的继承

```javascript
function Parent1() {
  this.parent1 = 'parent1'
  this.arr = [1, 2, 3]
}

Parent1.prototype.walk = 'I can walk'

function Child1() {
  Parent1.call(this)
  this.child1 = 'child1'
}

const child11 = new Child1()
const child12 = new Child1()

// 测试
child11.parent1 // parent1
child11.walk    // undefined

child11.arr.push(4)
child11.arr // [1, 2, 3, 4]
child12.arr // [1, 2, 3]
```

优点：构造函数的属性具有唯一性(child11.arr 与 child12.arr)
缺点：只能继承父类函数的方法和属性，而不能继承原型对象的方法和属性

### 基于原型链的继承

```javascript
function Parent2() {
  this.parent2 = 'parent2'
  this.arr = [1, 2, 3]
}

Parent2.prototype.walk = 'I can walk'

function Child2() {
  this.child2 = 'child2'
}

Child2.prototype = new Parent2()

const child21 = new Child2()
const child22 = new Child2()

// 测试
child21.parent2 // parent2
child21.walk    // I can walk

child21.arr.push(4)
child21.arr // [1, 2, 3, 4]
child22.arr // [1, 2, 3, 4]
```

优点：继承父类函数的方法和属性同时继承了原型对象的方法和属性
缺点：当在一个子类的实例上修改父类的属性和方法时，其它的子类也会受到影响(相当于改变了子类原型链上的属性

### 组合继承

```javascript
function Parent3() {
  this.parent3 = 'parent3'
  this.arr = [1, 2, 3]
}

Parent3.prototype.walk = 'I can walk'

function Child3() {
  Parent3.call(this)  // 这里执行第一次父类
  this.child3 = 'child3'
}

Child3.prototype = new Parent3() // 这里执行第二次父类，原型继承发现只要把父子的原型对象绑定起来就好，可以写成 Child3.prototype = new Parent3()>__proto__ 也正常
const child31 = new Child3()
const child32 = new Child3()

// 测试
child31.parent3 // parent3
child31.walk    // I can walk

child31.arr.push(4)
child31.arr // [1, 2, 3, 4]
child32.arr // [1, 2, 3]
```

优点：结合了构造函数继承和原型链继承的优点
缺点：父类方法调用了两次，而且不能判断实例的构造函数是子类还是父类

### 组合继承优化  

```javascript
function Parent5() {
  this.parent5 = 'parent5'
  this.arr = [1, 2, 3]
}

Parent5.prototype.walk = 'I can walk'

function Child5() {
  Parent5.call(this)
  this.child5 = 'child5'
}

Child5.prototype = Object.create(Parent5.prototype) // Object.create() 创建了一个中间对象，起到隔离子类和父类的作用。
Child5.prototype.constructor = Child5
const child51 = new Child5()
const child52 = new Child5()

// 测试
child51.parent5 // parent5
child51.walk    // I can walk

child51.arr.push(4)
child51.arr // [1, 2, 3, 4]
child52.arr // [1, 2, 3]

child51.constructor // Child5
```

> Object.create() 的作用：Object.create(Parent5.prototype) 相当于一个空对象 {}，这个空对象的 proto 等于 Parent5.prototype，所以这时候我们修改 Child5.prototype.constructor 实际上是在空对象上加上 constructor 属性。

## ES6继承

`ES6`通过`class`关键字，可以定义类，通过`extends`关键字实现继承，这比`ES5`的通过修改原型链实现继承，要清晰和方便很多。

```javascript
class Point {
  constructor(x, y) {

  }

  toString() {

  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```
> 子类必须在constructor方法中调用super方法，否则新建实例时会报错，如果子类没有定义constructor方法，这个方法会被默认添加。
另一个需要注意的地方是，在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错，这是因为子类实例的构建，基于父类实例，只有super方法才能调用父类实例。

