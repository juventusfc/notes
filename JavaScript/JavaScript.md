# JavaScript

## 文法

### 词法

### 语法

## 语义

## 运行时

### 数据结构

#### 类型

- Undefined
  只有一个值，那就是 undefined。  
  在老的浏览器中，由于 JavaScript 的设计缺陷，undefined 不是一个关键字，也就是说 undefined 可以被覆盖。为了弥补这个缺陷，引入 void。void 是一种操作符，执行 void 之后的代码并返回 undefined。一般用于得到 undefined。  
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
- String
- Symbol
- Object

#### 对象

### 算法

#### 事件循环

#### 微任务的执行

#### 函数的执行

#### 语句级的执行
