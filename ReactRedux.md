# Redux 笔记

> [Redux 中文文档入口](http://www.redux.org.cn/)

##  概述

- Redux 是 JavaScript 状态容器，提供可预测化的状态管理。
- 可以让你构建一致化的应用，运行于不同的环境（客户端、服务器、原生应用），并且易于测试。
- Redux 除了和 React 一起用外，还支持其它界面库。
- 它体小精悍（只有2kB）且没有任何依赖。

### 不需要使用redux的情况

- 用户的使用方式非常简单
- 用户之间没有协作
- 不需要与服务器大量交互，也没有使用 WebSocket
- 视图层（View）只从单一来源获取数据

### 需要使用redux的情况

- 用户的使用方式复杂
- 不同身份的用户有不同的使用方式（比如普通用户和管理员）
- 多个用户之间可以协作
- 与服务器大量交互，或者使用了WebSocket
- View要从多个来源获取数据

## 要点

- 应用中所有的 state 都以一个对象树的形式储存在一个单一的 store 中。
- 惟一改变 state 的办法是触发 action，一个描述发生什么的对象。
- 为了描述 action 如何改变 state 树，你需要编写 reducers。

````javascript

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

````

## 三大原则

### 单一数据源

> 整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

### State 是只读的

> 惟一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。

### 使用纯函数来执行修改

> 为了描述 action 如何改变 state tree ，你需要编写 reducers。

````javascript

function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}

import { combineReducers, createStore } from 'redux'
let reducer = combineReducers({ visibilityFilter, todos })
let store = createStore(reducer)

````

## Redux 基础

> [Redux 实例入口](https://github.com/reactjs/redux/tree/master/examples)

### Action

#### Action 对象

Action 是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的唯一来源。一般来说你会通过 store.dispatch() 将 action 传到 store。

因为数据是存放在数组中的，所以我们通过下标 index 来引用特定的任务。而实际项目中一般会在新建数据的时候生成唯一的 ID 作为数据的引用标识。我们应该尽量减少在 action 中传递的数据。

````javascript

{
  type: TOGGLE_TODO,
  index: 5
}

````

#### Action 创建函数

在 Redux 中的 action 创建函数只是简单的返回一个 action:

````javascript
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
````

Redux 中只需把 action 创建函数的结果传给 dispatch() 方法即可发起一次 dispatch 过程。

````javascript
//直接使用
dispatch(addTodo(text))
dispatch(completeTodo(index))

//通过创建函数调用
const boundAddTodo = (text) => dispatch(addTodo(text))
const boundCompleteTodo = (index) => dispatch(completeTodo(index))
````

store 里能直接通过 store.dispatch() 调用 dispatch() 方法，但是多数情况下你会使用 react-redux 提供的 connect() 帮助器来调用。bindActionCreators() 可以自动把多个 action 创建函数 绑定到 dispatch() 方法上。

### Reducer

Action 只是描述了有事情发生了这一事实，并没有指明应用如何更新 state。而这正是 reducer 要做的事情。 


````javascript

import { VisibilityFilters } from './actions'

const initialState = {
  visibilityFilter: VisibilityFilters.SHOW_ALL,
  todos: []
};

function todoApp(state, action) {
  if (typeof state === 'undefined') {
    return initialState
  }

  // 这里暂不处理任何 action，
  // 仅返回传入的 state。
  return state
}

````


### Store

Store 就是把它们联系到一起的对象。Store 有以下职责：

- 维持应用的 state；
- 提供 getState() 方法获取 state；
- 提供 dispatch(action) 方法更新 state；
- 通过 subscribe(listener) 注册监听器;
- 通过 subscribe(listener) 返回的函数注销监听器。

再次强调一下 Redux 应用只有一个单一的 store。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

根据已有的 reducer 来创建 store 

````javascript

import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)

````

### createStore 原理

````javascript

const createStore = (reducer) => {
    let state;
    let listeners = [];
    
    const getState = () => { 
        return state;
    }
    
    const dispatch = (action) => {
        state = reducer(state, action);
        listeners.forEach(listener => listener());
    }
    
    const subscribe = (listener) => {
        listener.push(listener);
        return () => {
            listeners = listeners.filter(l => l !=listener);
        }
    };
    
    dispatch({});
    
    return { getState, dispatch, subscribe };
};

````

### 生命周期

> State至View层，通过store.subscribe(listener)来推入回调函数

