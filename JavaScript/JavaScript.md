# JavaScript

## 文法

### 词法

### 语法

## 语义

## 运行时

### 数据结构

#### 类型

- undefined  
  只有一个值，那就是 undefined。  
  在老的浏览器中，由于 JavaScript 的设计缺陷，undefined 不是一个关键字，也就是说 undefined 可以被覆盖。为了弥补这个缺陷，引入 void。void 是一种操作符，执行 void 之后的代码并返回 undefined。一般用于得到 undefined。  
  在新的浏览器中，可以直接使用 undefined。

  ```javascript
  void 0 == undefined; // true
  ```

  ```html
  <a href="javascript: void(0)">About</a>
  ```

- null  
  只有一个值，那就是 null。
- boolean  
  只有两个值，那就是 true/false。
- number
- string  
  JavaScript 中的字符串是永远无法变更的，一旦字符串构造出来，无法用任何方式改变字符串的内容，所以字符串具有值类型的特征。  
  因为 string 的意义并非“字符串”，而是字符串的 UTF16 编码，我们字符串的操作 charAt、charCodeAt、length 等方法针对的都是 UTF16 编码。所以，字符串的最大长度，实际上是受字符串的编码长度影响的。
- Symbol  
  Symbol 是 ES6 中引入的新类型，它是一切非字符串的对象 key 的集合，在 ES6 规范中，整个对象系统被用 Symbol 重塑。
- Object

#### 对象

### 算法

#### 事件循环

#### 微任务的执行

#### 函数的执行

#### 语句级的执行
