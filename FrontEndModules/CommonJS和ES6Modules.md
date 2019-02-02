# CommonJS 和 ES6 Modules

## CommmonJS

### CommonJS 规范

- 模块的导出
  > module  
  > exports
  > 模块的唯一出口是 module.exports 对象。可用伪代码表示为：

```javascript
/* 模块自带的部分 */
var exports = {};
var module = {
  exports: exports
};

/* 我们对模块的操作 */

/* 实例1 -> 正确 */
module.exports = {
  name: "doublege"
};

/* 实例2 -> 正确 */
exports.name = "doublege";

/* 实例3 -> 错误*/
exports = {
  name: "doublege"
};
/* 操作完毕 */

/* 模块自带的部分 */
return module.exports;
```

- 模块的引用
  > require

### 原理

```javascript
// CommonJS模块
let { stat, exists, readFile } = require("fs");

// 等同于
let _fs = require("fs");
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

上面代码的实质是`整体加载` fs 模块（即加载 fs 的所有方法），生成一个对象（\_fs），然后再从这个对象上面读取 3 个方法。

### 例子

```javascript
// index.js
function isNumber(n) {
  return typeof n === "number";
}
// 导出一整个对象
module.exports = {
  sum: function(a, b) {
    if (isNumber(a) && isNumber(b)) {
      return a + b;
    } else {
      return NaN;
    }
  }
};
// 引用一整个对象
var mod = require("./index");
console.log(mod.sum(2, "2")); // NaN
console.log(mod.sum(2, 2)); // 4
mod.isNumber(); // 抛出错误
```

## ES6 Modules

### 规范

- 导出
  > export: 命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系
- 引用
  > import

### 例子

#### 分别 exprot

```javascript
// circle.js
// 导出方式1
export const PI = 3.14;
export let radius = 5;
export let getArea = r => PI * r * r;
// 导出方式2(推荐)
const PI = 3.14;
let radius = 5;
let getArea = r => PI * r * r;
export { PI, radius, getArea };

// 引用
// 1. 分别引用(推荐)
import { PI, radius, getArea } from "./circle";
console.log(PI); // 3.14
// 2. 整体引用
import * as circle from "./circle";
console.log(circle); // { PI: 3.14, radius: 5, getArea: [Function: getArea] }
```

#### exprot default

export default 命令用于指定模块的默认输出。

```javascript
// circle.js
const PI = 3;
let getArea = r => PI * r * r;
export default getArea;

// 1. 整体引用
import * as circle from "./circle";

console.log(circle); // { default: [Function: getArea] }
console.log(circle.default(3)); // 27
// 2. 单独引用(推荐)
import getArea from "./circle";
console.log(getArea); // [Function: getArea]
console.log(getArea(3)); // 27
```

## 两者比较

CommonJS 是在`运行时`才确定引入, 然后执行这个模块, 相当于是调用一个函数, 返回一个对象, 就这么简单。当 a 文件 require 了 b 文件时，相当于复制了一份 b 文件，b 文件的改变不会影响 a 文件引入的值。  
ES6 Module 是语言层面的, 导入导出是声明式的代码集合,`编译时`就能确定模块的依赖关系。当 a 文件 import 了 b 文件时，相当于引用了一份 b 文件，b 文件的改变会影响 a 文件引入的值。
