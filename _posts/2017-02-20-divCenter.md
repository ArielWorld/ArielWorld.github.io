---
layout: post
title:  "div居中问题"
date:   2017-02-20
excerpt: ""
tag:
- div center
comments: true
---

无聊时候，一个关于div居中的例子。粘贴即可运行。

```
<html>
<head>
<style>
#father{
    display: -webkit-flex;
    display: flex;
    height: 800px;
    background:#ff0;
}
#center{
    background:#f00;
    margin:auto;/*该方法无需确定div的宽高*/
}
</style>
</head>
<body>
<div id="father">
<div id="center">不定宽高div居中，为什么宽高不确定呢，因为总有这种变态的面试题出现~~如果我打很长的一段话呢？长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长长</div>
</div>
</body>
</html>
```


```
/*flex布局*/
.container{
    display:flex;
    align-items:center;
    justify-content:center;
}
.container div{
    width:200px;
    height:100px;
    background:#ff0;
}
/*或者利用transform*/
div{
    position:relative;
    width:200px;
    height:200px;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);
}
```
