### 先说一点React的知识补充
[context](https://github.com/facebook/react/blob/master/docs/docs/12-context.zh-CN.md)

### 为什么使用FLUX
`FLUX`解决了两个问题：
1. 数据流混乱的问题，强制使用单向数据流
2. 当组件树结构复杂时，多组件通信不方便的问题

[React为什么需要Flux-like的库](https://segmentfault.com/a/1190000003982553)
[Facebook：MVC不适合大规模应用，改用Flux](http://www.infoq.com/cn/news/2014/05/facebook-mvc-flux)
### 什么是FLUX
[如何理解 Facebook 的 flux 应用架构？](https://www.zhihu.com/question/33864532)
### 一个FLUX的例子
[Flux 架构入门教程](http://www.ruanyifeng.com/blog/2016/01/flux.html)

## Redux
### Action
`Action`是对用户行为操作的描述。一个`Action`定义了一种用户可能的行为。

`Action`在`redux`中就是基本的JavaScript对象。

如：

	{type:'add_todo', text:'读书'}
	{type:'remove_todo'}
	{type:'change_todo', text:'睡觉', time:'晚上', 'name': '金同尧'}
	
[Action](http://camsong.github.io/redux-in-chinese/docs/basics/Actions.html)

### Reducer
`Action`只是描述了有事情发生了这一事实，并没有指明应用如何更新数据。`Reducer`正是操作数据的函数。

	// reducer方法, 传入的参数有两个
	// state: 当前的state
	// action: 当前触发的行为, {type: 'xx'}
	// 返回值: 新的state
	var reducer = function(state, action){
	    switch (action.type) {
	        case 'add_todo':
	        		var newState = copyState();
	        		newState = state + action.text；
	            return newState;
	        default:
	            return state;
	    }
	};
	
注意，	`Reducer`应该为纯函数。即：`Reducer`并不操作原有的state。
[Reducer](http://camsong.github.io/redux-in-chinese/docs/basics/Reducers.html)

### Store
`Store`代表数据存储。存储**全局唯一**的数据。

`Store`有三个核心方法，分别是`getState`、`dispatch`和`subscribe`。
- `getState`用来获取store中的数据（state）
- `dispatch`后者用来修改store的数据（通过调用Action）
- `subscribe`注册数据变化后的回调方法

	// 创建store, 传入两个参数
	// 参数1: reducer用来修改state
	// 参数2(可选): 默认的state值,如果不传, 则为undefined
	var store = redux.createStore(reducer, []);
	
	// 通过 store.getState() 可以获取当前store的状态(state)
	// 默认的值是 createStore 传入的第二个参数
	console.log('state is: ' + store.getState());
	
	// 通过 store.dispatch(action) 来达到修改 state 的目的
	// 注意: 在redux里,唯一能够修改state的方法,就是通过	store.dispatch(action)
	store.dispatch({type: 'add_todo', text: '读书'});
	
[Store](http://camsong.github.io/redux-in-chinese/docs/basics/Store.html)
### 三者之间的关系
store.dispatch(action) --> reducer(state, action) --> final state

### Redux的两个例子
[redux入门实例-TodoList](https://segmentfault.com/a/1190000004177660)

### 推荐阅读
没看过这篇文章的同学一定要认真认真认真的读一下！！！
[已买到的宝贝前端组件化探索](http://taobaofed.org/blog/2015/11/02/buy-component/)

### 问题
store数据变化时，所有回调事件都触发的问题。

