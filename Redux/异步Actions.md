# 异步 Actions

## 同步 VS 异步

在前面的章节中，我们直接使用了同步 actions，但在实际应用场景中，我们用到的还有异步 actions。比如：

1 发起一个 API 请求，在发起之后，页面要显示一个 loading 标志，表示正在发起请求。  
2a 请求成功，页面显示正确的数据。  
2b 请求失败，页面显示错误信息。

那么，这种异步 actions 要怎么写呢？

### 同步 actions creator

返回一个对象。

```javascript
export const selectSubreddit = subreddit => ({
  type: SELECT_SUBREDDIT,
  subreddit
});
```

调用方法：

```javascript
store.dispatch(selectSubreddit("reactjs"));
```

### 异步 actions creator

返回一个函数。

```javascript
const fetchPosts = subreddit => dispatch => {
  dispatch(requestPosts(subreddit));
  return fetch(`https://www.reddit.com/r/${subreddit}.json`)
    .then(response => response.json())
    .then(json => dispatch(receivePosts(subreddit, json)));
};

const shouldFetchPosts = (state, subreddit) => {
  const posts = state.postsBySubreddit[subreddit];
  if (!posts) {
    return true;
  }
  if (posts.isFetching) {
    return false;
  }
  return posts.didInvalidate;
};

export const fetchPostsIfNeeded = subreddit => (dispatch, getState) => {
  if (shouldFetchPosts(getState(), subreddit)) {
    return dispatch(fetchPosts(subreddit));
  }
};
```

调用方法：与调用同步 action creator 一致

```javascript
dispatch(fetchPostsIfNeeded(selectedSubreddit));
```

## reducer

reducer 没有任何改变。

## store

引入 *redux-thunk*作为 middleware。同步/异步 action creator 的调用方式一致的原因就在这个中间件上。

```javascript
import React from "react";
import { render } from "react-dom";
import { createStore, applyMiddleware } from "redux";
import { Provider } from "react-redux";
import thunk from "redux-thunk";
import { createLogger } from "redux-logger";
import reducer from "./reducers";
import App from "./containers/App";

const middleware = [thunk];
if (process.env.NODE_ENV !== "production") {
  middleware.push(createLogger());
}

const store = createStore(reducer, applyMiddleware(...middleware));

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

## react component

react component 根据 store 里的 state 进行渲染。

## 总结

针对异步 action，只需要对 action creator 和 store 的建立方法进行修改。不需要修改 reducer/react component。  
[点击此处获得源码](https://github.com/reactjs/redux/tree/master/examples/async)
