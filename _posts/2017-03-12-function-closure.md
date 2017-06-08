---
layout: post
title:  "闭包，为什么都喜欢问闭包?"
date:   2017-03-12
excerpt: ""
tag:
- 闭包
- 斐波那契
comments: true
---

## 闭包

```js
function c(n){
    for(var i=n;i>0;i--){
        setTimeout(function(){
             console.log(i);
          },1000);//此处若省略1000，会立即执行setTimeout函数
    }
}
c(3);//输出3次0

//正常的倒计时
function c(n){
    for(var i=n;i>0;i--){
        (function(i){
            setTimeout(function(){
                console.log(i);
            },1000);
        })(i);
    }
}
c(3);//3 2 1
```

知乎上的变形面试题：

```js
for(var i=0;i<5;i++){
    setTimeout(function(){
        console.log(i);
        },1000*i);
}//自测，发现确实每隔一秒输出一个5,共5个5

//加闭包
for(var i=0;i<5;i++){
    (function(i){
        setTimeout(function(){
            console.log(i);
            },1000*i);
        })(i);
}//输出0 1 2 3 4 每隔一秒

//加闭包，去掉闭包的参数
for(var i=0;i<5;i++){
    (function(){
        setTimeout(function(){
            console.log(i);
            },1000*i);
        })(i);
}//没有对i保持引用，仍然是每隔一秒输出5 5个

//若setTimeout接受的是立即执行函数
for(var i=0;i<5;i++){
    setTimeout(function(i){
        console.log(i);
        }(i),1000*i);
}//立即执行函数会立即执行，输出0 1 2 3 4

//若setTimeout接受的是立即执行函数
for(var i=0;i<5;i++){
    setTimeout(function(){
        console.log(i);
        }(i),1000*i);
}//立即执行函数会立即执行，输出0 1 2 3 4
```

//懒得另写一篇，把斐波那契一起加上吧~~

## 斐波那契

```js
//使用缓存,输出n个斐波那契数列
var fib = function(){
    var cache = [1,1];
    return function(n){
        if(n>2){
            for(var i=2;i<n;i++){
                cache[i] = cache[i-1]+cache[i-2];
            }
        }
        return cache;
    };
}();

//若单纯只要计算到第n个
function fib(n){
    if(n<=2){
        return 1;
    }
    var a=1,b=1;
    for(var i=2;i<n-1;i++){
        b=a+b;
        a=b-a;
    }
    return a+b;
}

//事实上，常见的是输出n(比如1000)以内的斐波那契数列，真是变态
function fib(n){
    var i=1,j=1,last=1;
    var arr = [1,1];
    while(true){
        last = i+j;
        if(last>n)break;
        i=j;
        j=last;
        arr.push(last);
    }
    console.log(arr.join());
}
```



