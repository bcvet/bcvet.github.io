> 函数是Javascript区别于其他编程语言最大的地方，也是Javascript中设计最为美妙的部分。
> Javascript中的函数就是 **对象**（继承自`Function.prototype`）。
> 函数也是Javascript中的 **一等公民**。
> 函数可以被 **保存** 在变量、其他对象和数组中；
> 函数可以 **被当做参数传递** 给其他函数；
> 函数也可以再 **返回函数**；
> 函数可以拥有 **方法**。
> 函数可以被调用。
> 函数提供作用域支持。

### 函数字面量(Function Literal)
函数也可以通过字面量方式进行创建。

```javascript
var add = function(a, b) {
	return a + b;	
};
add(1);
```

### 调用(Invocation)
函数的调用通过调用运算符 `()` 执行。
`()` 内可包含0个或多个用逗号隔开的表达式，每个表达式表示一个参数值。
当实参与形参个数不符时，不会发生错误：
> 实参 \> 形参：超出的参数会被忽略
> 形参 \< 实参：未传递的参数被设为undefined

#### 方法调用模式
当函数被保存为对象的一个属性时，称为 **方法**。
当一个方法被调用时，`this` 被绑定为该对象。`this` 的绑定发生在调用时。
如果被调用时包含一个提取属性的动作(`.` 或者 `[]`)，那就是被当做一个方法来调用。

```javascript
var foo = {
	"name": "lalala",
	"methodTest": function (argument) {
		console.log(this.name);
	}
};
foo.methodTest();
```

#### 函数调用模式(Function Invocation)
当函数并非一个对象的属性时，那么他就被当做一个 **函数** 来调用。
在这种模式下，`this` 被绑定到全局对象。这是Javascript语言中设计的失误。

```javascript
var foo = {};
foo.name = "jintongyao";
foo.method = function() {
	var test = function () {
		console.log(this.name);
	};
	test();
};
foo.method();
```

#### 构造器调用模式(Constructor Invocation)
Javascript中没有 **类**，Javascript是一种 **基于原型** 的语言。
使用 `new` 关键字可以使用 **构造函数** 构造对象。同时 `this` 也会被绑定到创建的对象中。

```javascript
var Student = function(name, sex) {
	this.name = name;
	this.sex = sex || "boy";
	this.class = "class1";
	this.aboutMe = function() {
		console.log("My name is:" + this.name + ",
			I am a " + this.sex);
	}
}

var boyStudent = new Student("caoyun");
var girlStudent = new Student("penghuicong", "girl");

boyStudent.aboutMe();
girlStudent.aboutMe();
```

#### apply/call调用模式(call && apply)
**apply/call模式** 提供了一种动态调用函数的模式。类似Java反射中的 `invoke`。可以传递 `this的绑定` 和 `参数数组`。
**apply/call模式** 的作用主要为 **运行时动态改变上下文**。
`apply` 与 `call` 的区别主要在于传递的参数不同。

```javascript
function add(c) {
	console.log(this.a + this.b + c);
}

var test1 = {
	a: 1,
	b: 2
};

var test2 = {
	a: 11,
	b: 22
};

var sum = add.apply(test1, [3]);
var sum = add.call(test2, 5);
```

#### this
在5种不同情况下，`this` 的指代都是不同的。
1. 当在全局范围内使用 `this`，它将会指向全局对象。(浏览器环境中为`window`对象)
2. 函数调用，`foo();`这里 `this` 也会指向全局对象。
3. 方法调用，`test.foo(); `，这个例子中，`this` 指向 test 对象。
4. 调用构造函数，`var foo = new foo(); `，如果函数和 `new` 关键词一块使用，则我们称这个函数是 `构造函数`。在函数内部，`this` 指向新创建的对象。
5. 显示的设置`this`，当使用 `Function.prototype` 上的 `call` 或者 `apply` 方法时，函数内的 `this` 将会被显式设置为函数调用的第一个参数。
	
将一个方法赋值给一个变量时，this 指向什么？

```javascript
var test = someObject.methodTest;
test();
```

#### 立即调用的函数表达式（Immediately-Invoked Function Expression）
在Javascript中，一对圆括号 `()` 是一种运算符，跟在函数名之后，表示调用该函数。比如，`print()` 就表示调用 `print函数`。

有时，我们需要在定义函数之后，立即调用该函数。这时，你不能在函数的定义之后加上圆括号，这会产生语法错误。

```javascript
function(){ /* code */ }();
```

产生这个错误的原因是，Javascript引擎看到function关键字之后，认为后面跟的是函数定义语句，不应该以圆括号结尾。

解决方法就是让引擎知道，圆括号前面的部分不是函数定义语句，而是一个表达式，可以对此进行运算。你可以这样写：

```javascript
(function(){
	 /* code */ 
}());
```

或者

```javascript
(function() {
 	/* code */ 
})();
```

这两种写法都是以圆括号开头，引擎就会认为后面跟的是一个表示式，而不是函数定义，所以就避免了错误。这就叫做 `立即调用的函数表达式（Immediately-Invoked Function Expression）`，简称 `IIFE`。

通常情况下，只对匿名函数使用这种 `立即执行的函数表达式`。
它的目的有两个：
> 不必为函数命名，避免了污染全局变量；
> IIFE内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。

#### 参数(Arguments)
当函数被调用时，他会获得一个 **免费** 的参数，即 `arguments` 数组。即便定义时未定义参数，也可使用 `arguments` 使得编写一个动态参数的方法成为了可能。

```javascript
var sum = function() {
	var i, sum = 0;
	for(i = 0; i < arguments.length; i++) {
		sum += arguments[i];
	}
	console.log(sum);
}

sum(1, 2, 3);
```

#### 作用域(Scope)
尽管JavaScript支持一对花括号创建的代码段，但是并不支持`块级作用域`； 而仅仅支持`函数作用域`。

函数的作用域绑定其 **声明** 时所在的作用域。

很容易犯错的一点是，如果**函数A** 调用**函数B**，却没考虑到**函数B**不会引用**函数A**的内部变量。

```javascript
var x = function (){
  console.log(a);
};

function y(f){
  var a = 2;
  f();
}

y(x)
```

#### 闭包(Closure)
`作用域内部`的可以访问 `作用域外部`的变量，但外部如何访问内部的变量呢？
`闭包` 就是定义在函数体内部的函数。更理论性的表达是，**闭包是函数与其生成时所在的作用域对象（scope object）的一种结合**。

```javascript
function testFun() {
    var closure = function (){}; 
}
```

使用闭包封装对象的实例：

```javascript
var closureShow = function() {
	var _innerVar = 1;
	return {
		getInnerVar: function() {
			return _innerVar;
		},
		getDoubleInnerVar: function() {
			return _innerVar * 2;
		}
	}
};

var myClosulre = closureShow();
```
