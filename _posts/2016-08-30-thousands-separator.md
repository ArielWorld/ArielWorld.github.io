---
layout: post
title:  "js实现千位分隔符"
date:   2016-08-30
excerpt: ""
tag:
- test
comments: true
---

## 一个测试
```js
function test(num) {
    var num = num.toString();
    var decimal = "."+num.split('.')[1] || '';//小数部分
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

