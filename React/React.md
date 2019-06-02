# React

## 出现背景及特性

传统的前端开发中，使用 jQuery 进行 UI 的开发。但是，传统的前端开发面临了两个大问题：

1. UI 操作关注了大量细节。jQuery 的 API 众多，直接对 DOM 进行操作，很容易让开发者陷入细节而混淆了总体设计。
2. 应用程序的状态分散的各处，无法追踪和维护。

Facebook 为了解决了两个问题，开发出了 React。React 其实就是一个 View 层，引入了：

1. 1 个新概念： 组件
2. 4 个主要 API:
3. 单向数据流: 引入 Flux 设计方式，简化状态管理
4. 完善的错误提示

## 组件方式构建 UI

组件主要由 `props` 和 `state` 组成 `view`，可以理解为一个纯函数。`props` 是由上层组件传递给下层组件的，下层组件不能修改上层组件传给它的`props`，这叫做组件间的单向数据流(注意，Flux 单向数据流指的是整个 React 应用的数据流)。

组件设计时，遵循的原则有：

1. 让组件无自身 `state`，所需数据从 `props` 获取
2. DRY 原则
3. 单一职责原则

### 受控组件（推荐使用） VS 非受控组件

form 表单相关的元素比较特殊，在 React 中由两种设计思路：

1. 受控组件  
   受控组件的表单元素由使用者维护。

   ```javascript
   <input
     type="text"
     value={this.state.value}
     onChange={e => this.setState({ value: e.target.value })}
   />
   ```

2. 非受控组件  
   非受控组件的表单元素由 DOM 维护。

   ```javascript
   <input type="text" ref={node => (this.input = node)} />
   ```

```javascript
// 受控组件通过setState来控制组件状态
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { value: "" };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({ value: event.target.value });
  }

  handleSubmit(event) {
    alert("A name was submitted: " + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input
            type="text"
            value={this.state.value}
            onChange={this.handleChange}
          />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

// 非受控组件由DOM处理组件状态
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(event) {
    alert("A name was submitted: " + this.input.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={input => (this.input = input)} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

## JSX

JSX 不是模板语言，而是一种语法糖,能通过 javascript 改变 DOM。

```javascript
const element = <h1>Hello, {name}</h1>;

// 等价于
const element = React.createElement("h1", null, "Hello ", name);
```

## 生命周期

![life-cycle](./images/react-lifecycle.PNG)

## HOC

HOC 是包裹另一个 React 组件的 React 组件。[在线 DEMO](https://codesandbox.io/s/84296x60y0)

```javascript
hocFactory:: WrappedComponent: React.Component => EnhancedComponent: React.Component
```

### Props Proxy

整合 WrappedComponent 需要的 props。

```javascript
function ppHOC(WrappedComponent) {
  return class PP extends React.Component {
    render() {
      return <WrappedComponent {...this.props} />; // 这里的this指向PP实例
    }
  };
}
```

如果需要访问 WrappedComponent 实例，需要使用 refs

### Inheritance Inversion

HOC 类（Enhancer）继承了 WrappedComponent。可以通过调用`super.xxMethod()`来获得 WrappedComponent 的信息。

```javascript
function iiHOC(WrappedComponent) {
  return class Enhancer extends WrappedComponent {
    render() {
      return super.render();
    }
  };
}
```

## 函数作为子组件

```javascript
// 创建
class MyComponent extends React.Component {
  render() {
    return <div>{this.props.children("Scuba Steve")}</div>;
  }
}
MyComponent.propTypes = {
  children: React.PropTypes.func.isRequired
};

// 函数作为自组件使用
<MyComponent>{name => <div>{name}</div>}</MyComponent>;
```

## Context API

```javascript
import React from "react";

const enStrings = {
  submit: "Submit",
  cancel: "Cancel"
};

const cnStrings = {
  submit: "提交",
  cancel: "取消"
};
const LocaleContext = React.createContext(enStrings); // 1. 定义Context

class LocaleProvider extends React.Component {
  state = { locale: cnStrings };
  toggleLocale = () => {
    const locale = this.state.locale === enStrings ? cnStrings : enStrings;
    this.setState({ locale });
  };
  render() {
    return (
      // 2. 提供Context值
      <LocaleContext.Provider value={this.state.locale}>
        <button onClick={this.toggleLocale}>切换语言</button>
        {this.props.children}
      </LocaleContext.Provider>
    );
  }
}

class LocaledButtons extends React.Component {
  render() {
    return (
      // 3. 消费Context
      <LocaleContext.Consumer>
        {locale => (
          <div>
            <button>{locale.cancel}</button>
            &nbsp;
            <button>{locale.submit}</button>
          </div>
        )}
      </LocaleContext.Consumer>
    );
  }
}

export default () => (
  <div>
    <LocaleProvider>
      <div>
        <br />
        <LocaledButtons />
      </div>
    </LocaleProvider>
    <LocaledButtons />
  </div>
);
```

## 脚手架

- Create-React-App:Facebook 自己出的，是学习 React 比较好用的 CLI，但是没有集成 Redux/React-Router
- Codesandbox：在线的一款快速构建 React 应用
- Rekit：一款在 CRA 基础上增加了 Redux 等功能的 CLI
- React-Starter-kit：比较老的一款脚手架

## 打包和部署

使用 Webpack

## Redux

Redux 相当于给应用中的所有 React Component 增加了一个全局的控制机制，用户通过这个机制控制 Redux Store State，所有 React Component 根据 Redux Store State 来组成虚拟 DOM，用以生成 DOM

### combineReducers

生成 Store 时，需要给定 Reducer。当有多个 Reducer 时，使用 combineReducers 组成一个包括所有 Reducers 的 Reducer。初始化生成的 Store State 包含所有 Reducers 中定义的 initial state。

### bindActionCreators

ActionCreator 用以生成 Action，Action 生成后还需要 dispatch 出去才能使 Redux 更新 State。bindActionCreators 能简化这个步骤，当执行 bind 后的方法，会直接 dispatch 出去 Action

## // TODO

1. https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html
