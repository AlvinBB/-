#React

## 简介

### 特点
- React不是完整的MVC,MVVM框架,只负责view层
- virtual dom使得React很轻
- 组件化,高度可重用

### 适用场景
- 数据更新频繁,性能要求高
- 重用组件库,组件组合使用

## 生命周期

### Mounted

组件被render生成对应的DOM节点，并插入浏览器DOM结构中的过程

- getInitialState
- componentWillMount
- render
- componentDidMount


### Update

状态发生变化时的过程

- componentWillReceiveProps
- shouldComponentUpdate
- componentWillUpdate
- render
- componentDidUpdate

### Unmounted
一个已mounted的组件对应的DOM节点被移除的过程
- componentWillUnmount
