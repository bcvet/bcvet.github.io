# JavaScript中的异步-事件循环与Promise

## 异步的概念
- 一般为单线程
- 并不是所有操作都可以异步

## 目前使用的异步
- Ajax
- setTimeout
- DOM渲染
- 文件IO

### Ajax
#### XMLHTTPRequest

### 异步的缺陷-Callback Hell
- A() => B() => C() 模式
- D = A() & B() & C() 模式
### Callback Hell的例子
	setTimeout(function (name) {
		var catList = name + ',';
		setTimeout(function (name) {
			catList += name + ',';
			setTimeout(function (name) {
				catList += name + ',';
				setTimeout(function (name) {
					catList += name + ',';
					setTimeout(function (name) {
						catList += name;
						console.log(catList);
					}, 1, 'Lion');
				}, 1, 'Snow Leopard');
			}, 1, 'Lynx');
		}, 1, 'Jaguar');
	}, 1, 'Panther');

## JavaScript中的事件循环
### 基础概念
	console.log(1);
	setTimeout(function() {
		console.log(2);
	}, 100);
	...
	...
	doSomething();
	...
	...
	console.log(3);
上述代码的运行结果是什么？

	setTimeout(function() {
		console.log(1);
	}, 0);
	console.log(2);

![](http://www.ruanyifeng.com/blogimg/asset/2014/bg2014100802.png)

上图中，主线程运行的时候，产生堆（heap）和栈（stack），栈中的代码调用各种外部API，它们在 **任务队列** 中加入各种事件（click，load，done）。只要栈中的代码执行完毕，主线程就会去读取 **任务队列** ，依次执行那些事件所对应的回调函数。
**执行栈中的代码(同步任务)** ，总是在读取 **任务队列(异步任务)** 之前执行。

`setTimeout(fn,0)` 的含义是，指定某个任务在主线程最早可得的空闲时间执行，也就是说，尽可能早得执行。它在 **任务队列** 的尾部添加一个事件，因此要等到同步任务和 **任务队列** 现有的事件都处理完，才会得到执行。
`setTimeout()`只是将事件插入了 **任务队列**，必须等到当前代码（执行栈）执行完，主线程才会去执行它指定的回调函数。要是当前代码耗时很长，有可能要等很久，所以并没有办法保证回调函数一定会在 `setTimeout()` 指定的时间执行。

![](https://kongchenglc.github.io/imgs/sjxh.png)

### 微观队列与宏观队列

### 参考资料
[逃离回调地狱](https://www.zcfy.cc/article/saved-from-callback-hell-1420.html)

[JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

[从一道题浅说 JavaScript 的事件循环](https://github.com/dwqs/blog/issues/61)

[理解事件循环](https://juejin.im/entry/59f9caa46fb9a045132a051b)


## Promise

### 一个简单的Promise示例
	let runPromise = () => {
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				console.log('执行完成');
				resolve('随便什么数据');
			}, 2000);
		});
	}

	runPromise().then((data) => {
		console.log(data);
	});

### Promise语法糖
静态方法 `Promise.resolve(value)` 可以认为是 `new Promise()` 方法的快捷方式.

比如`Promise.resolve(42); `可以认为是以下代码的语法糖。

	new Promise(function(resolve) {
    	resolve(42);
	});

在这段代码中的 resolve(42); 会让这个promise对象立即进入 `resolved` 状态,并将42传递给后面then里所指定的`onFulfilled`函数.

### 理解Promise的几个关键点
- Promise在创建时会立即执行一次,所以一般使用方法对Promise对象的创建进行封装
- Promise对象的创建,理解`三个函数`
	- 作为初始化参数的函数
	- resolve
	- reject
- Promise.then()做了什么?
	- then()方法接收一(两)个参数,这个参数为函数,可以理解为 `回调函数`.
	- 理解then(resolve, reject)和new Promise(resolve, reject)中回调函数的区别.
	- 先执行Promise初始化方法中定义的函数,然后使用传入的回调函数.
	- then((data) => {doSomething with data...}) 这里的data为 `Promise定义中,调用resolve时传递的参数`
- 尝试理解Promise的
	- Promise可以理解为: 对异步操作的一种定义
	- Promise对象存在状态
	- Promise对象可以链式调用

### then()
在使用 `Promise.resolve(value)` 等方法的时候，如果`Promise`对象立刻就能进入resolve状态的话,那么`then()`中的方法是不是同步调用呢?
	
	let promise = new Promise((resolve) => {
		console.log("inner promise");
		resolve(42);
	});

	promise.then((value) =>{
		console.log(value);
	});

	console.log("outer promise");

	// 下面例子的运行结果是什么?
	const promise = new Promise((resolve, reject) => {
		console.log(1);
		resolve();
		console.log(2);
	});

	promise.then(() => {
		console.log(3);
	});

	console.log(4);


### 状态
一个 Promise有以下几种状态:
- `pending`: 初始状态，既不是成功，也不是失败状态。	
- `fulfilled`: 意味着操作成功完成。在`resolve(成功)`时改变为fulfilled状态.
- `rejected`: 意味着操作失败。在`reject(失败)`时改变为rejected状态.
- `settled`: `fulfilled`或`rejected`
- `resolved` == `fulfilled`

Promise对象的状态，从`pending`转换为`fulfilled`或`rejected`之后， 这个Promise对象的状态就不会再发生任何变化。在.then 后执行的函数可以肯定地说只会被调用一次。

![](http://liubin.org/promises-book/Ch1_WhatsPromises/img/promise-onFulfilled_onRejected.png)

### 使用Promise方式实现Ajax
	myAsyncFunction(url) => {
		return new Promise((resolve, reject) => {
			const xhr = new XMLHttpRequest();
			xhr.open("GET", url);
			xhr.onload = () => resolve(xhr.responseText);
			xhr.onerror = () => reject(xhr.statusText);
			xhr.send();
		});
	};

### 链式调用
Promise对象可以链式调用.

Promise.then()的返回值为`Promise对象`,存在如下几种情况:
- 如果then中的回调函数返回一个值 => then返回的Promise将会成为`fulfilled`状态,并且将返回的值作为接受状态的回调函数的参数值.
- 如果then中的回调函数抛出一个错误 => then返回的Promise将会成为`rejected`状态,并且将抛出的错误作为`rejected`状态的回调函数的参数值.

如果返回一个值，则会以该值调用下一个`then()`.

但是，如果返回*Promise*,下一个`then()`则会等待,并仅在*Promise*产生结果（成功/失败）时调用
- 如果then中的回调函数返回一个已经是`fulfilled`状态的*Promise*,那么then返回的*Promise*也会成为`fulfilled`状态,并且将那个Promise的`fulfilled`状态的回调函数的参数值作为该被返回的Promise的接受状态回调函数的参数值.
- 如果then中的回调函数返回一个已经是`rejected`状态的*Promise*,那么then返回的*Promise*也会成为`rejected`状态,并且将那个*Promise*的`rejected`状态的回调函数的参数值作为该被返回的*Promise*的`rejected`状态回调函数的参数值.
- 如果then中的回调函数返回一个`pending`状态的Promise,那么then返回Promise的状态也是`pending`的,并且它的终态与那个Promise的终态相同.同时,它变为终态时调用的回调函数参数与那个Promise变为终态时的回调函数的参数是相同的.

### all()与race()
#### all()
`Promise.all(iterable)`方法返回一个 *Promise* 实例，此实例在参数内所有的promise都`resolved`执行resolver回调.
如果参数中`rejected`,此实例 *回调失败(rejected)*，失败原因的是第一个失败Promise的结果.

	let p1 = new Promise((resolve) => {
		setTimeout(function () {
			resolve("Hello");
		}, 3000);
	});

	var p2 = new Promise((resolve) => {
		setTimeout(() => {
			resolve("World");
		}, 1000);
	});

	Promise.all([p1, p2]).then((result) => {
		console.log(result);
	});

#### race()

	let p1 = new Promise((resolve) => {
		setTimeout(() => {
			console.log('1s');
			resolve(1);
		}, 1000);
	});

	let p10 = new Promise((resolve) => {
		setTimeout(()=>{
			console.log('10s');
			resolve(10);
		}, 10000);
	})

	let p5 = new Promise((resolve) => {
		setTimeout(()=>{
			console.log('5s');
			resolve(5);
		}, 5000);
	})

	Promise.race([p1, p10, p5]).then((res)=>{
		console.log(res);
	})

### 加入Promise后,新的事件循环队列

	setTimeout(() => {
		console.log(4);
	}, 0);

	new Promise((resolve) => {
    	console.log(1);
		for(let i = 0; i < 10000; i++) {
			i == 9999 && resolve();
		}
    	console.log(2);
	}).then(function(){
    	console.log(5);
	});

	console.log(3);

[Promise的队列与setTimeout的队列有何关联？](https://www.zhihu.com/question/36972010)

[从一道题浅说 JavaScript 的事件循环](https://github.com/dwqs/blog/issues/61)

[理解事件循环](https://juejin.im/entry/59f9caa46fb9a045132a051b)

### Promise的应用
- Q.js
- axios
- fetch

### 参考资料
[ES6 JavaScript Promise的感性认知](http://www.zhangxinxu.com/wordpress/2014/02/es6-javascript-promise-%E6%84%9F%E6%80%A7%E8%AE%A4%E7%9F%A5/)

[MDN中的Promise讲解](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

[JavaScript Promise迷你书](http://liubin.org/promises-book/#chapter1-what-is-promise)

[大白话讲解Promise](https://www.cnblogs.com/lvdabao/p/es6-promise-1.html)

[ECMAScript 6 入门-Promise](http://es6.ruanyifeng.com/#docs/promise)

[Promise.prototype.then()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)