# JavaScript

## 文法

### 词法

### 语法

## 语义

## 运行时

### 数据结构

#### 类型

- Undefined

  只有一个值，那就是 undefined。在老的浏览器中，由于 JavaScript 的设计缺陷，undefined 不是一个关键字，也就是说 undefined 可以被覆盖。为了弥补这个缺陷，引入 void。void 是一种操作符，执行 void 之后的代码并返回 undefined。一般用于得到 undefined。  
  在新的浏览器中，可以直接使用 undefined。

  ```javascript
  void 0 == undefined; // true
  ```

  ```html
  <a href="javascript: void(0)">About</a>
  ```

- Null

  只有一个值，那就是 null。

- Boolean

  只有两个值，那就是 true/false。

- Number

  ```javascript
  new Number(value);
  var a = new Number("123"); // a === 123 is false。新建一个Number对象。
  var b = Number("123"); // b === 123 is true。新建一个Number类型。
  a instanceof Number; // is true
  b instanceof Number; // is false
  ```

- String

  JavaScript 中的字符串是永远无法变更的，一旦字符串构造出来，无法用任何方式改变字符串的内容，所以字符串具有值类型的特征。  
   string 的意义并非“字符串”，而是字符串的 UTF16 编码，我们字符串的操作 charAt、charCodeAt、length 等方法针对的都是 UTF16 编码。

- Symbol

  Symbol 是 ES6 中引入的新类型，它是一切非字符串的对象 key 的集合，在 ES6 规范中，整个对象系统被用 Symbol 重塑。

- Object

  对象的定义是“属性的集合”。属性分为数据属性和访问器属性，二者都是 key-value 结构，key 可以是字符串或者 Symbol 类型。事实上，JavaScript 中的“类”仅仅是运行时对象的一个私有属性，而 JavaScript 中是无法自定义类型的。

##### 类型转换

![type-trans](./images/type-trans.jpg)

- StringToNumber
  - Number()
  - parseInt()
  - parseFloat()
- NumberToString
  - toString()
- 装箱与拆箱
  - 装箱
  - 拆箱
    - 对象到 String 和 Number 的转换都遵循“先拆箱再转换”的规则。通过拆箱转换，把对象变成基本类型，再从基本类型转换为对应的 String 对象 或者 Number 对象。

##### typeof instanceof Object.prototype.toString()

- typeof 用于知道变量属于哪种类型
  ![typrof](./images/typeof.png)
- instanceof 用于判断是否是某个类的实例，涉及到原型链
- 在 JavaScript 中，没有任何方法可以更改私有的 Class 属性，因此 Object.prototype.toString 是可以准确识别对象对应的基本类型的方法，它比 instanceof 更加准确。

  ```javascript
  Object.prototype.toString.call([1, 2, 3]); //"[object Array]"
  Object.prototype.toString.call(new Date()); //"[object Date]"
  Object.prototype.toString.call(/a-z/); //"[object RegExp]"
  ```

_Use instanceof for custom types:_

```javascript
var ClassFirst = function() {};
var ClassSecond = function() {};
var instance = new ClassFirst();
typeof instance; // object
typeof instance == "ClassFirst"; // false
instance instanceof Object; // true
instance instanceof ClassFirst; // true
instance instanceof ClassSecond; // false
```

_Use typeof for simple built in types:_

```javascript
'example string' instanceof String; // false
typeof 'example string' == 'string'; // true

'example string' instanceof Object; // false
typeof 'example string' == 'object'; // false

true instanceof Boolean; // false
typeof true == 'boolean'; // true

99.99 instanceof Number; // false
typeof 99.99 == 'number'; // true

function() {} instanceof Function; // true
typeof function() {} == 'function'; // true
```

_Use instanceof for complex built in types:_

```javascript
/regularexpression/ instanceof RegExp; // true
typeof /regularexpression/; // object

[] instanceof Array; // true
typeof []; //object

{} instanceof Object; // true
typeof {}; // object
```

_And the last one is a little bit tricky:_

```javascript
typeof null; // object
```

#### 对象

##### 面向对象 VS 基于对象

对象的特点包括：

- 对象具有唯一标识性，一般来说是内存地址：即使完全相同的两个对象，也并非同一个对象。
- 对象有状态：对象具有状态，同一对象可能处于不同状态之下。
- 对象具有行为：即对象的状态，可能因为它的行为产生变迁。

JavaScript 中使用了原型来描述语言中的对象。Java/C++使用类来描述语言中的对象。在 JavaScript 中，状态和行为都用“属性”表示。JavaScript 也是面向对象的语言。

##### 数据属性 VS 访问器属性

为了更好得描述属性，引入了多种特征值。

- 数据属性
  - [[value]]：就是属性的值。
  - [[writable]]：决定属性能否被赋值。
  - [[enumerable]]：决定 for in 能否枚举该属性。
  - [[configurable]]：决定该属性能否被删除或者改变特征值。
- 访问器属性
  - [[getter]]：函数或 undefined，在取属性值时被调用。
  - [[setter]]：函数或 undefined，在设置属性值时被调用。
  - [[enumerable]]：决定 for in 能否枚举该属性。
  - [[configurable]]：决定该属性能否被删除或者改变特征值。

##### 原型

JavaScript 实现面向对象的方式是使用原型。

原型系统的“复制操作”有两种实现思路：

- 一个是并不真的去复制一个原型对象，而是使得新对象持有一个原型的引用；**JavaScript 选择了这种实现方式。**
- 另一个是切实地复制对象，从此两个对象再无关联。

原型系统的原理：

- 如果所有对象都有私有字段 [[prototype]]，就是对象的原型；
- 读一个属性，如果对象本身没有，则会继续访问对象的原型，直到原型为空或者找到为止。

操纵原型的方法：

- Object.create 根据指定的原型创建新对象，原型可以是 null；
- Object.getPrototypeOf 获得一个对象的原型；
- Object.setPrototypeOf 设置一个对象的原型。

###### [[prototype]] VS \_\_proto\_\_

\_\_proto\_\_ is the actual object that is used in the lookup chain to resolve methods, etc. [[prototype]] is the object that is used to build \_\_proto\_\_ when you create an object with new。[[prototype]]是对象的私有字段。

###### new

```javascript
function Foo() {
  // ...
}

var a = new Foo();

Object.getPrototypeOf(a) === Foo.prototype; // true
a.__proto__ == Foo.prototype; // true
```

`new 构造器` 和 [[prototype]] 一起理解，new 主要用于新建一个对象，新对象的[[__proto__]]指向函数的[[prototype]]。当函数调用时前面加了 new，该函数成为构造器。

1. 以构造器的 prototype 属性为原型，创建新对象；
2. 将 this 和调用参数传给构造器，执行；
3. 如果构造器返回的是对象，则返回，否则返回第一步创建的对象。

##### ES6 class

ES6 来了之后，推荐使用 class 来构造类。

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(this.name + " makes a noise.");
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // call the super class constructor and pass in the name parameter
  }

  speak() {
    console.log(this.name + " barks.");
  }
}

let d = new Dog("Mitzie");
d.speak(); // Mitzie barks.
```

### 算法

#### 事件循环

#### 微任务的执行

#### 函数的执行

#### 语句级的执行
