# 迷你redux的实现


##  redux基本慨念和API

- Store : 唯一的容器。Redux提供createStore这个函数生成Store
- State : 一个对象,通过store.getState()可以的保存的所有数据,通过store.dispatch()是发出Action的唯一方法 
- Action : 一个对象,视图变化时就是通过触发Action对象来通过状态变化的 {type: 'ADD_GUM' , pyaload: 10}
- Action Creator : 一个函数,专门用来生成Action
- Reducer : 是一个函数。接受当前的action和state作为参数,返回新的state
 

 
- store.getState()可以的保存的所有数据,通过store.dispatch()是发出Action的唯一方法
- store.dispatch会触发reducer自动执行,示意图Store需要知道Reducer函数
所以在生成store的时候,需要将reducer传入createStore
- store.subscribe()可以设置监听函数,一旦state发生变化的,就自动执行



 
## Store的一个简单实现

```
const createStore = (reducer) => {
  let state;
  let listeners = [];

  const getState = () => state;

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach(listener => listener());
  };

  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter(l => l !== listener);
    }
  };

  dispatch({});

  return { getState, dispatch, subscribe };
};

```
## Reducer是可以拆分的

reducer负责生成state,由于整个应用只有一个state对象,所以这个state会很大,也会导致reducer很大。

redux还提供了一个api来合并reducer, combineReducer

```
import {combineReducers} from 'redux';

const chatReducer = combineReducers({
    chatLog,
    statusMessage,
    userName
})

export default todoApp

```

> 下面是combineReducer的简单实现。

```
const combineReducers = reducers => {
  return (state = {}, action) => {
    return Object.keys(reducers).reduce(
      (nextState, key) => {
        nextState[key] = reducers[key](state[key], action);
        return nextState;
      },
      {} 
    );
  };
};
```
## 数据流通的流程

- 用户视图更新,执行store.dispatch(action)
- 自动调用reducer,传入当前的state和收到的action,返回新state
- state一旦变化,store就会调用监听函数,触发渲染