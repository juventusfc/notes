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

##### typeof instanceof toString()

- typeof 用于知道变量属于哪种类型
  ![typrof](./images/typeof.png)
- instanceof 用于判断是否是某个类的实例，涉及到原型链
- 在 JavaScript 中，没有任何方法可以更改私有的 Class 属性，因此 Object.prototype.toString 是可以准确识别对象对应的基本类型的方法，它比 instanceof 更加准确。

#### 对象

### 算法

#### 事件循环

#### 微任务的执行

#### 函数的执行

#### 语句级的执行
