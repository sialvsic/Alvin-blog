---
title: redux
date: 2016-12-29 13:59:12
tags:
- front
- redux
---

## Redux
首先Redux和React 没有直接相关的关系，完全是两个不同的事物
Tip: 学习Redux时，忘记React

Redux的本质是Javascript的状态容器，可以提供可预测的状态管理

### 思想/redux 动机
javasctipt单页应用开发日趋复杂，javasctipt需要管理越来越多state。state的管理已经有些不受控制，如何有效的管理state，如何合理的使用state，这些是需要解决的。

目标：通过限制更新发生的时间和方式，Redux试图让state的变化变得可预测。

你应该把要做的修改变成一个普通对象，这个对象被叫做 action，而不是直接修改 state。然后编写专门的函数来决定每个 action 如何改变应用的 state，这个函数被叫做 reducer。

如果你以前使用 Flux，那么你只需要注意一个重要的区别。Redux 没有 Dispatcher 且不支持多个 store。相反，只有一个单一的 store 和一个根级的 reduce 函数（reducer）。随着应用不断变大，你应该把根级的 reducer 拆成多个小的 reducers，分别独立地操作 state 树的不同部分，而不是添加新的 stores。这就像一个 React 应用只有一个根级的组件，这个根组件又由很多小组件构成。


## 三大原则
### 单一数据源
**整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中**

### State 是只读的
**惟一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。**

### 使用纯函数来执行修改
**为了描述 action 如何改变 state tree ，你需要编写 reducers**

Reducer 只是一些`纯函数`，它接收先前的 state 和 action，并返回新的 state。

## 数据流

**严格的单向数据流**是 Redux 架构的设计核心。

![Alt text](./redux.jpeg)	

## 一个简单的例子
流程：

```
import { createStore } from 'redux';

/**
 * 这是一个 reducer，形式为 (state, action) => state 的纯函数。
 * 描述了 action 如何把 state 转变成下一个 state。
 *
 * state 的形式取决于你，可以是基本类型、数组、对象、
 * 甚至是 Immutable.js 生成的数据结构。惟一的要点是
 * 当 state 变化时需要返回全新的对象，而不是修改传入的参数。
 *
 * 下面例子使用 `switch` 语句和字符串来做判断，但你可以写帮助类(helper)
 * 根据不同的约定（如方法映射）来判断，只要适用你的项目即可。
 */
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1;
  case 'DECREMENT':
    return state - 1;
  default:
    return state;
  }
}

// 创建 Redux store 来存放应用的状态。
// API 是 { subscribe, dispatch, getState }。
let store = createStore(counter);

// 可以手动订阅更新，也可以事件绑定到视图层。
store.subscribe(() =>
  console.log(store.getState())
);

// 改变内部 state 惟一方法是 dispatch 一个 action。
// action 可以被序列化，用日记记录和储存下来，后期还可以以回放的方式执行
store.dispatch({ type: 'INCREMENT' });
// 1
store.dispatch({ type: 'INCREMENT' });
// 2
store.dispatch({ type: 'DECREMENT' });
// 1
```


## 几个常见的概念理解

### action
action 是一个用于描述已发生事件的普通对象。
Action 是把数据从应用传到 store 的有效载荷。它是 store 数据的唯一来源。
一般来说你会通过 store.dispatch() 将 `action 传到 store`。

```
const ADD_TODO = 'ADD_TODO'

{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```

`Action 本质上是 JavaScript 普通对象`

**Action 创建函数**
在 Redux 中的 action 创建函数只是简单的返回一个 action:

```
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

### state
唯一改变store的方法就是触发action，一个描述发生什么的对象。
在 Redux 应用中，所有的 state 都被保存在一个单一对象中
应用中的所有state都以一个对象树的形式存储在一个单一的store中。

### reducers
形式为 （state，action）=> state的纯函数，接收先前的 `state tree` 和 `action`，并返回新的 state，描述了action如何更改state tree。
state 的形式多样	，可以是基本类型，数组，对象。
`当state变化时，需要全新的对象而不是修改传入的参数`。
第一次渲染时，state为undefined 的所以，需要设置default的值

本质： 纯函数

eg1：

```
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1;
  case 'DECREMENT':
    return state - 1;
  default:
    return state;
  }
}
```

eg2:
处理多个action

```
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      })
    default:
      return state
  }
}
```
如上，`不直接修改 state 中的字段，而是返回新对象。新的 todos 对象就相当于旧的 todos 在末尾加上新建的 todo。而这个新的 todo 又是基于 action 中的数据创建的`。


 **reducer 合成**
 开发一个函数来做为主 reducer，它调用多个子 reducer 分别处理 state 中的一部分数据
 

```
import { combineReducers } from 'redux';

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp;
```
注意上面的写法和下面完全等价：
```
export default function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```
combineReducers() 所做的只是生成一个函数，这个函数来调用你的一系列 reducer，每个 reducer 根据它们的 key 来筛选出 state 中的一部分数据并处理，然后这个生成的函数再将所有 reducer 的结果合并成一个大的对象

永远不要在 reducer 里做这些操作：
- 修改传入参数；
- 执行有副作用的操作，如 API 请求和路由跳转；
- 调用非纯函数，如 Date.now() 或 Math.random()。

### store
 创建 Redux store 来存放应用的状态。可以算的上是Redux 的唯一实例。
 API 是 { subscribe, dispatch, getState }

store 里能直接通过 store.dispatch() 调用 dispatch() 方法，但是多数情况下你会使用 react-redux 提供的 connect() 帮助器来调用。bindActionCreators() 可以自动把多个 action 创建函数 绑定到 dispatch() 方法上


**使用 action 来描述“发生了什么”，和使用 reducers 来根据 action 更新 state** 
Store 就是把它们联系到一起的对象。Store 有以下职责：

- 维持应用的 state；
- 提供 `getState()` 方法获取 state；
- 提供 `dispatch(action)` 方法更新 state；
- 通过 `subscribe(listener)` 注册监听器;
- 通过 `subscribe(listener)` 返回的函数注销监听器。


```
let store = createStore();
console.log(store);
//Store 就是包含几个函数的对象
{
     dispatch: [Function: dispatch],
     subscribe: [Function: subscribe],
     getState: [Function: getState],
     replaceReducer: [Function: replaceReducer]
 }
```

再次强调一下 Redux 应用只有一个单一的 store。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

根据已有的 reducer 来创建 store 是非常容易的。在前一个章节中，我们使用 combineReducers() 将多个 reducer 合并成为一个。现在我们将其导入，并传递 createStore()。

```
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
```

发起 Actions
```
// 注意 subscribe() 返回一个函数用来注销监听器
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)
// 发起一系列 action
store.dispatch(addTodo('Learn about actions'))；
// 停止监听 state 更新
unsubscribe();
```
这个是redux的基础的调用action，但实际上在项目上并不是这么应用的。

### store 和 reducer 的区别

你可能已经注意到，在简介章节中的 Flux 图表中，有 store，但没有Redux 中的 Reducer。那么，Store 与 Reducer 到底有哪些别呢？
实际的区别比你想象的简单：Store 可以保存你的 data，而 Reducer 不能。因此在传统的 Flux 中，Store 本身可以保存 state，但在 Redux 中，每次调用 reducer时，都会传入待更新的 state。这样的话，Redux 的 store 就变成了“无状态的 store” 并且改了个名字叫 Reducer。


## Redux The easy way 

## 异步action
### 同步action
当调用异步 API 时，有两个非常关键的时刻：发起请求的时刻，和接收到响应的时刻 （也可能是超时）。
这两个时刻都可能会更改应用的 state；为此，你需要 dispatch 普通的同步 action。一般情况下，每个 API 请求都需要 dispatch 至少三种 action：

- 一种通知 reducer 请求开始的 action。

> 对于这种 action，reducer 可能会切换一下 state 中的 isFetching 标记。以此来告诉 UI 来显示加载界面。

- 一种通知 reducer 请求成功的 action。

> 对于这种 action，reducer 可能会把接收到的新数据合并到 state 中，并重置 isFetching。UI 则会隐藏加载界面，并显示接收到的数据。

- 一种通知 reducer 请求失败的 action。

> 对于这种 action，reducer 可能会重置 isFetching。另外，有些 reducer 会保存这些失败信息，并在 UI 里显示出来。

为了区分这三种 action，可能在 action 里添加一个专门的 status 字段作为标记位：

```
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }
```

又或者为它们定义不同的 type：

```
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

### 处理action
```
import { combineReducers } from 'redux'
import {
  SELECT_SUBREDDIT, INVALIDATE_SUBREDDIT,
  REQUEST_POSTS, RECEIVE_POSTS
} from '../actions'

function selectedsubreddit(state = 'reactjs', action) {
  switch (action.type) {
    case SELECT_SUBREDDIT:
      return action.subreddit
    default:
      return state
  }
}

const initState = {
   isFetching: false,
   didInvalidate: false,
   items: []
}

function posts(state = initState, action) {
  switch (action.type) {
    case INVALIDATE_SUBREDDIT:
      return Object.assign({}, state, {
        didInvalidate: true
      })
    case REQUEST_POSTS:
      return Object.assign({}, state, {
        isFetching: true,
        didInvalidate: false
      })
    case RECEIVE_POSTS:
      return Object.assign({}, state, {
        isFetching: false,
        didInvalidate: false,
        items: action.posts,
        lastUpdated: action.receivedAt
      })
    default:
      return state
  }
}

function postsBySubreddit(state = {}, action) {
  switch (action.type) {
    case INVALIDATE_SUBREDDIT:
    case RECEIVE_POSTS:
    case REQUEST_POSTS:
      return Object.assign({}, state, {
        [action.subreddit]: posts(state[action.subreddit], action)
      })
    default:
      return state
  }
}

const rootReducer = combineReducers({
  postsBySubreddit,
  selectedsubreddit
})

export default rootReducer
```

### 异步 action 创建函数

如何把定义的同步的action创建函数和网络请求结合起来。
标准的做法是使用[redux-thunk](https://github.com/gaearon/redux-thunk) ,如果设置了中间件，那么

> 通过使用指定的 middleware，action 创建函数除了返回 action 对象外还可以返回函数。这时，这个 action 创建函数就成为了 [thunk](http://www.ruanyifeng.com/blog/2015/05/thunk.html)

```
import fetch from 'isomorphic-fetch'

export const REQUEST_POSTS = 'REQUEST_POSTS'
function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}

export const RECEIVE_POSTS = 'RECEIVE_POSTS'
function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}

// 来看一下我们写的第一个 thunk action 创建函数！
// 虽然内部操作不同，你可以像其它 action 创建函数 一样使用它：
// store.dispatch(fetchPosts('reactjs'))

export function fetchPosts(subreddit) {

  // Thunk middleware 知道如何处理函数。
  // 这里把 dispatch 方法通过参数的形式传给函数，
  // 以此来让它自己也能 dispatch action。

  return function (dispatch) {

    // 首次 dispatch：更新应用的 state 来通知
    // API 请求发起了。

    dispatch(requestPosts(subreddit))

    // thunk middleware 调用的函数可以有返回值，
    // 它会被当作 dispatch 方法的返回值传递。

    // 这个案例中，我们返回一个等待处理的 promise。
    // 这并不是 redux middleware 所必须的，但这对于我们而言很方便。

    return fetch(`http://www.subreddit.com/r/${subreddit}.json`)
      .then(response => response.json())
      .then(json =>

        // 可以多次 dispatch！
        // 这里，使用 API 请求结果来更新应用的 state。

        dispatch(receivePosts(subreddit, json))
      )

      // 在实际应用中，还需要
      // 捕获网络请求的异常。
  }
}
```

我们是如何在 dispatch 机制中引入 Redux Thunk middleware 的呢？我们使用了 applyMiddleware()，如下：

```
import thunkMiddleware from 'redux-thunk'
import createLogger from 'redux-logger'
import { createStore, applyMiddleware } from 'redux'
import { selectSubreddit, fetchPosts } from './actions'
import rootReducer from './reducers'

const loggerMiddleware = createLogger()

const store = createStore(
  rootReducer,
  applyMiddleware(
    thunkMiddleware, // 允许我们 dispatch() 函数
    loggerMiddleware // 一个很便捷的 middleware，用来打印 action 日志
  )
)

store.dispatch(selectSubreddit('reactjs'))
store.dispatch(fetchPosts('reactjs')).then(() =>
  console.log(store.getState())
)
```


`thunk 的一个优点是它的结果可以再次被 dispatch`


## Middleware

Middleware提供的是位于 `action 被发起之后，到达 reducer 之前`的扩展点。 你可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等

参考：http://cn.redux.js.org/docs/advanced/Middleware.html
实例：记录日志 可以使用react-logger

eg： 一个简单的Middleware

```
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}
```
将它们引用到 Redux store 中：

```
import { createStore, combineReducers, applyMiddleware } from 'redux'

// applyMiddleware 接收 createStore()
// 并返回一个包含兼容 API 的函数。
let createStoreWithMiddleware = applyMiddleware(logger, crashReporter)(createStore)

// 像使用 createStore() 一样使用它。
let todoApp = combineReducers(reducers)
let store = createStoreWithMiddleware(todoApp)
```

现在任何被发送到 store 的 action 都会经过 logger 和 crashReporter

## Fetch API
fetch 使用须知

本示例使用了 fetch API。它是替代 XMLHttpRequest 用来发送网络请求的非常新的 API。由于目前大多数浏览器原生还不支持它，建议你使用 isomorphic-fetch 库：

```
// 每次使用 `fetch` 前都这样调用一下
import fetch from 'isomorphic-fetch'
```

在底层，它在浏览器端使用 whatwg-fetch polyfill，在服务器端使用 node-fetch，所以如果当你把应用改成同构时，并不需要改变 API 请求。

注意，fetch polyfill 假设你已经使用了 Promise 的 polyfill。确保你使用 Promise polyfill 的一个最简单的办法是在所有应用代码前启用 Babel 的 ES6 polyfill：

```
// 在应用中其它任何代码执行前调用一次
import 'babel-polyfill'
```


## 问题
### 已解决
1. 什么是Thunk？
参考：http://www.ruanyifeng.com/blog/2015/05/thunk.html

2. 如何设置State 的缓存
参见代码：
```
//actions.js
import fetch from 'isomorphic-fetch'

export const REQUEST_POSTS = 'REQUEST_POSTS'
function requestPosts(subreddit) {
  return {
    type: REQUEST_POSTS,
    subreddit
  }
}

export const RECEIVE_POSTS = 'RECEIVE_POSTS'
function receivePosts(subreddit, json) {
  return {
    type: RECEIVE_POSTS,
    subreddit,
    posts: json.data.children.map(child => child.data),
    receivedAt: Date.now()
  }
}

function fetchPosts(subreddit) {
  return dispatch => {
    dispatch(requestPosts(subreddit))
    return fetch(`http://www.reddit.com/r/${subreddit}.json`)
      .then(response => response.json())
      .then(json => dispatch(receivePosts(subreddit, json)))
  }
}

function shouldFetchPosts(state, subreddit) {
  const posts = state.postsBySubreddit[subreddit]
  if (!posts) {
    return true
  } else if (posts.isFetching) {
    return false
  } else {
    return posts.didInvalidate
  }
}

export function fetchPostsIfNeeded(subreddit) {

  // 注意这个函数也接收了 getState() 方法
  // 它让你选择接下来 dispatch 什么。

  // 当缓存的值是可用时，
  // 减少网络请求很有用。

  return (dispatch, getState) => {
    if (shouldFetchPosts(getState(), subreddit)) {
      // 在 thunk 里 dispatch 另一个 thunk！
      return dispatch(fetchPosts(subreddit))
    } else {
      // 告诉调用代码不需要再等待。
      return Promise.resolve()
    }
  }
}
```

```
//views.js
store.dispatch(fetchPostsIfNeeded('reactjs')).then(() =>
  console.log(store.getState())
)
```

3. 

### 未解决
1. 如何设置缓存
2. 





