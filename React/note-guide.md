# React 官网学习笔记

## JSX

## state

## props

## componentDidMount()/componentWillUnmount etc. lifecycle

## handing events + `what is e when click a <a> tag? Both in React and DOM`

React 有自己包装好的事件对象。

注意区分 this 在 React 中的使用(1. 在构造器里 bind 2. 在类里面使用箭头表达式(class fields syntax) 3. 在 callback 里使用箭头表达式)。

```javascript
class LoggingButton extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    console.log("this is:", this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return <button onClick={this.handleClick}>Click me</button>;
  }
}
```

```javascript
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log("this is:", this);
  };

  render() {
    return <button onClick={this.handleClick}>Click me</button>;
  }
}
```

```javascript
class LoggingButton extends React.Component {
  handleClick() {
    console.log("this is:", this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return <button onClick={e => this.handleClick(e)}>Click me</button>;
  }
}
```

## Conditional Rendering

## Lists and Keys

## Forms

controlled components: 在 HTML 中 `<input>` `<textarea` `select`可以自己保持状态；在 React 中，组件使用 state 保持状态。如果上述的 tag 也用 state 保持状态，则可以认为这些 tag 对应的 component 是 controlled component

uncontrolled components

[formilk](https://jaredpalmer.com/formik/docs/overview)

## Lifting Sate Up

## class component 和 function component

1. class component 使用 class 定义，可以使用 state

   ```javascript
   class Board extends React.Component {
     renderSquare(i) {
       return (
         <Square
           value={this.props.squares[i]}
           onClick={() => this.props.onClick(i)}
         />
       );
     }

     render() {
       return (
         <div>
           <div className="board-row">
             {this.renderSquare(0)}
             {this.renderSquare(1)}
             {this.renderSquare(2)}
           </div>
           <div className="board-row">
             {this.renderSquare(3)}
             {this.renderSquare(4)}
             {this.renderSquare(5)}
           </div>
           <div className="board-row">
             {this.renderSquare(6)}
             {this.renderSquare(7)}
             {this.renderSquare(8)}
           </div>
         </div>
       );
     }
   }
   ```

2. function component 使用 function 定义，只有 render 方法(直接 return 了)，没有自己的 state

   ```javascript
   function Square(props) {
     return (
       <button className="square" onClick={props.onClick}>
         {props.value}
       </button>
     );
   }
   ```

## 如何使用 React

### 在 HTML 中直接引用

引入脚本：

1. react
2. react-dom
3. babel

### 在 app 中使用

用脚手架工具生成 app，然后使用 React 技术。

一般在写 Component 的时候，会使用 JSX 和 ES6 语法，然后使用 Babel 将这些代码转化为浏览器可识别的代码，之后用 Webpack 打包。

这也是 JSX 方式写 Component 一定要引用 react 模块的原因，因为 Babel 将 JSX 转化为 ES5 代码后，转化后代码中会有 React.createElement 等方法。

```javascript
// JSX 写法
const element = <h1 className="greeting">Hello, world!</h1>;

// 转化为 ES5
var element = React.createElement(
  "h1",
  { className: "greeting" },
  "Hello, world!"
);

// 运行后产生的 element 对象
const element = {
  type: "h1",
  props: {
    className: "greeting",
    children: "Hello, world!"
  }
};
```

## 工具库

- 初始化
  - Yeoman
  - 脚手架
    - create-react-app: 前端渲染，前端路由
    - Next.js: 后端渲染，后端路由
    - Gatsby: 在 CI 中渲染
    - Razzle: 类似于 Next.js
- 开发
  - 包管理
    - npm
  - 编译
    - Babel
  - 打包
    - Webpack
    - Neutrino
    - Parcel
- CI
- 测试
  - ava
  - nyc
- 发布
  - aws-cli
  - nwb

## 选型

- **Boilerplate**: create-react-app (with eject if required)
- Utility: JavaScript ES6 + Lodash
- **Styling**: CSS modules
- Requests: axios + fetch
- Higher Order Components: maybe + optional recompose
- **Formatting**: Prettier
- Type Checking: PropTypes
- **State Management**: Redux + (Redux Thunk/Saga)
- **Routing**: React Router
- Authentication: Solution with an own Express/Hapi/Koa Node.js Server with Passport.js
- Database: Solution with an own Express/Hapi/Koa Node.js Server with a SQL or NoSQL Database
- **UI Components**: Semantic UI
- Time: moment or date-fns
- **Testing**: Jest with Enzyme
- **Bundle**: Babel + Webpack
- **Deploy**
- HMR
