---
layout: post
title:  "js实现千位分隔符引发的惨案"
date:   2016-08-30
excerpt: ""
tag:
- test
comments: true
---

最开始参考网上写的特挫的方法如下，毫无疑问被嘲笑，可是写写又不犯法：

```js
function test(num) {
    var num = num.toString();
    var decimal = !!num.split('.')[1] ?("."+num.split('.')[1] ):'';//小数部分
    var n = num.split('.')[0] || 0, result = '';
   while (n.length > 3) {
        result = ',' + n.slice(-3) + result;
        n = n.slice(0, n.length - 3);
    }
     if (n&&n!="-") { result = n + result; }
     if(n=="-"){result = n + result.slice(1);}
    return result+decimal;
} alert(test(123456.789));
```


所以又找了正则的写法

```js
//正则法
function thousandBitSeparator(num) {
  return num && (num
    .toString().indexOf('.') != -1 ? num.toString().replace(/(\d)(?=(\d{3})+\.)/g, function($0) {
      return $0 + ",";
    }) : num.toString().replace(/(\d)(?=(\d{3})+\b)/g, function($0) {
      return $0 + ",";
    }));
}
```

