---
layout: post
title:  "先捕获还是先冒泡"
date:   2017-02-20
excerpt: ""
tag:
- Bubble
- capturing
comments: true
---

无聊时候的一段小例子，了解冒泡和捕获用。粘贴即可运行。

```
<html>
<head>
<!--右边div设置margin-left负值，会被左div覆盖；右div设置left负值，可覆盖左div,包括border-->
<style>
#one{position:absolute;/*position必须为absolute，否则margin不起作用*/
    margin:auto;
    top:0;
    left:0;
    bottom:0;
    right:0;width:500px;height:500px;background: #000; }
#two{position:absolute;/*position必须为absolute，否则margin不起作用*/
    margin:auto;
    top:0;
    left:0;
    bottom:0;
    right:0;width: 400px;height: 400px;background: #fff;}
#three{position:absolute;/*position必须为absolute，否则margin不起作用*/
    margin:auto;
    top:0;
    left:0;
    bottom:0;
    right:0;width: 300px;height: #300px;background: #f00;}
#four{position:absolute;/*position必须为absolute，否则margin不起作用*/
    margin:auto;
    top:0;
    left:0;
    bottom:0;
    right:0;width: 100px;height:100px;background: #c0c0c0;}
</style>
</head>
<body>
<div id='one'>  
<span style="white-space:pre">  </span><div id='two'>  
        <div id='three'>  
            <div id='four'>  
            </div>  
        </div>  
    </div>  
</div>
<script type='text/javascript'>  
    var one=document.getElementById('one');  
    var two=document.getElementById('two');  
    var three=document.getElementById('three');  
    var four=document.getElementById('four');
    //任何发生在w3c事件模型中的事件，首先进入捕获阶段，直到达到目标元素，再进入冒泡阶段。addEventListener第三个参数是true,表示在捕获阶段调用事件处理程序，false表示在冒泡阶段调用事件处理程序。
    /*//false是冒泡 ，eg:点击four，输出顺序为four,three,two,one; 
    //从被点击的子元素开始执行，一直冒泡到最后一个父元素，执行各父元素的点击事件
    one.addEventListener('click',function(){  
        alert('one');  
    },false);  
    two.addEventListener('click',function(){  
        alert('two');  
    },false);  
    three.addEventListener('click',function(){  
        alert('three');  
    },false);  
    four.addEventListener('click',function(){  
        alert('four');  
    },false); */

    /*//true是捕获 ，eg:点击four，输出顺序为one,two,three,four; 从根元素开始捕获，一直往下捕获各子元素的点击事件
    one.addEventListener('click',function(){  
        alert('one');  
    },true);  
    two.addEventListener('click',function(){  
        alert('two');  
    },true);  
    three.addEventListener('click',function(){  
        alert('three');  
    },true);  
    four.addEventListener('click',function(){  
        alert('four');  
    },true);  */
    /*//true是捕获，false是冒泡 ，既有捕获又有冒泡,依然按照先捕获再冒泡的顺序执行
    //eg:点击four，输出顺序为one,three,four,two; 从根元素开始捕获，有捕获事件的元素先执行捕获事件，
    //到达目标元素再往上执行冒泡事件
    one.addEventListener('click',function(){  
        alert('one');  
    },true);  
    two.addEventListener('click',function(){  
        alert('two');  
    },false);  
    three.addEventListener('click',function(){  
        alert('three');  
    },true);  
    four.addEventListener('click',function(){  
        alert('four');  
    },false);  */
   /* //true是捕获，false是冒泡 ，既有捕获又有冒泡,依然按照先捕获再冒泡的顺序执行
    //eg:点击four，输出顺序为two,four,three,one; 从根元素开始捕获，有捕获事件的元素先执行捕获事件，
    //到达目标元素再往上执行冒泡事件
    one.addEventListener('click',function(){  
        alert('one');  
    },false);  
    two.addEventListener('click',function(){  
        alert('two');  
    },true);  
    three.addEventListener('click',function(){  
        alert('three');  
    },false);  
    four.addEventListener('click',function(){  
        alert('four');  
    },true);  */
     /*//同一dom元素既有捕获又有冒泡
    //eg:点击four，输出顺序为two先写捕获,four,three,two后写冒泡，one; 从根元素开始捕获，有捕获事件的元素先执行捕获事件，
    //到达目标元素再往上执行冒泡事件
    //若是点击two,输出为two先写捕获，two后写冒泡，one
    one.addEventListener('click',function(){  
        alert('one');  
    },false);  
    two.addEventListener('click',function(){  
        alert('two先写捕获');  
    },true);
    two.addEventListener('click',function(){  
        alert('two后写冒泡');  
    },false);  
    three.addEventListener('click',function(){  
        alert('three');  
    },false);  
    four.addEventListener('click',function(){  
        alert('four');  
    },true);  */
     //同一dom元素既有捕获又有冒泡，冒泡和捕获换个顺序
    //eg:点击four，输出顺序为two后写捕获,four,three,two先写冒泡，one; 从根元素开始捕获，有捕获事件的元素先执行捕获事件，
    //到达目标元素再往上执行冒泡事件
    //若是点击two,输出为two先写冒泡，two后写捕获，one；two的事件是按顺序执行的，而不是先执行捕获再执行冒泡
    one.addEventListener('click',function(){  
        alert('one');  
    },false);
     two.addEventListener('click',function(){  
        alert('判断是否按顺序执行而不是先捕获');  
    },false);    
    two.addEventListener('click',function(){  
        alert('two先写冒泡');  
    },false);  
    two.addEventListener('click',function(){  
        alert('two后写捕获');  
    },true);
    three.addEventListener('click',function(){  
        alert('three');  
    },false);  
    four.addEventListener('click',function(){  
        alert('four');  
    },true);  
</script>    
</body>
</html>
```

