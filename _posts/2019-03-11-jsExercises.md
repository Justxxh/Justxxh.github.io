---
layout: post
title: "日常js练习【持续更新ing】"
date: 2019-03-11 18:49:20
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

