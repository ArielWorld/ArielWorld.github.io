---
layout: post
title:  "ES6一些点"
date:   2016-11-21
excerpt: "somenote"
tag:
- ECMAScript6
comments: true
---

首先，感谢阮一峰，所有内容来自阮一峰开源书籍[ECMAScript 6入门](http://es6.ruanyifeng.com/#README)。

1.function是对象。
2."_test"[0] == "_".(字符串可根据index取)
3.Symbol和proxy不懂。

## proxy

```js
var twice = {
  apply (target, ctx, args) {
    return args;
  }
};
function sum (left, right) {
  return left + right;
};
var proxy = new Proxy(sum, twice);
proxy(1, 2)//[1,2],args是传给apply函数的参数


var twice = {
  apply (target, ctx, args) {
    return arguments;
  }
};
function sum (left, right) {
  return left + right;
};
var proxy = new Proxy(sum, twice);
proxy(1, 2)//[function, undefined, Array[2]],arguments是传给Proxy的参数
```

## Gnerator 函数next()方法的参数

```js
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

上面代码中，第二次运行next方法的时候不带参数，导致y的值等于2 * undefined（即NaN），除以3以后还是NaN，因此返回对象的value属性也等于NaN。第三次运行Next方法的时候不带参数，所以z等于undefined，返回对象的value属性等于5 + NaN + undefined，即NaN。

如果向next方法提供参数，返回结果就完全不一样了。上面代码第一次调用b的next方法时，返回x+1的值6；第二次调用next方法，将上一次yield语句的值设为12，因此y等于24（即var y = 2*12;此时y被定义为24）,返回y / 3的值8；第三次调用next方法，将上一次yield语句的值设为13(即var z = 13;z被定义为13)，因此z等于13，这时x等于5（x是在调用foo()函数时传参被赋值为5），y等于24，所以return语句的值等于42。


## 复习

1.关于块级作用域

```js
function f1() {
  let n = 5;//此处换为var定义，输出结果一样
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}

function f1() {
  let n = 5;//此处换为var定义，输出结果一样
  if (false) {
    let n = 10;
  }
  console.log(n); // 5
}


//var 定义的变量n作用域是f1函数，与let定义的n作用域相同,属于重复定义 故报错
function f1() {
  let n = 5;
  if (true) {
    var n = 10;
  }
  console.log(n); // Uncaught SyntaxError: Identifier 'n' has already been declared
}

function f1() {
  let n = 5;
  if (false) {
    var n = 10;
  }
  console.log(n); // Uncaught SyntaxError: Identifier 'n' has already been declared
}
```

2.关于数值
JavaScript能够精确表示的整数范围在-2^{53}到2^{53}之间（不包含两个端点），超过这个范围无法精确表示。

```js
Math.pow(2,54)==Math.pow(2.53)//false
Math.pow(2,53)==Math.pow(2,53)+1//true
Math.pow(2,53)==Math.pow(2,53)+3//false
Math.pow(2,53)==Math.pow(2,53)+2//false
Math.pow(2,55)==Math.pow(2,55)+1//true
Math.pow(2,55)==Math.pow(2,55)+2//true
Math.pow(2,55)==Math.pow(2,55)+3//true
Math.pow(2,55)==Math.pow(2,55)+4//true
Math.pow(2,55)==Math.pow(2,55)+5//false
```

```js
var x;x = +x;//该操作是把x转为Number,无法转为数值的会被转成NaN

Math.sign();//用于判断一个数是正数负数还是零，没有参数返回NaN
Math.hypot();//返回所有参数的平方和的平方根，没有参数返回0；若有参数但参数不是数值会先转为数值，有一个无法转为数值就返回NaN
```

