---
layout: post
title:  "数据类型转换"
date:   2017-02-19
excerpt: ""
tag:
- 数据类型转换
comments: true
---

突然想到的之前校招时一个简单的面试题，关于运算时字符串是否会转为Number，当时以为能转为Number型的肯定会转，现在想到要分运算符的，查了一下，阮一峰早就写过相关博客了，男神果然是男神。
当JavaScript遇到预期为字符串的地方，就会将非字符串的数据自动转为字符串。系统内部会自动调用String函数。
字符串的自动转换，主要发生在加法运算时。当一个值为字符串，另一个值为非字符串，则后者转为字符串。

```js
"12"/"3"//4
"12"*"3"//36
"12"-"3"//9
12+3//15
//上面的运算会将字符串类型转为Number进行运算
"12"+3//"123"
12+"3"//"123"
"12"+"3"//"123"
```

