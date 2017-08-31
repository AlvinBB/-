# React坑汇总
### 关于回调函数
- 在react节点上绑定事件回调时，回调靠前的参数是bind所在dom节点的参数，而后才是event参数
```javascript
handleClick(value, event){
	event.stopPropagation();
	console.log(value);			//1
}
render(){
	return (
		<div onClick={this.handleClick.bind(this, 1)} />
	)
}
```
- 子组件调用父组件回调，参数是靠后的
```javascript
//父组件
handleFunc(value, childValue){
	event.stopPropagation();
	console.log(value, childValue);			//1,2
}
render(){
	return (
		<div callback={this.handleFunc.bind(this, 1)} />
	)
}

//子组件
handleClick(childValue){
	this.props.callback(childValue)
}
render(){
	return (
		<div onClick={this.handleClick.bind(this, 2)} />
	)
}
```