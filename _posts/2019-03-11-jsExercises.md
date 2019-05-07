---
layout: post
title: "日常js练习【持续更新ing】"
date: 2019-03-10 18:49:20
author: "JustX"
header-img: "assets/img/dhxy1.jpg"
tags: [JavaScript]
comments: true

---

<em>练习集</em>

1.写一个闭包，每次调用自增1

```js
var add = (function increase() {
    var tmp = 0;
    return function () {
        console.log(++tmp);
    }
})()
add()
```

2.递归计算阶乘

```js
function num(n){
    if(n==1) return 1;
    return num(n-1)*n;
}
num(100)
```

3.词法作用域（js采用该种）与动态作用域

> 来源 《你不知道的JavaScript上卷》- 作用域章节

```js
function foo(){
	console.log(a);
}
function bar(){
	var a = 3;
	foo();
}
var a = 2;
bar()
// 答案 2
// 假设在动态作用域 答案则为 3
```

```js
function foo(a){
	var b = a * 2;
	function bar(c){
		console.log(a,b,c)
	}
	bar(b*3);
}
foo(2);
// 2 4 12
```

```js
// eg1
function foo(str,a){
	eval(str);
	console.log(a, b);
}
var b = 2
foo("var b = 3", 1)
// 1 3

// eg2

function foo(str){
  "use strict";
	eval(str);
	console.log(a);
}
foo("var a = 2");
//在严格模式下 eval 运行时有自己的词法作用域因此其中的声明无法修改所在的作用域，则抛出：ReferenceError: a is not defined
```

