# React

## 受控组件（推荐使用） VS 非受控组件

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

## TODO

1. https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html
