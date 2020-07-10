---
title: JS交换两个变量的值
date: 2015-11-17 16:15:09
tags: javascript
categories: javascript
---

这个问题看似很基础，却有很多的实现方式
+ 使用中间变量
``` javascript
var a = 3, b = 2;
var temp;
temp = a;
a = b;
b = temp;
console.log(a, b);  // 2,3
```

+ 使用运算符(仅数字)
``` javascript
var a = 3, b = 2;
// 加法
b = a + b;
a = b - a;
b = b - a;
console.log(a, b);  // 2,3

// 减法
var a = 3, b = 2;
b = a - b;
a = a - b;
b = a + b;
console.log(a, b);  // 2,3

// 异或
var a = 3, b = 2;
b = a ^ b;
a = a ^ b;
b = a ^ b;
console.log(a, b);  // 2,3
```
<!--more-->
+ 利用用运算符优先级
``` javascript
var a = 3, b = 2;
a = [b, b = a][0];
console.log(a, b);  // 2,3
```

+ ES6解构赋值
``` javascript
var a = 3, b = 2;
[a, b] = [b, a];
console.log(a, b);  // 2,3
```

+ 数组元素交换位置
``` javascript
var arr = [3, 2, 4, 1];
// 交换第i个和第k个元素
// arr[i] = arr.splice(k, 1, arr[i])[0];
arr[0] = arr.splice(3, 1, arr[0])[0];
console.log(arr); // [1, 2, 4, 3]
```
