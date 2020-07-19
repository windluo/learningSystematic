## Class

### 基础概念

ES6 引入 Class 概念是为了让对象更接近面向对象的写法。Class 的绝大部分功能，都可以通过 ES5 实现。

```js
// ES5 定义对象
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);

// ES6 定义对象
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

var p = new Point(1, 2);
```

**注意：**，Class 里的方法之间不要加逗号，会报错；Class 内的方法不需要加`function`关键字。

构造函数的`prototype`属性，在 ES6 的“类”上面继续存在。事实上，类的所有方法都定义在类的`prototype`属性上面。

#### constructor 方法

`constructor`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。

#### 类的实例

Class 的实例必须通过`new`创建，直接调用会报错。

```js
class Point {
  // ...
}

// 报错
var point = Point(2, 3);

// 正确
var point = new Point(2, 3);
```

#### 取值函数（getter）和存值函数（setter）

与 ES5 一样，在“类”的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'

```

**注意：** Class 不存在变量提升。

#### 类的`this`指向

默认指向类的实例。如果单独使用，会报错。

```js
class Logger {
  printName(name = 'there') {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

解决方案一，在构造函数里绑定`this`。这也是最简单的方法。

```js
class Logger {
  constructor() {
    this.printName = this.printName.bind(this);
  }

  // ...
}
```

解决方案二，使用箭头函数。因为箭头函数内部的`this`总是指向定义时所在的对象。

```js
class Logger {
  printName = (name = 'there') => {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // hello there
```

上面的箭头函数的`this`在实例`logger`构建时生效，所以后面始终指向`logger`实例。

#### 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

**注意**，如果静态方法包含`this`关键字，这个`this`指的是类，而不是实例。

父类的静态方法，可以被子类继承。

静态方法也是可以从`super`对象上调用的。

#### 实例属性的新写法

实例属性除了定义在`constructor()`方法里面的`this`上面，也可以定义在类的最顶层。

```js
class IncreasingCounter {
  _num = 1; // 这个也是类的属性，等价于constructor里的_count，一目了然。
  constructor() {
    this._count = 0;
  }
  get value() {
    console.log('Getting the current value!');
    return this._count;
  }
  increment() {
    this._count++;
  }
}
```



### Class 的继承

Class 可以通过`extends`关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

```js
class Point {
  constructor(x, y){
    this.x = x;
    this.y = y;
  }
  
  toString() {
	return this.x + ' ' + this.y
  }
}

// 完整的代码
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

上面代码中，`constructor`方法和`toString`方法之中，都出现了`super`关键字，它在这里表示父类的构造函数，用来新建父类的`this`对象。

子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。这是因为子类自己的`this`对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用`super`方法，子类就得不到`this`对象。

**注意：**在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字，否则会报错。这是因为子类实例的构建，基于父类实例，只有`super`方法才能调用父类实例。

#### super 关键字

它的用法有两种：

- 作为函数使用，代表父类的构造函数。在ES6中，子类必须执行一次 super 函数。原因见上面。
- 作为对象使用，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。



### 类的prototype 和 \_proto_

prototype属性属于对象本身，挂载一些可以继承给子类的属性和方法。

\_proto_ 表示构造函数的继承，总是指向父类的原型（prototype）。

> 所以 ES5 实现继承靠`prototype`。



### 总结

ES6 引入`Class`来定义类。

类的继承通过`extends`来实现。在子类构造函数里面一定要先调用`super()`。

prototype 和 \_proto_。