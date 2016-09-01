---
layout: post
title:  "纯css实现元素逐行显示并loading"
date:   2016-08-31
excerpt: ""
tag:
- markdown 
- sample
- 逐行显示
- loading
comments: true
---

## 没有标题

产品需求之一，希望用户信息可以逐行加载，加载完成打钩号，加载期间下方显示loading图标，转圈圈啊转圈圈。

html代码：

```html
<div class="loader">  
    <p>西安 — 三门峡 <span></span></p> 
    <p>2016-07-18 周六<span></span> </p> 
    <p>K245   14:00发 <span></span></p>
    <p>张三、李四、王五 <span></span></p>
</div> 
```

css代码：

```css
@font-face {
            font-family: "iconfont";
            src: url("http://css.40017.cn/cn/train/mobileqq/160107/css/iconfont.eot");
            src: url("http://css.40017.cn/cn/train/mobileqq/160107/css/iconfont.eot?") format("embedded-opentype"),url("http://css.40017.cn/cn/train/mobileqq/160107/css/iconfont.ttf") format("truetype"),url("http://css.40017.cn/cn/train/mobileqq/160107/css/iconfont.svg") format("svg");
        }
span{display: inline-block;position:absolute;width:48px;height:48px;left:222px;-webkit-animation:suc 1s ease both;font-family:iconfont;overflow:hidden}
span:before{content:"\e617";font-size:22px;line-height:22px;color:#000;top:0;position:absolute;}
p{-webkit-animation: loader 1s  ease both;}
.loader p:nth-child(2),.loader p:nth-child(2) span {  
    -webkit-animation-delay: 1s;  
}  
.loader p:nth-child(3),.loader p:nth-child(3) span  {  
    -webkit-animation-delay: 2s; 
}  
.loader p:nth-child(4),.loader p:nth-child(4) span  {  
    -webkit-animation-delay: 3s; 
}  
 
@-webkit-keyframes suc{0%{width:0}
100%{width:48px}}
@-webkit-keyframes loader{0%{visibility:hidden;}
100%{visibility:visible;}}
```

转圈圈的html:

```html
<div class="sk-fading-circle">
  <div class="sk-circle sk-circle1"></div>
  <div class="sk-circle sk-circle2"></div>
  <div class="sk-circle sk-circle3"></div>
  <div class="sk-circle sk-circle4"></div>
  <div class="sk-circle sk-circle5"></div>
  <div class="sk-circle sk-circle6"></div>
  <div class="sk-circle sk-circle7"></div>
  <div class="sk-circle sk-circle8"></div>
  <div class="sk-circle sk-circle9"></div>
  <div class="sk-circle sk-circle10"></div>
  <div class="sk-circle sk-circle11"></div>
  <div class="sk-circle sk-circle12"></div>
</div>
```

css如下，开始转圈圈吧：

```css
.sk-fading-circle {
  width: 40px;
  height: 40px;
  position: relative;
}
.sk-fading-circle .sk-circle {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
}
.sk-fading-circle .sk-circle:before {
  content: '';
  display: block;
  margin: 0 auto;
  width: 15%;
  height: 15%;
  background-color: #333;
  border-radius: 100%;
  animation: sk-circleFadeDelay 1.2s infinite ease-in-out both;
}
@-webkit-keyframes sk-circleFadeDelay {
  0%, 39%, 100% { opacity: 0; }
  40% { opacity: 1; }
}
@keyframes sk-circleFadeDelay {
  0%, 39%, 100% { opacity: 0; }
  40% { opacity: 1; } 
}
.sk-circle2 { transform: rotate(30deg);}
.sk-circle3 { transform: rotate(60deg);}
.sk-circle4 { transform: rotate(90deg);}
.sk-circle5 { transform: rotate(120deg);}
.sk-circle6 { transform: rotate(150deg);}
.sk-circle7 { transform: rotate(180deg);}
.sk-circle8 { transform: rotate(210deg);}
.sk-circle9 { transform: rotate(240deg);}
.sk-circle10 { transform: rotate(270deg);}
.sk-circle11 { transform: rotate(300deg);}
.sk-circle12 { transform: rotate(330deg);}
.sk-fading-circle .sk-circle2:before {animation-delay: -1.1s; }
.sk-fading-circle .sk-circle3:before { animation-delay: -1s; }
.sk-fading-circle .sk-circle4:before { animation-delay: -0.9s; }
.sk-fading-circle .sk-circle5:before {animation-delay: -0.8s; }
.sk-fading-circle .sk-circle6:before { animation-delay: -0.7s; }
.sk-fading-circle .sk-circle7:before { animation-delay: -0.6s; }
.sk-fading-circle .sk-circle8:before {animation-delay: -0.5s; }
.sk-fading-circle .sk-circle9:before { animation-delay: -0.4s; }
.sk-fading-circle .sk-circle10:before { animation-delay: -0.3s; }
.sk-fading-circle .sk-circle11:before {animation-delay: -0.2s; }
.sk-fading-circle .sk-circle12:before { animation-delay: -0.1s; }
```