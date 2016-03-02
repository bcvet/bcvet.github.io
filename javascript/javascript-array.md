## Array的方法
ES5中,Array对象新增了部分方法,多加利用可以大大减少代码量:

### map
`map`方法对数组的所有成员依次调用一个函数，根据函数结果返回一个新数组。

	var numbers = [1, 2, 3];
	
	var newNumbers = numbers.map(function(item) {
		return item + 1; 
	});
	console.log(newNumbers); // [2, 3, 4]
	console.log(numbers); // [1, 2, 3]

map方法的回调函数依次接受三个参数，分别是当前的数组成员、当前成员的位置和数组本身。

	[1, 2, 3].map(function(item, index, array) {
		return item;// [1,2,3]
		// return index; // [0,1,2]
		// return array; // [1,2,3],[1,2,3],[1,2,3]
	});

### forEach
数组实例的`forEach`方法与`map`方法很相似，也是遍历数组的所有成员，执行某种操作，但是`forEach`方法没有返回值，一般只用来操作数据。如果需要有返回值，一般使用`map`方法。

	function log(item, index, array) {
		console.log(`${index} = ${item}`);
	}

	[2, 5, 9].forEach(log);
	// [0] = 2
	// [1] = 5
	// [2] = 9

### filter
`filter`方法依次对所有数组成员调用一个测试函数，返回结果为true的成员组成一个新数组返回。

	var newArr = [1, 2, 3, 4, 5].filter(function(item) {
	  return (item > 3);
	})
	
	console.log(newArr);// [4, 5]

### some & every
这两个方法类似`断言assert`，用来判断数组成员是否符合某种条件。

`some`方法对所有元素调用一个测试函数，**只要有一个元素通过该测试**，就返回true，否则返回false。

	var arr = [1, 2, 3, 4, 5];
	var someFlag = arr.some(function (item, index, arr) {
	  return item >= 3;
	});

	console.log(someFlag);// true
	
`every`方法对所有元素调用一个测试函数，**只有所有元素通过该测试**，才返回true，否则返回false。
	
	var arr = [1, 2, 3, 4, 5];
	var someFlag = arr.every(function (item, index, arr) {
	  return item >= 3;
	});

	console.log(someFlag);// false


### reduce & reduceRight
`reduce`方法和`reduceRight`方法的作用，是依次处理数组的每个元素，最终累计为一个值。
这两个方法的差别在于，`reduce`对数组元素的处理顺序是**从左到右（从第一个成员到最后一个成员）**，`reduceRight`则是**从右到左（从最后一个成员到第一个成员）**，其他地方完全一样。

`reduce`方法的第一个参数是一个处理函数。
该函数接受四个参数，分别是：
	1. 用来累计的变量（即当前状态），默认值为0
	2. 数组的当前元素
	3. 当前元素在数组中的序号（从0开始）
	4. 原数组
这四个参数之中，只有前两个是必须的，后两个则是可选的。

	var sum = [1, 2, 3, 4, 5].reduce(function(sum, current){
	    return sum + current;
	});
	
	console.log(sum); // 15

### 链式使用
上面这些数组方法之中，有不少返回的还是数组，所以可以链式使用。
	
	var users = [
		{name:"tom", email:"tom@example.com"},
	 	{name:"peter", email:"peter@example.com"}
	];

	users.map(function(user){return user.email;})
		.filter(function (email) {return /^t/.test(email);})
		.forEach(alert);
		
		// 弹出tom@example.com



