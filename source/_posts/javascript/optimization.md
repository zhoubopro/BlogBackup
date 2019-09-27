---
title: JS性能优化之代码优化
top: true
cover: false
date: 2019-09-18 14:02:52
password:
summary:
tags:
- javaScript
- code优化
categories:
- javaScript
---

#### 利用函数的惰性载入提高 javaScript 代码性能
在 javaScript 代码中，因为各浏览器之间的行为的差异，我们经常会在函数中包含了大量的 ```if``` 语句，以检查浏览器特性，解决不同浏览器的兼容问题。 例如，我们最常见的为 ```dom``` 节点添加事件的函数：
```js
function addEvent (type, element, fun) {
	if (element.addEventListener) {
		element.addEventListener(type, fun, false);
	}
	else if(element.attachEvent){
		element.attachEvent('on' + type, fun);
	}
	else{
		element['on' + type] = fun;
	}
}
```
每次调用 ```addEvent``` 函数的时候，它都要对浏览器所支持的能力进行检查，首先检查是否支持 ```addEventListener``` 方法，如果不支持，再检查是否支持 ```attachEvent``` 方法，如果还不支持，就用 dom 0 级的方法添加事件。 这个过程，在 ```addEvent``` 函数每次调用的时候都要走一遍，其实，如果浏览器支持其中的一种方法，那么他就会一直支持了，就没有必要再进行其他分支的检测了， 也就是说，```if``` 语句不必每次都执行，代码可以运行的更快一些。

解决的方案就是称之为惰性载入的技巧。

所谓惰性载入，就是说函数的if分支只会执行一次，之后调用函数时，直接进入所支持的分支代码。 有两种实现惰性载入的方式，第一种事函数在第一次调用时，对函数本身进行二次处理，该函数会被覆盖为符合分支条件的函数，这样对原函数的调用就不用再经过执行的分支了， 我们可以用下面的方式使用惰性载入重写 ```addEvent()```。
```js
function addEvent (type, element, fun) {
	if (element.addEventListener) {
		addEvent = function (type, element, fun) {
			element.addEventListener(type, fun, false);
		}
	}
	else if(element.attachEvent){
		addEvent = function (type, element, fun) {
			element.attachEvent('on' + type, fun);
		}
	}
	else{
		addEvent = function (type, element, fun) {
			element['on' + type] = fun;
		}
	}
	return addEvent(type, element, fun);
}
```
在这个惰性载入的 ```addEvent()``` 中，```if``` 语句的每个分支都会为 ```addEvent``` 变量赋值，有效覆盖了原函数。 最后一步便是调用了新赋函数。下一次调用 ```addEvent()``` 的时候，便会直接调用新赋值的函数，这样就不用再执行 ```if``` 语句了。

第二种实现惰性载入的方式是在声明函数时就指定适当的函数。 这样在第一次调用函数时就不会损失性能了，只在代码加载时会损失一点性能。 一下就是按照这一思路重写的 ```addEvent()```。
```js
var addEvent = (function () {
	if (document.addEventListener) {
		return function (type, element, fun) {
			element.addEventListener(type, fun, false);
		}
	}
	else if (document.attachEvent) {
		return function (type, element, fun) {
			element.attachEvent('on' + type, fun);
		}
	}
	else {
		return function (type, element, fun) {
			element['on' + type] = fun;
		}
	}
})();
```
这个例子中使用的技巧是创建一个匿名的自执行函数，通过不同的分支以确定应该使用那个函数实现，实际的逻辑都一样， 不一样的地方就是使用了函数表达式（使用了 ```var``` 定义函数）和新增了一个匿名函数，另外每个分支都返回一个正确的函数，并立即将其赋值给变量 ```addEvent```。

惰性载入函数的优点只执行一次 ```if``` 分支，避免了函数每次执行时候都要执行 ```if``` 分支和不必要的代码，因此提升了代码性能，至于那种方式更合适，就要看您的需求而定了。

