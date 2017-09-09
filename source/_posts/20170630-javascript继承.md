---
title: javascript继承
date: 2017-07-30 21:05:02
tags: [javascript]
---
　
javascript是一门面向对象的弱类型语言，继承也是它的一大特性。

### 原型继承
要继承就要有子类和父类

``` javascript
	// 父类
	function superClass(name) {
		this.name = name || '我是父类';
	}
	superClass.prototype.getName = function() {
		console.log(this.name);
	}
	// 子类
	function subClass(name) {
		superClass.call(this, name);
	}
```
<!-- more -->
然后在把superClass实例赋予subClass原型

``` javascript 
	subClass.prototype = new superClass();
	subClass.constructor = subClass;
	var sub = new subClass('我是子类');
	sub.getName(); // 我是父类
	console.log(sub instanceof superClass); //true
	console.log(sub instanceof subClass); //true
```

