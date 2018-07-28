# Use Redux with React

## Architecture

![Architecture](./images/architecture.png)

## Immutation

Array:

* concat 连接
* slice 截取
* ... spread

Object:

* Object.assign({},todo,{a:"frank"})
* ... spread

## combineReducers

```javascript
const todoApp = (state = {}, action) => {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  };
};
```

使用 combineReducers。其中的参数是个对象，属性表示 state 的属性，值表示对应的 reducer。

```javascript
const { combineReducers } = Redux; // CDN Redux import

const todoApp = combineReducers({
  todos: todos,
  visibilityFilter: visibilityFilter
});
```

## 例子

以下代码可在[JSBin](https://jsbin.com/vozesub/edit?js,output)中运行

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
  <script src="https://unpkg.com/expect@%3C21/umd/expect.min.js"></script>
  <script src="https://fb.me/react-15.1.0.js"></script>
  <script src="https://fb.me/react-dom-15.1.0.js"></script>
  <script src="https://cdn.bootcss.com/redux/3.7.2/redux.js"></script>
  <div id='root'></div>
</body>
</html>
```

```scss
.texttrue {
  color: #ff0000;
  text-decoration: line-through;
}
```

```javascript
//reducer
const todo = (state, action) => {
  switch (action.type) {
    case "ADD_TODO":
      return {
        text: action.text,
        id: action.id,
        completed: false
      };
    case "TOGGLE_TODO":
      return {
        ...state,
        completed: !state.completed
      };
    default:
      return state;
  }
};
const todos = (state = [], action) => {
  switch (action.type) {
    case "ADD_TODO":
      return [...state, todo(undefined, action)];
    case "TOGGLE_TODO":
      return state.map(t => {
        if (t.id === action.id) {
          return todo(t, action);
        } else {
          return t;
        }
      });
    default:
      return state;
  }
};
const visibilityFilter = (state = "SHOW_ALL", action) => {
  return state;
};
const { combineReducers } = Redux;
const todoApp = combineReducers({
  todos: todos,
  visibilityFilter: visibilityFilter
});

//store
const { createStore } = Redux;
const store = createStore(todoApp);

//react
const { Component } = React;

let nextTodoId = 0;
class TodoApp extends Component {
  render() {
    return (
      <div>
        <input
          ref={node => {
            this.input = node;
          }}
        />
        <button
          onClick={() => {
            store.dispatch({
              type: "ADD_TODO",
              text: this.input.value,
              id: nextTodoId++
            });
            this.input.value = "";
          }}
        >
          Add
        </button>
        <ul>
          {this.props.todos.map(todo => (
            <li
              className={`text${todo.completed}`}
              key={todo.id}
              onClick={() => {
                store.dispatch({
                  type: "TOGGLE_TODO",
                  id: todo.id
                });
              }}
            >
              {todo.text}
            </li>
          ))}
        </ul>
      </div>
    );
  }
}

const render = () => {
  ReactDOM.render(
    // Render the TodoApp Component to the <div> with id 'root'
    <TodoApp todos={store.getState().todos} />,
    document.getElementById("root")
  );
};

store.subscribe(render);
//store.subscribe(()=>{console.log(store.getState().todos)});
render();
```

如果 JSBin 访问不了，可访问[Github](https://github.com/juventusfc/react-redux-simple)。
