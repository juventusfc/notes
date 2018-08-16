# 作用域和闭包

## 编译、执行时发生的事

当执行 `var a = 2;` 时，发生了什么呢？  
`Compiler` 进行编译，询问 `Scope` 是否有 a 的值，如果没有，在 `Scope` 中新建 a。`Compiler` 编译完后，由 `Engine` 进行执行。在执行过程中，`Engine` 询问 `Scope` 是否有 a 。如果没有，去上一层查找。
![compile](./images/compile.PNG)

### LHS && RHS

在 `Engine` 询问 `Scope` 时，会涉及查找问题。分为两种查找方式：
LHS(left-hand side) 寻找变量容器本身，RHS(righte-hand side) 寻找变量容器中的值。如 `var a=2`，变量容器是 a，2 是其中的值。

如果在执行 `console.log(a)` 没找到 a，报 ReferenceError。如果 RHS 找到了，但执行了值不具备的方法或属性，报 TypeError。
![LHS](./images/LHS.PNG)

## 词法作用域

JS 的作用域类型是 Lexical Scope，有些语言的 Scope 类型是 Dynamic Scope(本文不做解释。如果有时间，以后再谈这种方式)。顾名思义，词法作用域就是指在 Compiler 执行时，变量所处的范围。一般来说，词法作用域就是指**代码书写时所处的代码块**。

### 作用域欺骗

有 `eval` 和 `with` 两种方式。但由于这两种是执行时才会确定代码，所以会**有性能问题**。尽量避免使用。

- eval

  `eval(str)` 中的 str 就像在编码时就在那一样，相当于 `var b = 3;` 入侵到 foo 的作用域中。

  ```javascript
  function foo(str, a) {
    eval(str); // cheating!
    console.log(a, b);
  }
  var b = 2;
  foo("var b = 3;", 1); // 1 3
  ```

- with

  with(obj){}相当于在 obj 对象上修改属性值。如果新增了属性，会泄漏到全局，造成全局污染。

  ```javascript
  var obj = {
    a: 1,
    b: 2,
    c: 3
  };

  // more "tedious" to repeat "obj"
  obj.a = 2;
  obj.b = 3;
  obj.c = 4;

  // "easier" short-hand
  with (obj) {
    a = 3;
    b = 4;
    c = 5;
  }
  ```

## Function vs. Block Scope

创建 Scope 的两种方式:

1. Function
2. Blocks

### Function as scope

#### 函数声明 declaration

function foo(){} 在全局定义了 foo

```javascript
var a = 2;
//该foo函数名会在global scope上定义
function foo() {
  var a = 3;
  console.log(a); // 3
}
foo();
console.log(a); // 2
```

`注意`：上述代码会在 global 上定义 foo，造成全局污染。可以使用 IIFE，避免函数名的全局污染.

#### 函数表达式 expression

(function foo(){})或(function(){}) 不会在全局定义 foo

```javascript
var a = 2;
//foo 只在()里能调用，在 global scope 中不存在
(function foo() {
  var a = 3;
  console.log(a); // 3
})(); //IIFE 立即执行函数表达式
console.log(a); // 2
```

```javascript
var foo = function() {
  // ..
};

var x = function bar() {
  // ..
};
```

比较上面两种定义函数的方式：第一种，将匿名函数指定给 foo。第二种，将 bar 函数指定给 x。但是，不能直接在之后调用 bar()，否则会报引用错误。  
在定义`回调函数表达式`时，最好将匿名函数加上函数名称，便于 debug

```javascript
setTimeout(function timeoutHandler() {
  console.log("I waited 1 second!");
}, 1000);
```

### Blocks as scope

JS 没有 Block 的概念，如：

```javascript
for (var i = 0; i < 10; i++) {
  console.log(i);
}
```

i 并不是只在{}中，它在全局的 Scope 中。为了避免无谓的全局变量定义，可以使用以下方法在 block 里定义变量：

- with
- try()catch(){}中在 catch 中定义的
- let
- const

## Hoisting 变量提升

一个 Scope 里所有的声明都会提升到 Scope 中其他代码执行之间。

注意：

1. 只有声明会提升，赋值不提升。进一步来说，对于函数，函数声明会提升，但是，函数表达式不提升。
2. 先提升函数后提升变量。

```javascript
console.log(a);
var a = 2;
```

执行结果是打印 undefined。原因是：变量和函数声明先由 compiler 读取，然后由 engine 执行代码。在这里，先将 var a = 2 拆成 var a;和 a =2;compliler 先执行 var a；然后执行 console.log( a );之后执行 a=2

## Closure

> Closure is when a function is able to remember and access its lexical scope even when that function is executing **outside** its lexical scope.

即：一个函数在他定义的 Scope 之外被调用，就是 Closure

```javascript
function foo() {
  var a = 2;

  function bar() {
    console.log(a);
  }

  return bar;
}

var baz = foo(); //bar函数被返回，赋值给baz。baz保留了对foo scope中a的也引用。

baz(); // 执行baz函数。这意味着在foo scope之外，bar被调用了。bar对foo scope的引用就是closure。closure使得bar能继续调用foo scope中的a。
```

```javascript
function wait(message) {
  setTimeout(function timer() {
    // 回调函数中meesage引用了wait函数的message参数，也是一种closure
    console.log(message);
  }, 1000);
}

wait("Hello, closure!");
```
