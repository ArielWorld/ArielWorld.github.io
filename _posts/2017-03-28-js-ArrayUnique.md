---
layout: post
title:  "js数组去重"
date:   2017-03-28
excerpt: ""
tag:
- 数组去重
comments: true
---

以var arr = [1,1,'1','1',0,0,'0','0',undefined,undefined,null,null,NaN,NaN,{},{},[],[],/a/,/a/];为例测试各去重方法。

```js
//Set方法，NaN可以去重，看起来相同的对象和数组、正则表达式不会被去重.计时: 0.363ms
Array.from(new Set(arr));
//或者写为,计时: 0.095ms
[...new Set(arr)]
//[1, "1", 0, "0", undefined, null, NaN, Object, Object, Array[0], Array[0], /a/, /a/]
console.time('计时');
unique(arr);
console.timeEnd('计时');

//或者使用includes，NaN可以去重，对象和数组、正则表达式不会被去重,计时: 0.339ms
function unique(arr){
    var brr = [];
    for(var i=0;i<arr.length;i++){
        if(!brr.includes(arr[i])){brr.push(arr[i]);}
    }
    return brr;
}
//[1, "1", 0, "0", undefined, null, NaN, Object, Object, Array[0], Array[0], /a/, /a/]
//由此例可知
var a = [[],/a/,{},NaN];
a.includes([]);//false
a.includes(/a/);//false
a.includes({});//false
a.includes(NaN);//true
```

ES5实现：

ES5实现方法多用'==',关于'=='和'==='，有些比较有意思的比较：

```js
0=='';//true
0==='';//false
0==null;//false
0==undefined;//false
null==undefined;//true
NaN==NaN;//false
var a1 = 'a';
var a2 = new String('a');
var a3 = new String('a');
a1 == a2;//true
a1 == a3;//true
a2 == a3;//false,对象都不相等
a1 === a2;//false
a1 === a3;//false
```

```js
//indexOf方法,NaN不会去重，看起来相同的对象和数组、正则表达式也不会被去重。计时: 0.374ms
function unique(arr){
    if(Object.prototype.toString.call(arr).slice(8,-1) === 'Array'){
        var brr = [];
        for(var i=0;i<arr.length;i++){
            if(brr.indexOf(arr[i])==-1) brr.push(arr[i]);
        }
        return brr;
    }else{
        throw new Error(arr+' is not Array');
    }
}
//[1, "1", 0, "0", undefined, null, NaN, NaN, Object, Object, Array[0], Array[0], /a/, /a/]
//或者变形写法，结果跟上面一致，但速度慢些。计时: 0.433ms
function unique(arr){
    if(Object.prototype.toString.call(arr).slice(8,-1) === 'Array'){
        var brr = [];
        arr.forEach(function(item,index){
            if(brr.indexOf(item) === -1){
                brr.push(item);
            }
        });
        return brr;
    }else{
        throw new Error(arr+' is not Array');
    }
}

//判断数组下标，NaN会消失，看起来相同的对象和数组、正则表达式也不会被去重。计时: 0.286ms
function unique(arr){
    if(Object.prototype.toString.call(arr).slice(8,-1) === 'Array'){
        var brr = [arr[0]];
        //从数组第二项开始遍历
        for(var i=1;i<arr.length;i++){
            //如果数组第i项第一次在数组中出现的位置不是i,说明是重复项
            if(arr.indexOf(arr[i])==i) brr.push(arr[i]);
        }
        return brr;
    }else{
        throw new Error(arr+' is not Array');
    }
}
//[1, "1", 0, "0", undefined, null, Object, Object, Array[0], Array[0], /a/, /a/]
//判断数组下标的变形写法，结果跟上面一致，但速度慢些。计时: 0.782ms
function unique(arr){
    if(Object.prototype.toString.call(arr).slice(8,-1) === 'Array'){
        return arr.filter(function(item,index){
             return arr.indexOf(item) == index;
        });
    }else{
        throw new Error(arr+' is not Array');
    }
}


//推荐使用的对象方法，NaN会被去重，看起来相同的对象和数组、正则表达式也会被去重计时: 1.149ms
function unique(arr){
    if(Object.prototype.toString.call(arr).slice(8,-1) === 'Array'){
        var brr = [];
        var obj = {};
        var len = arr.length;
        //为对象的属性加一个类型，这样就可以区分数值1和字符串‘1’，不会认为1和‘1’重复了
        var tempKey = '';
        for(var i=0;i<len;i++){
            //typeof优先级比加法运算符高，此处得到数组成员类型加成员如‘string1’
            tempKey = typeof arr[i]+arr[i];
            if(!obj[tempKey]){
                brr.push(arr[i]);
                obj[tempKey] = 1;
            }
        }
        return brr;
    }else{
        throw new Error(arr+' is not Array');
    }
}
//[1, "1", 0, "0", undefined, null, NaN, Object, Array[0], /a/]

//但是上述写法会将所有的对象都认为是重复的
var a = [{},{n:1}];
unique(a);//[{}]
//所以可以
tempKey = typeof arr[i]+JSON.stringify(arr[i]);
//这样不同的对象可以得到保留，但是正则表达式/a/会消失，因为JSON.stringify(/a/)结果是'{}'，和空对象一样
```

综上可知，数组去重需要建立在一定的条件之下，不能一概而论。有一篇非常全面的博客可以参考[也谈JavaScript数组去重](https://www.toobug.net/article/array_unique_in_javascript.html)。