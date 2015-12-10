
> 对象，也可以理解为一种数据集合，类似于Java中的Map，由 **key-value** 方式构成。

### 对象字面量(Object Literals)
一般的对象创建方式。

```javascript
var defaultObj = {};
// var defaultObj = new Object();
// var defaultObj = Object.create(null);

defaultObj.name = "jintongyao";
defaultObj.age = 28;
```

更好的创建对象的方法，是使用 `对象字面量` 来进行创建。
即，用花括号包围的 `key-value` 来进行对象的定义。

```javascript
var man = {
	"name": "tongyao",
    "sex": "male",
    "job": "programmer"
};
```

JavaScript中自定义的对象（用户定义的本地对象）任何时候都是可变的。内置本地对象的属性也是可变的。

对象的属性名可以加引号或者不加引号。
但如果属性名本身并非合法的标示符名，则需要添加引号。
如, `first_name` 可以不使用引号，但 `first-name` 则必须使用引号包装。（因为在Javascript的标识符中使用 `连接符 -` 为不合法的。）

> 规范：不要忘记变量定义最后的分号。

### 对象的引用(Reference)
Javascript中，对于对象的传递为 **引用传递**。也就是说，不同变量名指向同一个对象，实际指向了相同的内存地址，对其中任何一个对象的修改，都会引起其他对象引用的改变。

```javascript
var foo = {};
var bar = foo;
foo.name = "Tom";
console.log(bar.name);
```

需要特别说明的是，与对象相对应的是，原始类型的传递采用 **值复制** 的方式。

### 默认值运算符 `||`
可以使用运算符 `||` 来填充对象或者属性的默认值。

```javascript
var app = app || {};
var status = schedule.status || "complete";
```

### 读取/写入属性(Read/Write)
对象的属性，可以通过 `.` 运算符或者 `[]` 运算符来进行读取。
这两种方法都可以使用，一般来说使用 `.` 运算符更加直观。
如 `var myName = student.name;`。
但如果属性的值，是通过变量名进行传递的，则只有在 `[]` 运算符中才可以获得正确的动态值。
数值键名也不能使用 `.` 运算符（因为会被当成小数点），只能使用 `[]` 运算符。

```javascript
function readProperty(propName) {
    console.log(foo[propName]);// Tom
    console.log(foo.propName);// undefined
}

readProperty(name);
```

对属性的赋值也是如此，使用以上两种运算符：
```javascript
var foo = {};
foo.name = "tom";
foo["sex"] = "male";
```

> 规范：默认使用`.`运算符读取写入属性，特别情况下使用 `[]` 运算符。

### 原型(Prototype)
每个对象都连接到一个原型对象，而且能够从中继承属性。
所有通过对象字面量创建的对象都连接到 `Object.prototype` ，他是javascript中的 **基类** 。

### in运算符(in)
in运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回true，否则返回false。

```javascript
var o = { 
	p: 1 
};
'p' in o;// true
```

在JavaScript语言中，所有全局变量都是顶层对象（浏览器的顶层对象就是window对象）的属性，因此可以用in运算符判断 **一个全局变量是否存在** 。

```javascript
// 假设变量x未定义

// 写法1：报错
if(x) {
	console.log("in");
}

// 写法2：不正确
if(window.x) {
	console.log("in");
}

// 写法3：正确
if('x' in window) {
	console.log("in");
}
```

### 枚举(Enumerations) && 反射(Reflection)
对对象使用 `for-in` 循环，可以遍历对象的所有属性。也包括从 **原型链上继承** 的的属性。

如果不想包含从原型链上继承的的属性，需要使用 `hasOwnProperty` 方法。
如果希望过滤函数，可以使用 `typeof` 运算符。

也可使用 `Object.keys()` 方法获得所有的属性名。

### 删除属性(Delete)
使用 `delete` 运算符可以删除对象的属性。

```javascript
delete foo.name;
```

`delete` 运算符不会删除原型链中的属性。如果对象中的属性覆盖了原型链中的属性，删除对象属性后，使用原型链的属性。

### 减少全局变量污染
为每个app定义一个唯一的全局变量。将这个变量作为其他变量的容器。

```javascript
var myApp = {};
myApp.student = {
	name: "zhouyuanzheng",
	age: 30
}
```

> 规范：慎用全局变量。创建前考虑是否可以使用命名空间进行管理。

### 思考：如何复用对象？
