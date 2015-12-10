## 什么是前后台分离？什么是模块化？
`前后台分离` 就是**视图**与数据分离，**视图**与逻辑分离。
`模块化` 就是解耦合，将一个大的模块拆分为多个小的模块，使各个模块间的内容相互独立，功能单一化，复用性增强。

---- 

## 基础概念
### 模型(Model)
- 模型具备属性。模型一般支持属性验证功能。
- 模型需要持久化。
- 可能存在多个视图在同时观察一个模型的变化，通过观察，每当模型更新时，视图可获得通知，使得视图能够确保屏幕上显示的内容与模型同步。
### 视图(View)
- 视图与用户进行交互。
- 视图既可以通过事件接收用户命令，也可以渲染模型。视图为用户操作与模型的中介。
### 模板(Template)
- 模板为包含模板变量的HTML标记。
- 一个视图可能由多个模板进行渲染。
### 容器(Collection)

---- 
## Backbone中的MVC

### Model
#### 定义
`Model`为模型，包含应用程序中的数据与相关逻辑。

```javascript
var Man = Backbone.Model.extend({
	
});
```

#### 初始化
初始化方法会在模型被创建时被调用。相当于构造方法。

```javascript
var Man = Backbone.Model.extend({
        // 初始化方法
        initialize : function() {
            // 在这里进行事件绑定
        this.bind("change:name", function() {
            var name = this.get("name");
            alert("你改变了name属性为: " + name);
            });
        },
});
```

#### 默认值
有时页面在获取后台传递的数据前，页面可能需要展示一些默认数据，可以通过**defaults**属性设置。
```javascript
var Man = Backbone.Model.extend({
        // 初始化方法
        defaults : {
        name : 'Eric',
            age : 28,
        sex : 'male'
    },
});
```

#### get/set

```javascript
model.get('name');
model.set('name', 'jintongyao');
```

调用某个模型的数据对象使用 `model.toJSON()`方法。返回的是一个包含所有实体属性的**对象**。

`Model` 暴露了一个 `attributes` 属性，包含了所有实体属性。如果直接修改`attributes`，不会触发模型事件触发器。也可以通过 `{silent: true}` 属性达到相同效果。
	var person = new Backbone.Model();
	person.set({name: 'jintongyao'}, {silent: true});

#### Model的事件监听
当Model的属性发生变化时，可以通过事件进行监听，从而触发相应的功能。这是面向对象式编程的方便之处。一般来说，可以将监听事件绑定在Model的初始化方法中。

```javascript
initialize : function() {
    this.on('change', function() {
        var name = this.get("name");
        console.log('value has been changed');
    });
},
```

#### 验证\* Backbone使用 `Model.validate()` 进行模型验证，允许在设置属性之前对属性值进行检查。默认情况下，调用 `save()` 方法或者带有 `{validate: true}`参数的方法持久化模型时，验证会被触发。

如果有返回值，则会触发 `invalid` 方法。

### View
#### 定义
`View`为视图，用于对 `Model` 中的信息进行展示，为最终与用户进行交互的内容。现代的视图一般使用Javascript模板来完成这一功能，Backbone中也是如此，但包含了更多强大的特性。
	var View = Backbone.View.extend({
	    tagName: 'div',// 标签
	    el: 'id',// 对应的dom
	    model: 'user'// 使用的Model
	});
#### el
`el` 是 `View ` 的核心。本质上来说，`el` 是对DOM元素的引用。视图可以将需要渲染的内容分多次添加至 `el` 中，再一次性加载至DOM中，使页面渲染更快。
有两种 `el` 的使用方式：
1. 创建一个新的，并添加至DOM
	2. 使用DOM中已有的一个元素

如果 ** 创建一个新的，并添加至DOM**，可以在视图上对 `tagName`， `id` 和 `className` 三个属性进行任意组合。
	var UserView = Backbone.View.extend({
	    tagName: 'ul',
	    id: 'newUser',
	    className: 'user'
	});
	var userView = new UserView();
	console.log(userView.el);
上面的代码已经创建了一个DOM元素，但是他**还没有添加至页面视图中**。

如果 **使用DOM中已有的一个元素**，可以为 `el` 设定一个属性选择器来匹配元素。
	el: '#user'
	var userView = new UserView({el: $('#user')});
	// 注意上述两者的区别

#### $()
- `$(selector)` 等价于 `$(view.el).find(selector)`

#### render()
`render()` 是 `View` 中的核心方法，用于将数据渲染到视图中。`render()` 方法自行实现。
> 一般来说，在Backbone中约定，在render方法的最后返回this。这样的好处有：
> 1. 使得该视图可以在其他父视图中重用。
> 2. 子视图的render方法并不渲染/绘制元素，只是作为填充数据作用。

#### View的事件
在 `View`中，使用 `events` 属性来为el定义监听器。采用 `'eventName selector' : 'callbackFunction'` 格式。如：

```javascript
events: {
        'click .toggle': 'toggleComplete',
        'dbclick label': 'dbClickComplete',
        'onblur #focus': 'focusBlur'
}
```

### Collection
#### 定义
`Collection` 是 `Model` 的集合。

```javascript
var UserList = Backbone.Collection.extend({
    model: User
});
```

#### add/remove

```javascript
var userList = new UserLsit();
userList.add(user1);
userList.add(user2);
userList.remove(user2);
```

#### 检索Model
根据id检索，直接调用 `get()` 方法即为按id检索。
> id & cid。id为后台数据标识的唯一属性。但数据很可能先形成前台对象，再存入到数据库中，此时没有id。Backbone会自动为每个model生成cid属性。

#### Collection的事件监听
可以监听集合的变化。
- `change` 变化
	- `add` 新增
	- `remove` 删除
与`model` 一样，使用 `on` 进行绑定。

```javascript
this.on('add', function(user) {
    console.log('added:' + user.get('name'));
});
this.on('change', function(user) {
    console.log('changed:' + user.get('name'));
});
```

> `Model` 与 `Collection` 不能使用 `events` 属性。

### Router
`Router` 是Backbone中封装的一种特殊功能，专门为单页面应用所设计。目的为使单页面应用也能够支持类似多页面应用中的**历史记录**与**前进/后退**等功能。

### 与后台交互
Backbone默认实现了一套与RESTful风格的服务端同步模型的机制，这套机制不仅可以减轻开发人员的工作量，而且可以使模型变得更为健壮（在各种异常下仍能保持数据一致性）。不过，要真正发挥这个功效，一个**与之匹配的服务端实现**是很重要的。为了说明问题，假设服务端有如下REST风格的接口：
> - GET /resources 获取资源列表
> - POST /resources 创建一个资源，返回资源的全部或部分字段
> - GET /resources/{id} 获取某个id的资源详情，返回资源的全部或部分字段
> - DELETE /resources/{id} 删除某个资源
> - PUT /resources/{id} 更新某个资源的全部字段，返回资源的全部或部分字段
> - PATCH /resources/{id} 更新某个资源的部分字段，返回资源的全部或部分字段

backbone会使用到上面这些HTTP方法的地方主要有以下几个：
> - Model.save() 逻辑上，根据当前这个model的是否具有id来判断应该使用POST还是PUT，如果model没有id，表示是新的模型，将使用POST，将模型的字段全部提交到/resources；如果model具有id，表示是已经存在的模型，将使用PUT，将模型的全部字段提交到/resources/{id}。当传入options包含patch:true的时候，save会产生PATCH。
> - Model.destroy() 会产生DELETE，目标url为/resources/{id}，如果当前model不包含id时，不会与服务端同步，因为此时backbone认为model在服务端尚不存在，不需要删除
> - Model.fetch() 会产生GET，目标url为/resources/{id}，并将获得的属性更新model。
> - `Collection.fetch()` 会产生GET，目标url为/resources，并对返回的数组中的每个对象，自动实例化model
> - `Collection.create()` 实际将调用Model.save

options参数存在于上面任何一个方法的参数列表中，通过options可以修改backbone和ajax请求的一些行为，可以使用的options包括：
> - `wait`: 可以指定是否等待服务端的返回结果再更新model。默认情况下不等待
> - `url`: 可以覆盖掉backbone默认使用的url格式
> - `attrs`: 可以指定保存到服务端的字段有哪些，配合options.patch可以产生PATCH对模型进行部分更新
> - `patch`: 指定使用部分更新的REST接口
> - `data`: 会被直接传递给jquery的ajax中的data，能够覆盖backbone所有的对上传的数据控制的行为
> - `其他`: options中的任何参数都将直接传递给jquery的ajax，作为其options

backbone通过 `Model`的 `urlRoot` 属性或者是`Collection`的 `url` 属性得知具体的服务端接口地址，以便发起ajax。在 `Model` 的 `url` 默认实现中，Model除了会考察 `urlRoot` ，第二选择会是 `Model` 所在 `Collection` 的`url`，有时只需要在 `Collection` 里面书写url就可以了。

### underscore.js
`underscore.js` 是 Backbone唯一需要依赖的库。
`underscore.js` 在不扩展任何js的原生对象的情况下提供很多实用的功能。它弥补了部分 `jQuery` 没有实现的功能，同时又是Backbone必不可少的部分。`underscore` 也针对模型和集合，提供了数组，对象，事件的常用方法。

##### 模板

```javascript
var temp = _.template('<div class="<%=className%>" id="<%=id%>"><%=name%></div>');
var str = temp(obj);
```

##### 时间戳
原来的取法：

```javascript
var seconds = _.now();
```

`underscore.js`的取法：

```javascript
_.now()
```

##### 随机数
我要随机数，而且是整数

```javascript
parseInt(Math.random()*100)
```

如果改一下，要1到100的整数？

```javascript
_.random(1,100)
```

#### 对象比较
如何比较两个对象是否相等？

```javascript
_.isEqual(obj, _obj);
```

#### pick/omit
服务器返回的结果可能是这样的：

```javascript
var old = {k1: v1, k2: v2, k3: v3, k4: v4, k5: v5};
```

我们需要的可能是这样的：

```javascript
var new = {k1: old.k1, k3: old.k3};
```
- `omit()`: 过滤出除模型特定属性以外的属性值
- `pick`: 过滤出模型特定属性的值
 使用 `underscore.js` 处理：

```javascript
	var new1 = _.pick(old, 'k1', 'k3');
	var new2 = _.omit(old, 'k2', 'k4', 'k5');
```

过滤得多就用pick，复制得多就用omit。
#### extend
> var info = {id:1, name:'jack', sex: 'male', age: '22'};
> var score = {id:1, math:88, biology:64, chinese: 55};
要把个人信息和成绩合并怎么办？
	var student = _.extend(info, score);
#### map
使用传统JS遍历对象：
	function(obj){
	    for(var e in obj){
	        //...
	    }
	}
使用 `underscore`：
	_.map(obj,function(){value, key}{
	    //...
	});
	collection.each(function(value) {
		console.log(value);
	})
#### 合并数组
如何合并多个数组并去重？
	_.union([1, 2, 3], [101, 2, 1, 10], [2, 1]);
> ==\> [1, 2, 3, 101, 10]()
#### 快速获得一个整数数组
	var arr = _.range(1,100);
#### each
	_.each(arr, function(obj, index){
	    console.log(index);
	    console.log(obj);
	});
#### 累加求和
	var count = 0;
	for(var i=0;i<i<arr.length;i++){
	    count += arr[i].price;
	}
	
	var count = _.reduce([{name:'x', price:10}, {name:'y', price:20}], function(memo, item){ 
	    return memo + item.price; 
	}, 0);
#### 过滤
	_.filter([-1, 2, -3, -4, 5, 6], function(num){ 
	        return num > 0; 
	});
> ==\> [2, 5, 6]()
#### 抽取
	_.pluck([{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}], 'name');
> ==\> ["moe", "larry", "curly"]()
#### 排序
不传函数的情况下按大小排序，传函数的情况下可以通过计算结果对原数组排序
	_.sortBy([1, 2, 3, -4, 5, 6]);
> ==\> [-4,1,2,3,5,6]()
	_.sortBy([1, 2, 3, -4, 5, 6], function(num){ 
	    return num*num
	});
> ==\> [1, 2, 3, -4, 5, 6]()
#### indexOf()
返回集合中特定索引位置的模型。
#### min()/max()
获取特定属性为最大值/最小值的model项。
#### size()
获取特定集合的大小。
#### groupBy
通过模型属性将集合进行分组。
#### Collection中继承的underscore方法

```javascript
var methods = ['forEach', 'each', 'map', 'collect', 'reduce', 'foldl','inject', 'reduceRight', 'foldr', 'find', 'detect', 'filter', 'select','reject', 'every', 'all', 'some', 'any', 'include', 'contains', 'invoke','max', 'min', 'toArray', 'size', 'first', 'head', 'take', 'initial', 'rest','tail', 'drop', 'last', 'without', 'difference', 'indexOf', 'shuffle','lastIndexOf', 'isEmpty', 'chain', 'sample']();
```

---- 

## 参考资料
- [Backbone中文文档][7]
- [Backbone学习笔记(重点看这个,按照这个教程全部做完)][8]
- [Backbone使用总结][9]

[7]:	http://www.css88.com/doc/backbone/
[8]:	https://github.com/the5fire/backbonejs-learning-note
[9]:	http://segmentfault.com/a/1190000000589218
