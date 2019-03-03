# ECMAScript6 入门

## let 和 const 命令

## 变量的解构赋值

- 数组中的变量

  ```javascript
  // 模式匹配的一种。只要两边的模式相同，左边的变量就会被赋值。
  // 当解构不成功时，左边的变量为 undefined。
  var [a, b, c, d = 10086] = [1, 2, 3];
  a; // 1
  b; // 2
  c; // 3
  d; // 10086 默认值
  ```

- 对象中的变量

  ```javascript
  // 变量名与属性名一致
  var { foo, bar, goo } = {
    foo: "aaa",
    bar: "bbb"
  };
  /* 等价于
  var { foo: foo, bar: bar, goo: goo } = {
      foo: "aaa",
      bar: "bbb"
  };
  前一个foo为对象的属性名，后一个foo为定义的变量
  */
  foo; // "aaa"
  bar; // "bbb"
  goo; // undefined
  ```

  ```javascript
  // 变量名与属性名不一致
  var { foo: baz } = {
    foo: "aaa",
    bar: "bbb"
  };
  baz; // "aaa"
  foo; // Error! foo is not defined!
  ```

## 字符串的扩展

### 常用方法

- `contains()`
- `startsWith()`
- `endsWith()`

- `repeat()`: `hello.repeat(2); // "hellohello"`

### 模板字符串

```javascript
var name = "Frank";
var result = `Hello ${name}`; // 变量名写在${}中
```

## 数值的扩展

## 数组的扩展

- `Array.from(arrayLike[, mapFn[, thisArg]])`将类数组对象或可遍历对象转换为真正的数组。注意，是浅复制

  ```javascript
  var a = [
    "frank",
    {
      name: "Buick"
    }
  ];
  var b = Array.from(a);
  a[1] === b[1]; // true
  ```

- `Array.of(element0[, element1[, ...[, elementN]]])`将一组值转换为数组

- 数组实例
  - `find()`
  - `fill()`
  - `entries()`和`keys()`和`values()`都返回遍历器，可以用`for...of`遍历

## 对象的扩展

- `Object.assign(target, ...sources)`:将源对象的所有可枚举属性浅复制到目标对象上

- `增强的对象写法`

  ```javascript
  var person = {
    name: "frank",
    birth, // 等同于birth: birth
    hello() {} // 等同于hello: function(){}
  };
  ```

- `symbol`ES6 新增的原生类型，表示独一无二的值，适合做标识符

- `Proxy` 代理对象

## 函数的扩展

- 函数参数默认值

  ```javascript
  function Point(x = 0, y = 0) {
    this.x = x;
    this.y = y;
  }
  var p = new Point();
  p; // { x: 0, y: 0 }
  ```

- `...`运算符

  ```javascript
  // 1. 在函数定义时使用
  function add(...values) {
    let sum = 0;
    for (var val of values) {
      // values为数组
      sum += val;
    }
    return sum;
  }
  add(1, 2, 3); // 6

  // 2. 在函数调用时使用
  function add(value1, value2) {
    sum = value1 + value2;
    return sum;
  }
  add(...[1, 2]); // 3。将[1,2]数组转换为逗号分隔的参数序列
  ```

- 箭头函数

## Set 和 Map 数据结构

### Set

类似于数组，但是成员值是唯一的。

```javascript
var s = new Set();
[1, 2, 4, 3, 1].map(x => s.add(x));
for (i of s) {
  console.log(i); // 1 2 4 3
}
```

- `size`

- `add()`

- `delete(value)`

- `has(value)`

- `clear()`

- Set 与 Array 相互转换

  ```javascript
  var set = new Set([1, 2, 3]); // {1,2,3}
  var arr = Array.from(set); //[1,2,3]
  ```

### Map

JavaScript 的对象本质上也是`key-value`键值对，但是只能用字符串当做 key。

- `size`
- `set(key, value)`
- `get(key)`
- `has(key)`
- `delete(key)`
- `clear()`
- `keys()` 返回键名遍历器
- `values()` 返回键值遍历器
- `entries()` 返回所有键值对

### WeakMap

只接受对象作为 key 的特殊的 Map。专用场合是它的 key 所对应的对象，可能会在未来消失，当这个对象被回收后，WeakMap 自动移除对应的键值对。有助于防止内存泄漏。

```javascript
var m = new WeakMap();
var a = { name: "frank" };
m.set(a, "hello");
m.get(a); // "hello"

a = null;
m.get(a); // undefined
m.has(a); // false
```

## Iterator, Iterable 和 generator

遍历器是一种协议，任何对象只要部署这个协议，就可以完成遍历操作。在 ES6 中，遍历操作特指`for...of`循环。

### Iterator

Iterator 是一个具有`next()`方法的对象，它存在的目的是为了改善遍历的体验。`next()`返回一个包含`value`和`done`属性的对象。

```javascript
function makeIterator(array) {
  var nextIndex = 0;

  return {
    next: function() {
      return nextIndex < array.length
        ? {
            value: array[nextIndex++],
            done: false
          }
        : {
            value: undefined,
            done: true
          };
    }
  };
}
var it = makeIterator(["a", "b"]); // it是一个Iterator
it.next().value; // 'a'
it.next().value; // 'b'
```

### Iterable 跟`for...of`有关

Iterable 是一个具有 Symbol.iterator 属性的对象。这个属性是一个函数，返回一个 iterator。 在 ES6 中，所有的集合类对象(Arrays, Sets, and Maps) 和 strings 都是 iterable。

Iterable 的存在是为了配合使用 ES6 的新遍历方式——`for...of`。

它的执行原理是这样的：在使用`for...of`遍历 iterable 时，

1. 会调用 iterable 内置的`Symbol.iterator`方法，产生一个 iterator
2. 在每一轮遍历中，调用 iterator 的 next 方法，得到具体的值。这个遍历过程直到 next 返回的对象的`done`为 true。

### Generator

Generator 返回一个 Iterator。Generator 函数的`function`关键字和括号之间有一个`*`，用以区分 generator 和普通函数。

```javascript
function* createIterator() {
  yield 3;
  yield 2;
  yield 1;
}

let iterator = createIterator(); // 返回Iterator
iterator.next(); // {value: 3, done: false}
iterator.next(); // {value: 2, done: false}
iterator.next(); // {value: 1, done: false}
iterator.next(); // {value: undefined, done: true}
```

## Promise

请参考 YDKJS
