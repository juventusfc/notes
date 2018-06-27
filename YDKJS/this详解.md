## 主要知识点

this 是运行时绑定的。在不考虑 arrow function 的情况下，`跟声明 this 时的 scope 无关`。arrow function 默认 this 绑定到声明的地方。

## call-site 调用点

* call-site: 方法在哪里被真正调用，不是在哪里被声明—-**跟 this 关系很大。决定了 this 的值**
* call-stack: 方法执行时的执行栈。

通常，call-site 在执行栈之前。

```javascript
function baz() {
  // 执行栈是: `baz`
  console.log("baz");
  bar(); // <--bar 的调用点
}
function bar() {
  // 执行栈是: `bar`
  console.log("bar");
  foo(); // <-- `foo` 的调用点
}
function foo() {
  // 执行栈是: `foo`
  console.log("foo");
}
baz(); // <-- `baz` 的调用点
```

## 绑定规则及优先级

在 JavaScript 函数执行期间，this  指向的对象可以通过函数的调用点来确定。另外需要注意的是，这 4 个规则的优先级各不相同。接下来我们先看看这 4 个规则分别是什么，再确定它们的优先级。

### 默认绑定

默认绑定的 this 值由调用点决定。单独的函数调用一定是这种情况。比如来看下面的代码：

```javascript
function foo() {
  console.log( this.a );
}
var a = 2;
foo(); *// 2
```

在上面这段代码中，函数的调用点是全局作用域，那么默认绑定会生效，`this`  指向全局对象。  
如果函数运行在[strict mode](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Strict_mode)下，情况会有不同吗？我们来看代码：

```javascript
function foo() {
  "use strict";
  console.log(this.a);
}
var a = 2;
foo(); // TypeError: `this` is undefined
```

可以看到，在`strict mode`下，依然是默认绑定生效了，可是这时候  `this`  指向的不再是全局对象了，而是  `undefined`。

### 隐式绑定

这个规则需要检查的是，**当前函数的调用点是不是有上下文对象，或者被包含在某个对象之内**，来看这段代码：

```javascript
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
  foo: foo
};
obj.foo(); // 2
```

这里我们可以看到，函数  `foo`  是在对象外部声明，然后被对象内的属性引用，其实这和在对象内部声明函数一样，不会对函数的调用点产生影响。因为函数  `foo`  被包含在对象  `obj`  内部，那么通过  `obj`  对象调用的时候，`foo`  函数的上下文对象就是  `obj`，在函数内部，`this`  也就被绑定到  `obj`  对象。

如果存在`引用链`，this 指向最后一个引用

```javascript
function foo() {
  console.log(this.a);
}

var obj2 = {
  a: 42,
  foo: foo
};

var obj1 = {
  a: 2,
  obj2: obj2
};

obj1.obj2.foo(); // 42
```

`警惕隐式绑定变为默认绑定`。由于 bar 是对 foo 函数的另外一个引用，所以在调用 bar()时,相当于直接调用 foo()，调用点是 global。

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
};

var bar = obj.foo;

var a = "oops, global";

bar(); // "oops, global"
obj.foo; //2
```

更典型的应用是在回调上使用

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
};

var a = "oops, global";

setTimeout(obj.foo, 100); // "oops, global"
```

```javascript
function setTimeout(fn, delay) {
  // wait (somehow) for `delay` milliseconds
  fn(); // 调用点在此
}
```

相当于将 obj.foo 赋值给 fn，然后执行 fn()。fn 相当于另外一个对 foo 函数的引用。所以 this 指向了 global。

### 显式绑定 call apply 或 bind

在 JavaScript 中，所有的函数都可以调用  `call`  和  `apply`  来显式的绑定  `this`。`注意`：call 和 apply 是直接执行函数，bind 是返回一个新的函数。

```javascript
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2
};
foo.call(obj); // 2
```

上面这段代码显式的把  `foo`  函数的  `this`  绑定到  `obj`  对象上。

```javascript
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
var obj = {
  a: 2
};
var bar = function() {
  return foo.apply(obj, arguments);
};
var b = bar(3); // 2 3
console.log(b); // 5
```

事实上，你也经常会在实际代码中见到这样的一个方法：

```javascript
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
// simple `bind` helper
function bind(fn, obj) {
  return function() {
    return fn.apply(obj, arguments);
  };
}
var obj = {
  a: 2
};
var bar = bind(foo, obj);
var b = bar(3); // 2 3
console.log(b); // 5
```

这个模式很常见，所以 JavaScript 自己也提供了同样的方法，可以通过  `Function.prototype.bind`  来调用：

```javascript
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
var obj = {
  a: 2
};
var bar = foo.bind(obj);
var b = bar(3); // 2 3
console.log(b); // 5
```

`bind()`返回了一个新的函数，这个函数的  `this`  被强制绑定到我们指定的任意对象上，这里是  `obj`。

一些常用的 API 内置了显示绑定，如：

```javascript
function foo(el) {
  console.log(el, this.id);
}
var obj = {
  id: "awesome"
};
// use `obj` as `this` for `foo(..)` calls
[1, 2, 3].forEach(foo, obj); // 1 awesome  2 awesome  3 awesome
```

### new 绑定

JavaScript 中的任意函数，如果在调用的时候，前面  `new`  关键字，那么如下的几件事情会依次发生：

1.  新对象被创建；
2.  新创建的对象被加入到  `prototype`  链中,即新对象的`__propto__`指向函数的 `prototype`；
3.  新创建的对象被设为该函数调用的  `this`；
4.  除非该函数指定返回其他对象，否则新创建的对象将会成为返回值

来看下面这段代码：

```javascript
function foo(a) {
  this.a = a;
}
var bar = new foo(2);
console.log(bar.a); // 2
```

`new foo(...)`  把新创建的对象绑定为  `this`，这也就是  `new`  绑定。

注意：当要调用 class 内其他方法时，必须使用 this

```javascript
class A {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    this.sayHi(); // 必须使用this
    console.log("hello " + this.name);
  }
  sayHi() {
    console.log("forza juven");
  }
}
```

### 优先级

* 函数被  `new`  关键字调用了吗？如果是，那么  `this`  就是新创建的对象。

  > var bar = new foo()

* 函数通过  `apply`  或  `call`  或  `bind`  显式绑定了吗？如果是，`this`  指向显式指定的对象。

  > var bar = foo.call( obj2 )

* 函数通过隐式绑定调用了吗？如果是那么就是隐式调用的对象。

  > var bar = obj1.foo()

* 如果以上都不是，那么就是默认绑定，`this`  指向全局变量或者  `undefined` (strict mode 下)。
  > foo()

### 箭头函数绑定

ES6 的新特性。this 绑定到函数定义时的 scope 中。这个 scope 对应的对象由调用点决定。  
如下所示，箭头函数定义在 foo 函数中。当执行 foo 时，foo 被绑定到了 obj 上，那么，此时箭头函数中的 this 也被绑定到了 obj 上。

```javascript
// ES6 using arrow function
function foo() {
  setTimeout(() => {
    console.log(this.a);
  }, 100);
}

var obj = {
  a: 2
};

foo.call(obj); // 2
```

等价于

```javascript
// ES5
function foo() {
  var self = this;
  setTimeout(function() {
    console.log(self.a);
  }, 100);
}

var obj = {
  a: 2
};

foo.call(obj); // 2
```
