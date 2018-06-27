# React + Redux + React Router

前面介绍了 React 与 Redux 相结合的使用方式，解决了 UI 与 store 的同步。  
现实生活中，SPA 项目经常使用 React Router 进行 UI 与浏览器地址的同步。基本的 React Router 可参考[React Router v4 Tutorial](../../React/react-router.md)

## 包装 Router

### 将 store 存入 Root 中

```javascript
render(<Root store={store} />, document.getElementById("root"));
```

### 新建 Root.js

```javascript
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/:filter?" component={App} />
    </Router>
  </Provider>
);
```

## 指定跳转发生地 Link

FilterLink 中点击，然后浏览器跳转到目标 url。

```javascript
const FilterLink = ({ filter, children }) => (
  <NavLink
    to={filter === "SHOW_ALL" ? "/" : `/${filter}`}
    activeStyle={{
      textDecoration: "none",
      color: "black"
    }}
  >
    {children}
  </NavLink>
);
```

## 根据跳转目的地，渲染页面

### App.js 中传入 path

App 中的`match.params`等价于上面新建的 Root.js 中的 path 里的内容，`{ filter: 'SHOW_COMPLETED' }`

```javascript
const App = ({ match: { params } }) => {
  return (
    <div>
      <AddTodo />
      <VisibleTodoList filter={params.filter || "SHOW_ALL"} />
      <Footer />
    </div>
  );
};
```

### 更改 VisibleTodoList.js

ownProps 表示传入 VisibleTodoList 这个容器组件的 props，在这里就是 filter 属性。

```javascript
const mapStateToProps = (state, ownProps) => {
  return {
    todos: getVisibleTodos(state.todos, ownProps.filter) // previously was getVisibleTodos(state.todos, state.visibilityFilter)
  };
};
```
