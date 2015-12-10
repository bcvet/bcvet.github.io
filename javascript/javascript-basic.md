### 引言
先看如下两个例子，判断一下运行结果。

1.

```javascript
var myVal = "my value";
function test() {
    console.log(myVal);
	var myVal = "local value";
};
test();
```

2.

```javascript
function printName() {
	return this.yourName;
}
printName.yourName = "jintongyao";
console.log(printName());
```

### 声明与定义(Declare && Define)
如果只是声明变量而没有赋值，则该变量的值为undefined。
`var a;` 等价于 `var a = undefined;`

思考如下两个操作的区别：

```javascript
var a = 1;
a = 1;
```

如果对变量未声明而直接赋值的话（如 `a = 1;`），此时的变量将定义为 **全局变量**（即便语句在函数内部）！此时往往会不知不觉的造成 **全局变量污染**。所以一定要使用关键字 `var` 对变量先进行声明。

如下方法也会造成同样的问题：

```javascript
function foo() {
    var a = b = 0;
    // ...
}
```

`var a = b = 0; ` 等价于 `var a = (b = 0);`

> 规范：变量要先声明再赋值，不要忘记使用var。

### 变量提升(Hoisting)
在Javascript中，会自动将变量的声明进行提升。
这意味着 `var表达式` 和 `function声明` 都将会被提升到当前作用域的顶部。

引言中的代码，等价于：

```javascript
var myVal = "my value";
function test() {
	var myVal;
    console.log(myVal);
 	var myVal = "local value";
// var myVal;
// myVal = "local value";
};
test();
```

为了避免这种情况可能带来的问题，我们应该将变量的声明显示的放在作用域最上方。这样做的好处有：
- 在同一个位置可以查找到函数所需的所有变量
	- 避免当在变量声明之前使用这个变量时产生的逻辑错误
	- 提醒你不要忘记声明变量，顺便减少潜在的全局变量

使用 `function声明` 定义的函数也会被提升。

```javascript
(function () {
    foo();
    function foo() {
        alert("我来自 foo");
    }
}());
```

> 规范：在作用域开始时，先声明好所有可能需要使用的变量。
