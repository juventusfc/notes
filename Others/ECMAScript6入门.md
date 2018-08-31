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
