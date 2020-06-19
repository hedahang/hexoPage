---
title: content换行符与打点loading效果实例页面
date: 2020-06-19 11:31:10
categories: JS
tags:
	- JS  
---

代码如下：

### CSS
``` css
dot {
    display: inline-block; 
    height: 1em;
    line-height: 1;
    text-align: left;
    vertical-align: -.25em;
    overflow: hidden;
}
dot::before {
    display: block;
    content: '...\A..\A.';
    white-space: pre-wrap;
    animation: dot 3s infinite step-start both;
}
@keyframes dot {
    33% { transform: translateY(-2em); }
    66% { transform: translateY(-1em); }
}
```
### HTML
``` html
<div class="wrap">正在加载中<dot>...</dot></div>
```