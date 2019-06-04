# Redux Guide

![architecture](./images/redux.gif)

```javascript
import { createStore } from "redux";

/**
 * This is a reducer, a pure function with (state, action) => state signature.
 * It describes how an action transforms the state into the next state.
 *
 * The shape of the state is up to you: it can be a primitive, an array, an object,
 * or even an Immutable.js data structure. The only important part is that you should
 * not mutate the state object, but return a new object if the state changes.
 *
 * In this example, we use a `switch` statement and strings, but you can use a helper that
 * follows a different convention (such as function maps) if it makes sense for your
 * project.
 */
function counter(state = 0, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
}

// Create a Redux store holding the state of your app.
// Its API is { subscribe, dispatch, getState }.
let store = createStore(counter);

// You can use subscribe() to update the UI in response to state changes.
// Normally you'd use a view binding library (e.g. React Redux) rather than subscribe() directly.
// However it can also be handy to persist the current state in the localStorage.

store.subscribe(() => console.log(store.getState()));

// The only way to mutate the internal state is to dispatch an action.
// The actions can be serialized, logged or stored and later replayed.
store.dispatch({ type: "INCREMENT" });
// 1
store.dispatch({ type: "INCREMENT" });
// 2
store.dispatch({ type: "DECREMENT" });
// 1
```

## Actions

_Action_ 是一个简单对象。当该对象被 _store.dispatch(action)_ 后，_Reducers_ 会根据这个 _Action_ 更新 _Store_ 中的 _state_。  
_Action_ 需要定义*type*属性。

```javascript
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
```

### Action Creators

顾名思义，是产生 Action 的函数。

```javascript
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  };
}

dispatch(addTodo(text));
```

## Reducers

_Reducer_ 是一个纯净函数，输入为旧的 _state_ 和 _action_，输出为新的 _state_。

```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      });
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      });
    default:
      return state;
  }
}
```

上面的代码中，多个 _action.type_ 混在一起，会造成代码混乱。实际应用中，会将多个 _reducer_ 根据 _type_ 进行拆分,然后在总的 _reducer_ 中进行组装。  
拆分：

```javascript
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    default:
      return state
  }
}
​
function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}
```

组装：

```javascript
function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  };
}
```

_Redux_ 提供了一个 _combineReducers_ 来统领所有 _reducer_ 。功能与上面代码中的 _todoApp_ 类似。

```javascript
import { combineReducers } from 'redux'
​
const todoApp = combineReducers({
  visibilityFilter,
  todos
})
​
export default todoApp
```

## Store

_Store_ 是存放应用 state 的地方。常用方法有：

1. getState() // 获取当前 state
2. dispatch(action) // 发出一个 action。 Store 自动调用 reducer 更新 state。
3. subscribe(listener) // 当 state 有更改后，会执行回调函数
4. unsubscribe listener 通过  
   `const listener1 = subscribe(listener); listener1();` 实现。

_Store_ 中的 state 类似于：

```javascript
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}
```

创建方法：

```javascript
import { createStore } from "redux";
import todoApp from "./reducers";
const store = createStore(todoApp);
```

## 数据流向

1. store.dispatch(action)发起一个 action
2. store 调用 reducer，产生新的 state
3. store 保存新的 state
4. 由于 state 有了更新，执行 subscribe 中的监听函数
