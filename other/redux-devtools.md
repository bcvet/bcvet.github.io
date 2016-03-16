# redux-devtools是什么?
redux-devtools是redux开源社区提供的一种redux开发者工具,可以方便的对action和store的变化状态进行查看与管理.
![redux-devtools效果](https://camo.githubusercontent.com/a0d66cf145fe35cbe5fb341494b04f277d5d85dd/687474703a2f2f692e696d6775722e636f6d2f4a34476557304d2e676966)

## 定义DevTools组件
已封装好.在项目中的位置: `/components/ReduxDevTools`

	import {createDevTools} from 'redux-devtools';

	import LogMonitor from 'redux-devtools-log-monitor';
	import DockMonitor from 'redux-devtools-dock-monitor';

	const ReduxDevTools = createDevTools(
		<DockMonitor toggleVisibilityKey="ctrl-h" changePositionKey="ctrl-q">
			<LogMonitor theme="tomorrow" />
		</DockMonitor>
	);

	export default ReduxDevTools;
	
其中,自定义的属性有:
	- `toggleVisibilityKey`,显示/隐藏devtools窗口的快捷键.
	- `changePositionKey`,改变devtools窗口位置的快捷键.
	- `theme`,devtools主题.

## 使用DevTools组件
以`person`模块为例,对devtools的配置都与`store`关联在一起.
如果想添加devtools,需要
	1. 在store中使用enhancer绑定devtools
	2. 在根组件的`<Provider>`中渲染即可...

使用compose方法在store中绑定devtools

	import {createStore, applyMiddleware, compose} from 'redux';

	const enhancer = compose(
	// Middleware you want to use in development:
	applyMiddleware(thunk),
	// Required! Enable Redux DevTools with the monitors you chose
	ReduxDevTools.instrument()
	);
	
	const store = createStore(
	lessonReducer,
	enhancer
	);


渲染ReduxDevTools:

	ReactDOM.render(
		<Provider store={store}>
			<div>
				<PersonMain />
				<ReduxDevTools />
			</div>
		</Provider>,
		document.getElementById('root')
	);

