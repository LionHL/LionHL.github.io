---
title: JavaScript中的遍历循环方法之map、forEach、for-in、for-of
date: 2017-06-29 11:49:55
tags: [笔记,js] 
---

## map()
对数组的所有成员一次调用一个函数，根据函数结果返回一个新数组。   

```
var array = [1, 2, 3];
array.map(function (n) {
	return n + 1;
});
// [2, 3, 4]

array
// [1, 2, 3]

/*
 *	array数组所有成员都加上1，组成新数组，原数组没有变化
 */
```
<!--more--> 
   
`map`方法接收一个函数作为参数。调用时传入三个参数，分别是当前成员、当前位置和数组本身。    

语法：

```
var new_array = arr.map(function callback(currentValue, index, array) {
    // Return element for new_array
}[, thisArg])

// thisArg 可选的。执行 callback 函数时 使用的this 值
```

```
var arr = ['a', 'b', 'c'];
[1, 2].map(function (e) {
	return this[e];
}, arr)
// ['b', 'c']
```

```
[1, 2, 3].map(function (elem, index, arr) {
	return elem * index;
});
//	[0, 2, 6]
```

`map`方法还可以用于字符串，用来遍历字符串的每个字符。但不能直接使用，要通过函数的`call`方法间接使用，或者先转为数组，然后使用。

```
var upper = function (x) {
	return x.toUpperCase();
};

[].map.call('abc', upper)
// ['A', 'B', 'C']

// 或者
'abc'.split('').map(upper)
// ['A', 'B', 'C']
 
 
// 反转字符串：
var str = '12345';
Array.prototype.map.call(str, function(x) {
	return x;
}).reverse().join('');
// '54321'
```

如果数组有空位，`map`方法的回调函数在这个位置不会执行，会跳过数组的空位。

```
var f = function(n) { return n + 1 };

[1, undefined, 2].map(f)	// [2, NaN, 3]
[1, null, 2].map(f)			// [2, 1, 3]
[1,  , 2].map(f)				// [2,  , 3]
```
<br>
## forEach()
`forEach`与`map`方法相似，遍历数组所有成员，执行某种操作，但不返回值。   

`forEach`方法接收一个函数作为参数。调用时传入三个参数，分别是当前成员、当前位置和数组本身。语法：

```
array.forEach(callback(currentValue, index, array) {
	// do something
}, this)

array.forEach(callback[, thisArg])

// thisArg 可选参数。当执行回调 函数时用作this的值(参考对象)。
```

```
function log(element, index, array) {
	console.log('[' + index + '] = ' + element);
}

[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [3] = 9
```

```
// 接受第二个参数，用来绑定回调函数的 this 关键字
var out = [];

[1, 2, 3].forEach(function(elem) {
	this.push(elem * elem);
}, out);

out		// [1, 4, 9]
```

`forEach`会跳过数组的空位   

```
var log = function (n) { console.log(n + 1); };

[1, undefined, 2].forEach(log)	
// 2
// NaN
// 3

[1, null, 2].forEach(log)
// 2
// 1
// 3

[1,  , 2].forEach(log)
// 2
// 3

```

注：`forEach`方法无法中断执行，会将所有成员遍历完。如果想符合某种条件中断遍历，使用`for`循环。   

`forEach`也可用于类似数组的对象和字符串。

```
var obj = {
	0: 1,
	a: 'hello',
	length: 1
}

Array.prototype.forEach.call(obj, function (elem, i) {
	console.log(i + ':' + elem);
});
// 0:1

// obj是一个类似数组的对象，forEach方法可以遍历它的数字键

var str = 'hello';
Array.prototype.forEach.call(str, function (elem, i) {
	console.log(i + ':' + elem);
});
// 0:h
// 1:e
// 2:l
// 3:l
// 4:o
```
<br>
## for in
`for-in`循环实际是为循环`enumerable`对象而设计的，一般不推荐循环数组

```
var obj = {
	a: 1,
	b: 2,
	c: 3
}

for (var prop in obj) {
	console.log('obj.' + prop + ' = ' + obj[prop]);
};

// obj.a = 1
// obj.b = 2
// obj.c = 3

/* 循环数组 */
fof (var i in maArray) {			// 不推荐
	console.log(myArray[index]);
}

```
<br>
## for of
语法：   

```
for (variable of object) {
	statment
}
```

遍历Array:

```
var array = [10, 20, 30];
for (let value of array) {
	console.log(value);
}
// 10
// 20
// 30
```

遍历String：

```
let it = 'boo';
for (let value of it) {
	console.log(value);
}
// b
// o
// o
```

遍历DOM集合：   

```
// 给每个article标签的p子标签添加一个‘read’ class

let articleParagrahs = documment.querySelectorAll('article > p');

for (let paragrahs of articleParagraphs) {
	paragrahs.classList.add('read');
}

```

遍历生成器

```
function* fibonacci() {		// 一个生成器函数
	let [prev, curr] = [0, 1];
	for (;;) {
		[prev, curr] = [curr, prev + curr];
		yield curr;
	}
}

for (let n of fibonacci()) {
	if (n > 1000)
		break;
	console.log(n);
}
```
<br>

****
<br>
## for of 与 for in的区别
for in循环会遍历一个object所有的可枚举属性。   
for of是为了各种collection对象专门定制的，并不适用于所有的object。它会议这种方式迭代出任何拥有[Symbol.iterator]属性的collection对象的每个元素。   
   
for in 遍历（当前对象及原型上的）每个元素属性名称   
for of 遍历（当前对象上的）每一个属性值：

```
Object.prototype.objCustom = function () {};
Array.prototype.arrCustom = function () {};

let it = [3, 5, 7];
it.foo = 'hello';

for (let i in it) {
	console.log(i);		// logs 0, 1, 2, 'foo', 'arrCustom', 'objCustom'
}

for (let i of it) {
	console.log(i);		// logs 3, 5, 7
}
```





