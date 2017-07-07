---
title: 关于javascript继承的学习笔记
date: 2017-07-07 11:04:56
tags: [笔记,js]
---

这几天学习js的继承，看了一些文章，其中有
<a href='http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html' target='_blank'>阮一峰的对象之间的“继承”的五种方法</a>，<a href='http://keenwon.com/1524.html' target='_blank'>keenwon的ES5和ES6中的继承</a>，对JavaScript的继承有了一些了解，顺便写个学习总结。

每个对象都有一个私有属性（[[Prototype]]），它持有一个连接到另一个称其为prototype对象的链接。该原型对象具有一个自己的原型，等等，直到达到一个对象的prototype为null。   
null没有prototype。  
<!--more-->
# 基于原型链的继承
遵循ECMAScript的标准，someObject.\_\_proto\_\_符号是用于指派someObject的原型。等同于JavaScript的\_\_proto\_\_属性。ES6开始，\_\_proto\_\_可以用`Object.getPrototypeOf()`和`Object.setPrototypeOf()`访问器来访问。   

当继承的函数被调用时，`this` 指向的是当前继承的对象，而不是继承的函数所在的原型对象。

```
var o = {
	a: 2,
	m: function() {
		return this.a + 1;
	}
}
console.log(o.m());		// 3

var p = Object.create(o);
//	p.__proto__是o
p.a = 12;
console.log(p.m());		// 13
```

<br>

# 不同的方法创建对象和生成原型链
### 普通语法
```
var o = { a: 1};
// o ---> Object.prototype ---> null

var a = ['yo', 'whadup', '?'];
// a ---> Array.prototype ---> Object.prototype ---> null

function f(){ return 2; }
// f ---> Function.prototype ---> Object.prototype ---> null
``` 

### 构造器创建对象
在JavaScript中，构造器就是一个普通函数。当使用`new 操作符`来作用这个函数时，它就可以被称为构造函数（方法）。

```
function Graph() {
	this.vertices = [];
	this.edges = [];
}
Graph.prptotype = {
	addVertex: function (v) {
		this.vertices.push(v);
	}
}
var g = new Graph();
// 在g被实例化时，g.__proto__指向Graph.prototype.
```

### 使用Object.create创建对象
```
var a = {a: 1};
// a ---> Object.prototype ---> null

var b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a);	// 1

var c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

var d = Object.create(null);
// d ---> null
```

<br>

# 构造函数的继承方法 
```
function Animal () {
	this.species = '动物';
}

function Cat (name, color) {
	this.name = name;
	this.color = color;
}
```
### 1、构造函数绑定
使用call或者apply方法，将父对象的构造函数绑定在子对象中

```
function Cat(name, color) {
	Animal.apply(this, arguments);
	this.name = name;
	this.color = color;
}

var cat1 = new Cat('大毛', '黄色');
alert(cat1.species);	// 动物
```

### 2、prototype模式
```
// 将Cat的prototype对象指向一个Animal的实例
Cat.prototype = new Animal();	
Cat.prototype.constructor = Cat;	

var cat1 = new Cat('大毛', '黄色');
alert(cat1.species);	// 动物
```  
`Cat.prototype.constructor = Cat;` :   
任何一个prototype对象都有一个constructor属性，指向它的构造函数。如果没有`Cat.prototype = new Animal();`这行代码，`Cat.prototype.constructor`指向的是Cat，加上后，这段代码指向Animal。   
更重要的是每个实例也有一个constructor属性，默认调用prototype对象的constructor属性。

```
alert(cat1.constructor == Cat.prototype.constructor);	// true
```
运行`Cat.prototype = new Animal();`后，cat1.constructor也指向Animal！   
这会导致继承链的紊乱，因此需要手动矫正。

### 3、直接继承prototype
这种方法是对第二种的改进。Animal对象中，不变的属性都可以写入Animal.prototype中，所以可以让Cat()跳过Animal()，直接继承Animal.prototype。

```
function Animal () {}
Animal.prototype.species = '动物';
```

```
// 将Cat的prototype对象指向Animal的prototype对象，完成继承
Cat.prototype = Animal.prototype;
Cat.prototype.constructor = Cat;

var cat1 = new Cat('大毛', '黄色');
alert(cat2.species);	// 动物
```

与第二中方法相比，效率比较高，省内存。缺点是Cat.prototype和Animal.prototype现在指向了同一对象，那么任何对Cat.prototype的修改，都会反映到Animal.prototype。

```
Cat.prototype.constructor = Cat;
// 这段代码实际上把Animal.prototype对象的constructor属性也修改了：
alert(Animal.prototype.constructor);		// Cat
```

### 4、利用空对象作为中介
```
var F = function () {};
F.prototype = Animal.prototype;
Cat.prototype = new F();
Cat.prototype.constructor = Cat;
```
F是空对象，所以几乎不占内存。此时修改Cat的prototype对象，就不会影响到Animal的prototype对象。   
将其封装成一个函数，方便使用。   

```
function extend(Child, Parent) {
	var F = function () {};
	F.prototype = Parent.prototype;
	Child.prototype = new F();
	Child.prototype.constructor = Child;
	Child.uber = Parent.prototype;
}
```

` Child.uber = Parent.prototype; `意思是为子对象设一个uber属性，这个属性直接指向父对象的prototype属性。等于在子对象上打开一条通道，可以直接调用父对象的方法。在这只是为了实现继承的完备性，纯属备用性质。

### 5、拷贝继承
将父对象的所有属性和方法拷贝进子对象。

```
function Animal(){}
Animal.prototype.species = '动物';
```
```
// 实现属性拷贝目的的函数
function extend2(Child, Parent) {
	var p = Parent.prototype;
	var c = Child.prototype;
	
	for (var i in p) {
		c[i] = p [i];
	}
	c.uber = p;
}
```
```
// 使用
extend2(Cat, Animal);
var cat1 = new Cat('大毛', '黄色');
alert(cat1.species);	// 动物
```
