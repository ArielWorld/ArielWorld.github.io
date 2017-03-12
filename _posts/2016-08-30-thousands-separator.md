---
layout: post
title:  "js实现千位分隔符引发的正则表达式学习惨案"
date:   2016-08-30
excerpt: ""
tag:
- 千位分隔符
- 正则表达式
comments: true
---

最开始参考网上写的特挫的方法如下，毫无疑问被嘲笑，可是写写又不犯法，而且我注释的这么贴心：

```js
function test1(num) {
    if(!num) return;
    //传入的是数值，要转成字符串才可用字符串的方法
    var num = num.toString(),
        nums = num.split('.');
    var n = nums[0] || 0, 
        decimal = !!nums[1] ?("."+nums[1] ):'',//小数部分
        result = '';
    while (n.length > 3) {
        result = ',' + n.slice(-3) + result;
        n = n.slice(0, n.length - 3);
    }
     if (n&&n!="-") { result = n + result; }
     if(n=="-"){result = n + result.slice(1);}
     console.log(result+decimal);
    return result+decimal;
} 
console.time('计时');
test1(98765123456.789);
console.timeEnd('计时');//计时：1.608ms,是有点久
```


所以又找了正则的写法，陷入深深的对正则表达式恐惧的痛苦中：

```js
//正则法1
function test2(num){
    if(!num) return;
    var num = num.toString();
    //大概还可以不用判断的简写，今日头痛，下次再说 debugger;
    var reg = num.indexOf('.')!=-1?(/(\d)(?=(\d{3})+\.)/g):(/(\d)(?=(\d{3})+\b)/g);
    return num.replace(reg,function($0,$1,$2,$3,$4){
        console.log($0,$1,$2,$3,$4);//这个console用于理解下文的正向肯定预查
            return $1+',';//其实return $0+',';也是一样的
        });
}
//或者使用ECMAScript提供的特殊的字符序列，自测来看该方法似乎耗时最少
function test3(num){
    if(!num) return;
    var num = num.toString();
    var reg = num.indexOf('.')!=-1?(/(\d)(?=(\d{3})+\.)/g):(/(\d)(?=(\d{3})+\b)/g);
   return num.replace(reg,'$1,');
}
```

（<<javascript高级程序设计（第三版）>>5.6.3String类型--字符串的模式匹配方法）replace第二个参数可以是函数，在只有一个匹配项的情况下，会向这个函数传递三个参数：模式匹配项、模式匹配项在字符串的中的位置和原始字符串；在正则表达式定义了多个捕获组的情况下，传递给函数的参数依次是模式匹配项、第一个捕获组的匹配项、第二个捕获组的匹配项...但最后两项仍然是模式匹配项在字符串中的位置和原始字符串。

方法1中给replace的第二个参数函数传递的参数$0是匹配项，$1是第一个捕获组(\d),该方法中的模式后面是一个正向肯定预查(?=pattern)，正向肯定预查在任何匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说该匹配不需要获取供以后使用，所以我们拿到的模式匹配项$0跟其实就是第一个捕获组$1的值；预查不消耗字符串，即下一次匹配是从最后一次匹配之后立即搜索，而不是从包含预查的字符串之后搜索。

举例说明：

```js
//调用上面正则方法2
test2(123456789.568);//转为"123,456,789.568"
//控制台输出两次,分别对应
//$0(模式匹配项),$1(第一个捕获组),$2(第二个捕获组即(\d{3})，虽然(?=)是非获取匹配，但是内部的捕获组还是要的),$3(模式匹配项在字符串中的位置),$4(原始字符串)
//3 3 789 2 123456789.568
//6 6 789 5 123456789.568
```

关于return方法2提到的ECMAScript提供的特殊的字符序列，包括：

$&:匹配整个模式的子字符串，等同于RegExp.lastMatch;
$':匹配的子字符串之前的子字符串。等同于RegExp.leftContext;
$`:匹配的子字符串之后的子字符串。等同于RegExp.rightContext;
$n:匹配第n个捕获组的子字符串，n取值0-9；
$nn:匹配第nn个捕获组的子字符串，n取值01-99；

最后，喜闻乐见的耗时研究,有其他因素影响，不一定准确：

```js
console.time('test1计时');
test1(123456789.568);
console.timeEnd('test1计时');//test1计时: 2.475ms

console.time('test2计时');
test2(123456789.568);
console.timeEnd('test2计时');//test2计时: 2.902ms，比上面那个还久 何必装逼呢

console.time('test3计时');
test3(123456789.568);
console.timeEnd('test3计时');//test3计时: 0.382ms，不错
```

//今儿个累了，先去提升点多巴胺，明日再学正则表达式

正则表达式大体看了下，上例用到的是零宽度正向肯定预查(?=exp)，零宽断言包括：

(?=exp):正向肯定预查，前瞻，断言自身出现的位置后面能匹配表达式exp,不会被捕获；
(?!exp):正向否定预查，前瞻，断言自身出现的位置后面不能匹配表达式exp，不会被捕获；
(?<=exp):反向肯定预查，后顾，断言自身出现的位置前面能匹配表达式exp，不会被捕获；
(?<
!exp):反向否定预查，后顾，断言自身出现的位置前面不能匹配表达式exp，不会被捕获；

个人经常忽略的几种写法：.:匹配除\n之外的任意字符；[abc]:匹配包含的任意一个字符；[^xyz]:排除型字符集合；
排除型的写法可用于获取url查询字符串：

```js
//获取URL查询字符串
function getUrlParam(url){
    url = !!url?url:window.location.href;
    var obj = {};
    var search = url.substring(url.indexOf('?')+1);
    var reg = /([^?&=]+)=([^?&=]*)/g;
    search.replace(reg,function($0,$1,$2){
            var para = $1;
            var val = $2;
            obj[para] = String(val);
            return $0;
        });
        return obj;
}
```

正则表达式中包含能接受重复的限定符时，通常的行为是匹配尽可能多的字符，例如正则表达式/a.*b/去搜素字符串'aabab'，会匹配整个字符串'aabab'，这称为贪婪匹配；如果们希望得到尽可能少的匹配字符，比如'aab',只要在限定符*后面加上?，就会匹配'aab'和'ab'。

当问号?紧跟在任何一个其他限定符(*,+,?,{n},{n,},{n,m})后面时，匹配模式就会变成非贪婪的,匹配会尽可能少的重复。

最后，正则表达式匹配IP的写法：((25[0-5]|2[0-4]\d|[01]?\d\d?)\.){3}(25[0-5]|2[0-4]\d|[01]?\d\d?).


